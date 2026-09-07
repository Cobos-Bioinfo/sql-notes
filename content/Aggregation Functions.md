**Date**: 2026-08-22
**Tags**: [[SQL]], [[Numeric Functions in SQL]]

## Available Functions:
| Function | Description                  | Example               |
| -------- | ---------------------------- | --------------------- |
| `COUNT`  | **Count** number of **rows** | `COUNT(*)`            |
| `SUM`    | **Sum column** values        | `SUM(playtime_hours)` |
| `AVG`    | Compute average              | `AVG(playtime_hours)` |
| `MIN`    | Return Minimum value         | `MIN(year)`           |
| `MAX`    | Return Maximum value         | `MAX(year)`           |
>[!example]- Example: `COUNT` total videogames:
>```PostgreSQL
>SELECT COUNT(*) AS total FROM videogames;
>```

**`COUNT(*)` vs `COUNT(column)`**
Small but important distinction. Common technical interview question.
- `COUNT(*)`: Counts **all** records (rows), regardless if `NULL` values are present or not.
- `COUNT(column)`: Counts **only non-`NULL`** records.

## Combining Functions
Data Aggregation functions can be combined in a single query:
```PostgreSQL
SELECT
  COUNT(*) AS total,
  MIN(year) AS first_year,
  MAX(year) AS last_year
FROM videogames;
```

## `GROUP_CONCAT`: Concatenate Values
`GROUP_CONCAT`: Combines values from a **column** into a single **comma-separated string**. 
- Common to visualize all elements from a group:

```PostgreSQL
SELECT developer, GROUP_CONCAT(title) AS games
FROM videogames
GROUP BY developer;
```
This query would return something like: `Nintendo` → `The Legend of Zelda,Super Mario Bros,...`
*Passing a second argument to `GROUP_CONCAT()` changes the separator: `GROUP_CONCAT(title, ' | '*.

## NULL and Aggregation Functions
[[NULL Handling in SQL]]

`SUM`, `AVG`, `MIN` and `MAX` **ignore** **`NULL`** values by default.

>[!example]- Example: Computing `AVG` with `NULL` values:
>If there are **10** records but **3** are `NULL`, the average will be computed over the **7** records with data.
>
>*This is usually the desired behavior but good to keep in mind.*



---
# References
[AprendeSQL from midudev - Funciones de Agregacion](https://www.aprendesql.dev/funciones-de-agregacion)
