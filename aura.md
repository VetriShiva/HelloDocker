```
fun existsByUserId(userId: Long): Boolean =
    entityManager.createNativeQuery(
        "SELECT TOP 1 1 FROM users_aura_membership_details WHERE user_id = :userId"
    )
    .setParameter("userId", userId)
    .resultList
    .isNotEmpty()
```