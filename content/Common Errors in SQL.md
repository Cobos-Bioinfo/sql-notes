**Date**: 2026-08-22
**Tags**: [[SQL]]

1. **Aliases (`AS`) can't be used in `WHERE`**
	This errors in [[PostgreSQL]], [[MySQL]] and [[Miscrosoft SQL Server]]:

```SQL
--- ERROR
SELECT developer, COUNT(*) AS total
FROM videogames
WHERE total > 1      -- 'total' does not exist when 'WHERE' is executed
GROUP BY developer
```
Alternative: Use the full expression or `HAVING`
```SQL
--- CORRECT
SELECT developer, COUNT(*) AS total
FROM videogames
GROUP BY developer
HAVING COUNT(*) > 1;
```

>[!attention] Attention with [[SQLite]]:
>SQLite is **more permissive** than the standard. It allows to use aliases during `WHERE` (and `HAVING`). Most other SQL engines (and the standard itself) do not. Always rely on the mental model of [[Execution Order in SQL]]. This will always work regardless of the engine.

2. **You can't use [[Aggregation Functions#Available Functions]] in `WHERE`**
	Data Aggregation Functions (`COUNT()`, `SUM()`, `AVG()`) need groups to exist. 

```SQL
--- ERROR
SELECT developer, COUNT(*) AS total
FROM videogames
WHERE COUNT(*) > 1       -- no data agg functions allowed in WHERE
GROUP BY developer
```
Alternative: Use `HAVING` to filter aggregation results:
```SQL
--- CORRECT
SELECT developer, COUNT(*) AS total
FROM videogames
GROUP BY developer
HAVING COUNT(*) > 1;
```

*Similar to `WHERE` being executed **before** `GROUP BY` and therefore not having access to said groups.*


**You CAN use Aliases during `ORDER BY`**
```SQL
SELECT developer, COUNT(*) AS total
FROM videogames
GROUP BY developer
ORDER BY total DESC;       -- This works since ORDER BY (step 6) is                                       executed after SELECT (step 5) 
```


---
# References
[AprendeSQL from midudev - Orden de Ejecucion - Errores Comunes](https://www.aprendesql.dev/orden-de-ejecucion#errores-comunes-por-el-orden-de-ejecuci%C3%B3n)
