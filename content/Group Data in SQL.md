**Date**: 2026-08-22
**Tags**: [[SQL]]

**`GROUP BY`** groups records that share the same values in a column. This allows [[Aggregation Functions]] functions to be applied to each group.

```SQL
SELECT developer, COUNT(*) AS total
FROM videogames
GROUP BY developer;
```
*This query will display how many games each developer has.*

**`HAVING`** behaves like `WHERE` but **filters groups** instead of individual records. 
- Is is used **after** `GROUP BY`.

```SQL
SELECT developer, COUNT(*) AS total
FROM videogames
GROUP BY developer
HAVING COUNT(*) > 1;
```

>[!tip] Tip: You could also use **total** instead of `COUNT(*)` in the final `HAVING`

## **`WHERE`** vs **`HAVING`**

- `WHERE` filters records **before** grouping
- `HAVING` filters records **after** grouping

```SQL
SELECT developer, COUNT(*) AS total
FROM videogames
WHERE year > 2000        -- Filter records
GROUP BY developer       -- Groups
HAVING COUNT(*) > 1;     -- Filters groups
```

## **`GROUP BY`** Multiple Columns

`GROUP BY` can be used to obtain more specific combinations:
```SQL
SELECT developer, year, COUNT(*) AS total
FROM videogames
GROUP BY developer, year
```
*This will create groups for each unique combination of **developer** and **year***

---
# References
[AprendeSQL from midudev - Agroupando datos](https://www.aprendesql.dev/agrupando-datos)
