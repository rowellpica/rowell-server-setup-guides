## Check Databases Size
```
SELECT table_schema AS "dbname", ROUND(SUM(data_length + index_length) / 1024 / 1024, 1) AS "dbsize"
FROM information_schema.tables
GROUP BY table_schema;
```