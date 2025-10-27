```
fun existsByUserId(userId: Long): Boolean =
    entityManager.createNativeQuery(
        "SELECT TOP 1 1 FROM users_aura_membership_details WHERE user_id = :userId"
    )
    .setParameter("userId", userId)
    .resultList
    .isNotEmpty()
```

```
CREATE NONCLUSTERED INDEX IX_users_aura_membership_details_user_id
ON dbo.users_aura_membership_details (user_id);
```