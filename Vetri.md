Excellent — you’re focusing on concurrent delete requests coming from two users hitting the same endpoint at the same time.
Here’s how to make your Spring Boot + Kotlin + MSSQL service production-grade and deadlock-resilient at the application level (not just DB side).
⚙️ 1. Enforce Transaction Isolation and Idempotence
Two users deleting the same entity (or related entities) can deadlock if both sessions hold conflicting locks.
So, combine these safeguards:
Copy code
Kotlin
@Transactional(isolation = Isolation.READ_COMMITTED)
fun deleteEntity(id: Long) {
    val entity = repository.findById(id)
        .orElseThrow { EntityNotFoundException("Record $id not found") }

    // Defensive: soft-delete first
    entity.status = "DELETED"
    repository.save(entity)

    // Optionally trigger async purge later
}
✅ Best Practices:
Always fetch and lock in a deterministic order (e.g., by primary key).
Avoid modifying multiple unrelated entities inside one transaction.
Mark methods @Transactional at the service layer (not controller).
🔁 2. Handle Deadlocks Gracefully with Retries
Deadlocks can still happen even in perfect code, so always add a retry policy.
Copy code
Kotlin
@Configuration
@EnableRetry
class RetryConfig

@Service
class DeleteService(
    private val repo: ParentRepository
) {

    @Retryable(
        include = [DeadlockLoserDataAccessException::class],
        maxAttempts = 4,
        backoff = Backoff(delay = 200, multiplier = 2.0, random = true)
    )
    @Transactional
    fun deleteParent(id: Long) {
        repo.deleteById(id)
    }
}
Retries happen automatically for transient deadlocks.
Each retry gets a random backoff to prevent thundering-herd collisions.
🔒 3. Use Pessimistic Locking When Needed
If both users might delete or update the same record, explicitly lock the row.
Copy code
Kotlin
@Repository
interface AccountRepository : JpaRepository<AccountEntity, Long> {

    @Lock(LockModeType.PESSIMISTIC_WRITE)
    @Query("SELECT a FROM AccountEntity a WHERE a.id = :id")
    fun findByIdForUpdate(@Param("id") id: Long): AccountEntity?
}
Then:
Copy code
Kotlin
@Transactional
fun deleteSafely(id: Long) {
    val entity = accountRepository.findByIdForUpdate(id)
        ?: throw EntityNotFoundException("Account not found")
    accountRepository.delete(entity)
}
✅ Guarantees:
Only one transaction can hold a delete lock per record — the other waits or times out instead of deadlocking.
📦 4. Short-Lived, Narrow Transactions
Never include external calls (HTTP, Kafka, file writes) inside the same transaction.
→ Commit DB first, then fire async tasks.
Keep each delete atomic: fetch → delete → commit, not long chains.
🧩 5. Use Version Columns for Optimistic Locking (if suitable)
When deletion depends on the latest state:
Copy code
Kotlin
@Entity
class AccountEntity(
  @Id val id: Long,
  @Version val version: Int,
  var status: String
)
Then:
Copy code
Kotlin
@Transactional
fun deleteWithVersion(id: Long) {
    val entity = repo.findById(id).orElseThrow()
    repo.delete(entity) // Will throw OptimisticLockException if another tx modified it
}
✅ Benefit: second user gets an immediate version conflict instead of waiting on locks.
🪶 6. Use Soft Delete + Async Purge
If deletes are business-critical and frequent:
Mark record as deleted (is_deleted = true).
Push record ID to a background queue (Kafka, Job Scheduler, Quartz).
Actual DB DELETE runs in low-traffic batch with retry pattern.
Copy code
Kotlin
data class DeleteEvent(val id: Long)

@Service
class DeleteProcessor(private val repo: AccountRepository) {
    @Transactional
    fun purge(event: DeleteEvent) {
        repo.deleteById(event.id)
    }
}
🔍 7. Observability & Metrics
Log deadlock retries in WARN level (add MDC = correlationId, userId).
Add a Micrometer counter:
delete_deadlock_retries_total → visualize in Grafana.
Use AppDynamics / APM to track slow deletes, concurrent deletes/sec.
✅ Summary — Application-Side Production Practices
Category
Best Practice
Transaction
Keep small and consistent, use @Transactional at service
Retry
Use Spring Retry on DeadlockLoserDataAccessException
Locking
Pessimistic lock for same-record deletes
Ordering
Always access entities in same deterministic order
Isolation
Prefer READ_COMMITTED; use RCSI in DB
Async
Offload heavy delete to background batch
Observability
Add retry metrics, APM traces, and logs
Idempotency
Ensure repeated deletes are safe (check existence first)







Here’s a comprehensive Cucumber BDD suite that covers all realistic edge cases for “handling deadlocks gracefully with retries” in a Spring Boot + Kotlin + MSSQL service.
This design simulates two concurrent delete requests on the same record and validates retry, backoff, and final behavior outcomes.
🧩 Feature: Handle Deadlocks Gracefully with Retries
Copy code
Gherkin
Feature: Handle database deadlocks gracefully with automatic retry

  Background:
    Given the system has a Parent record with ID 1001
    And two concurrent users "UserA" and "UserB" try to delete the same record
✅ Scenario 1: Successful delete without conflict (happy path)
Copy code
Gherkin
Scenario: Single user deletes record successfully without any deadlock
    When "UserA" sends a delete request for record 1001
    Then the record should be deleted successfully
    And the system should not perform any retry
    And a success log should be written with status "DELETED"
⚔️ Scenario 2: Two users delete same record at same time — transient deadlock resolved by retry
Copy code
Gherkin
Scenario: Concurrent delete causes transient deadlock but retry succeeds
    Given "UserA" and "UserB" send delete requests for record 1001 at the same time
    When a deadlock occurs between both delete transactions
    Then the system should automatically retry the delete for one of them
    And the retry should succeed within max 3 attempts
    And total retry count should be less than or equal to 3
    And the record should be deleted successfully
    And a warning log should mention "Deadlock detected - retry attempt"
🕐 Scenario 3: Repeated deadlock until retry limit exceeded (final failure)
Copy code
Gherkin
Scenario: Retry limit reached and delete still fails
    Given a database stress condition where every delete attempt causes a deadlock
    When "UserA" sends a delete request for record 1001
    Then the system should retry the delete up to 3 times with exponential backoff
    And after the maxAttempts are exhausted
    Then the system should throw "DeadlockLoserDataAccessException"
    And a final error log should mention "Delete failed after retries"
    And the record should still exist in database
🧠 Scenario 4: Deadlock on child table — verify transactional rollback
Copy code
Gherkin
Scenario: Deadlock occurs in child table during cascade delete
    Given Parent 1001 has multiple child records in table Child
    And both "UserA" and "UserB" initiate deleteParent(1001)
    When a deadlock occurs during delete on Child table
    Then the transaction should roll back for the failed thread
    And retry should start for the failed transaction
    And after retry, both Parent and Child records should be deleted
    And rollback log should mention "Transaction rolled back due to deadlock"
🪶 Scenario 5: Deadlock detected and resolved within same transaction scope
Copy code
Gherkin
Scenario: In-transaction retry succeeds without full rollback
    Given the delete method uses @Retryable with propagation REQUIRED
    When a deadlock occurs mid-transaction
    Then Spring Retry should re-execute the delete logic within same bean context
    And retry count should increment by 1
    And final transaction commit should succeed
🧩 Scenario 6: Idempotency check — repeated delete after success
Copy code
Gherkin
Scenario: Retry after successful delete does not rethrow error
    Given "UserA" deletes record 1001 successfully
    When "UserA" re-sends delete request for record 1001 after 2 seconds
    Then the system should not fail
    And it should return status 200 with message "Already deleted"
    And no retry should occur
🚀 Scenario 7: Logging and Observability Verification
Copy code
Gherkin
Scenario: Verify logging, metrics, and retry counters during deadlock recovery
    Given a transient deadlock simulated in delete operation
    When the retry logic executes
    Then application logs should contain "Deadlock detected" and "Retry attempt #"
    And Micrometer metric "delete_deadlock_retries_total" should increment
    And tracing span "deleteEntity" should show retry count in attributes
🧪 Step Definition Hints (Kotlin + Spring Boot)
Copy code
Kotlin
@When("UserA and UserB send delete requests for record {long} at the same time")
fun concurrentDelete(recordId: Long) {
    val executor = Executors.newFixedThreadPool(2)
    val futures = listOf("UserA", "UserB").map {
        executor.submit {
            deleteService.deleteWithRetry(recordId)
        }
    }
    futures.forEach { it.get() }
}
Copy code
Kotlin
@Then("the system should automatically retry the delete for one of them")
fun verifyRetryHappened() {
    verify(logger).warn(contains("Deadlock detected - retry attempt"))
}
Copy code
Kotlin
@Then("the record should still exist in database")
fun verifyRecordStillExists() {
    assertTrue(parentRepository.existsById(1001L))
}
💡 Edge Cases Summary Table
Case
What it Tests
Expected Outcome
1
Single-user delete
No retry, success
2
Concurrent delete
Deadlock once → Retry → Success
3
Always-deadlock DB
Retry limit hit → Exception
4
Deadlock in child delete
Rollback & retry success
5
In-tx retry (same bean)
Partial retry, commit
6
Repeat delete after success
Idempotent safe delete
7
Metrics & Logging
Retry count & logs verified



Here’s a Kotlin-based concurrent delete simulation you can run in your Spring Boot test suite (JUnit 5) to reproduce and verify your concurrency fixes — especially for deadlock, retry, and lock-behavior validation.
🧪 1. Goal
Simulate two HTTP clients deleting the same record at the same time using your REST API, and observe:
Deadlock exception behavior
Retry success (if using @Retryable)
Isolation and locking behavior
🧱 2. Setup: Sample Entity + Repository
Copy code
Kotlin
@Entity
@Table(name = "account")
data class AccountEntity(
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long = 0,
    var name: String = "",
    var balance: Double = 0.0
)
Copy code
Kotlin
@Repository
interface AccountRepository : JpaRepository<AccountEntity, Long> {

    @Lock(LockModeType.PESSIMISTIC_WRITE)
    @Query("SELECT a FROM AccountEntity a WHERE a.id = :id")
    fun findByIdForUpdate(@Param("id") id: Long): AccountEntity?
}
🧩 3. Service with Retry & Transaction
Copy code
Kotlin
@Service
class AccountService(private val repo: AccountRepository) {

    @Retryable(
        include = [DeadlockLoserDataAccessException::class],
        maxAttempts = 3,
        backoff = Backoff(delay = 200, multiplier = 2.0, random = true)
    )
    @Transactional
    fun deleteAccount(id: Long) {
        val account = repo.findByIdForUpdate(id)
            ?: throw EntityNotFoundException("Account $id not found")
        repo.delete(account)
        println("Deleted account: $id by thread ${Thread.currentThread().name}")
    }
}
🌐 4. REST Controller
Copy code
Kotlin
@RestController
@RequestMapping("/accounts")
class AccountController(private val accountService: AccountService) {

    @DeleteMapping("/{id}")
    fun deleteAccount(@PathVariable id: Long): ResponseEntity<String> {
        accountService.deleteAccount(id)
        return ResponseEntity.ok("Deleted $id")
    }
}
🧵 5. Concurrency Test Simulation
Copy code
Kotlin
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@AutoConfigureMockMvc
class ConcurrentDeleteTest(
    @Autowired private val mockMvc: MockMvc,
    @Autowired private val repo: AccountRepository
) {

    @Test
    fun `simulate concurrent deletes`() {
        // 1️⃣ Prepare test data
        val account = repo.save(AccountEntity(name = "TestUser", balance = 1000.0))
        val accountId = account.id

        // 2️⃣ Create executor to simulate 2 parallel HTTP DELETE calls
        val executor = Executors.newFixedThreadPool(2)
        val latch = CountDownLatch(2)

        val deleteTask = Runnable {
            try {
                mockMvc.perform(delete("/accounts/$accountId"))
                    .andDo(print())
            } catch (ex: Exception) {
                println("Thread ${Thread.currentThread().name} exception: ${ex.message}")
            } finally {
                latch.countDown()
            }
        }

        // 3️⃣ Fire both delete requests at the same time
        repeat(2) { executor.submit(deleteTask) }

        latch.await(10, TimeUnit.SECONDS)
        executor.shutdown()
    }
}
✅ What this does
Saves a test record.
Spawns 2 threads calling the /accounts/{id} DELETE endpoint at the same time.
You’ll see one delete succeed, one fail or retry.
If your retry logic works, both threads will complete gracefully.
🔍 6. Expected Outcomes
Scenario
Behavior
Expected Result
No Locking, No Retry
Likely DeadlockLoserDataAccessException
One fails, one succeeds
Pessimistic Locking + Retry
Second waits, retries
Both succeed, no exception
Soft Delete
Both mark same record
Idempotent, no error
Hard Delete + Cascade
Higher chance of deadlock
Use batch + ordering
📊 7. Logging Tips
Enable Hibernate SQL & deadlock debug logging to trace lock behavior:
Copy code
Yaml
logging:
  level:
    org.hibernate.SQL: DEBUG
    org.hibernate.engine.jdbc.spi.SqlExceptionHelper: WARN
    org.springframework.dao.DeadlockLoserDataAccessException: DEBUG
In SQL Server, check which process deadlocked:
Copy code
Sql
SELECT * FROM sys.dm_tran_locks WHERE request_status = 'WAIT';
🧠 8. Optional: Extend to Multi-Entity or Cascade Deletes
You can easily extend the test by:
Creating a Parent → Child table relationship.
Deleting parent records while both threads are active.
Observing cascade-related deadlocks (typical real-world scenario).
Would you like me to extend this simulation to include child table cascade deletes (Parent → Child) to visualize a true deadlock graph scenario (like two users deleting parent records referencing the same children)?