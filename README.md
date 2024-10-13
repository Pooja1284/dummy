```sql
SELECT
City,
    COUNT(store_id) as Total_Stores
FROM
    dim_stores
GROUP BY
    City
ORDER BY
    Total_Stores DESC;
```

