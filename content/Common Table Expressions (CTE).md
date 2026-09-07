**Date**: 2026-09-02
**Tags**: [[SQL]]

A **CTE** (Common Table Expression) is a named **temporary query** that can be defined before your main query via `WITH`.
- It is **created** when **used**
- It stores an intermediate result to use it later

```SQL
WITH <CTE_name> AS (
  SELECT ...
)

SELECT * FROM <CTE_name>;
```

**CTE** is defined between `WITH` and `SELECT`. 

>[!question]+ Why use CTEs?
>Let's compare the same query that look for developers with more than one game:
>
>**Without CTE** (using [[Subqueries in SQL]])
>
>```SQL
>SELECT developer, total
>FROM (
>  SELECT developer, COUNT(*) AS total
>  FROM videogames
>  GROUP BY developer
>)
>WHERE total > 1;
>```
>
>**With CTE**
>
>```SQL
>WITH count AS (
>  SELECT developer, COUNT(*) AS total
>  FROM videogames
>  GROUP BY developer
>)
>
>SELECT developer, total
>FROM count
>WHERE total > 1;
>```

## Multiple CTEs

Multiple CTEs can be defined at once via comma-separation:

```SQL
WITH
large_games AS (
  SELECT title, developer, playtime_hours
  FROM videogames
  WHERE playtime_hours > 50
),
dev_count AS (
  SELECT developer, COUNT(*) AS total
  FROM videogames
  GROUP BY developer
)

SELECT g.title, g.playtime_hours, c.total
FROM large_games g
JOIN dev_count c ON g.developer = c.developer;
```

>[!tip]- **CTE** vs [[VIEWS in SQL]]
>Both define query logic, not stored data.
>- **CTE**: Lives only inside one query. Use it to tidy up or break down a single query
>- **`VIEW`**: Stored in the database and reusable anywhere. Use it when you'll want the same logic across multiple queries

**CTE**s are overall preferred to [[Subqueries in SQL]]. 

---

I made a [[Execution Order in SQL]] - [[Common Errors in SQL]] mistake during one of the practice exercises:

>[!failure] **Error**: Used an aggregate function in `WHERE`
>
>```SQL
WITH 
promedio AS (
  SELECT AVG(playtime_hours) AS avg_hours
  FROM videogames),
por_encima AS (
  SELECT * FROM videogames
  WHERE playtime_hours > AVG(playtime_hours)) -- ✗ here
  >
SELECT title, playtime_hours FROM por_encima;
>```

>[!success]- **Correct**: Compute the average separately so it is a single scalar value to be compared in the `WHERE`
>
>```SQL
>WITH
promedio AS (
SELECT AVG(playtime_hours) AS avg_hours FROM videogames
),
por_encima AS (
SELECT title, playtime_hours
FROM videogames
WHERE playtime_hours > (SELECT avg_hours FROM promedio)
)
SELECT * FROM por_encima;
>```




---
# References
[AprendeSQL from midudev - Expresiones de Tabla Comunes](https://www.aprendesql.dev/expresiones-de-tabla-comunes)
