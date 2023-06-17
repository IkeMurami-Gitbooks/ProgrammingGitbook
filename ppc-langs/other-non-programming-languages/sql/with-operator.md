# WITH operator

Позволяет бить сложные запросы на маленькие поэтапные

```sql
WITH
    "SubQuery" as (SELECT * FROM "log"),
    "SubQuery2" as (SELECT * FROM "SubQuery" GROUP BY "id")
SELECT * 
FROM "SubQuery2" 
    LEFT JOIN "SubQuery" ON "SubQuery2"."id" = "SubQuery"."id"
```
