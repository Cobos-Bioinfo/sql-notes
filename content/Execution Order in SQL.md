**Date**: 2026-08-22
**Tags**: [[SQL]]

**SQL** queries are written in a specific order while the **execution order** is different.

![[{12C742D8-5A33-4AAE-9813-1B89A02FB9AE}.png]]

*This is why:*
- You **CAN'T** use **aliases** (See [[Aliases in SQL]])  defined during `SELECT` in the `WHERE`. Because `WHERE` is **executed before** `SELECT`
- You **CAN** use **aliases** in `ORDER BY`. Because `ORDER BY` is **executed after** `SELECT`
- `HAVING` **filters groups**, not records. Because `HAVING` is **executed after** `GROUP BY`

```SQL
SELECT developer, COUNT(*) AS total     -- 5. Select columns
FROM videogames                         -- 1. Access table
WHERE year > 2000                       -- 2. Filter rows
GROUP BY developer                      -- 3. Group
HAVING COUNT(*) > 1                     -- 4. Filter groups
ORDER BY total DESC                     -- 6. Order result
LIMIT 5;                                -- 7. Limit returned rows
```

See some [[Common Errors in SQL]]

---
## Examples:

- Show the developer and number of games (alias 'total'), only for developers with more than 1 game, sorted by total in descending order.

```SQL
SELECT developer, COUNT(*) AS total
FROM videogames
GROUP BY developer
HAVING total > 1
ORDER BY total DESC;
```

- Show the year and number of games (alias 'total') published that year, only for years after 2000, with at least 2 games, sorted by descending total.

```SQL
SELECT year, COUNT(*) AS total
FROM videogames
WHERE year > 2000
GROUP BY year
HAVING total >= 2
ORDER BY total DESC;
```

---
# References
[AprendeSQL from midudev - Orden de Ejecucion](https://www.aprendesql.dev/orden-de-ejecucion)
