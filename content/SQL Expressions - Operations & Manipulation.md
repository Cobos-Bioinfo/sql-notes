**Date**: 2026-08-21
**Tags**: [[SQL]]

## Arithmetic Operations in SQL

**Arithmetic** operators can be used during **`SELECT`**:
```SQLite
SELECT title, CAST(strftime('%Y', 'now') AS INTEGER) - year AS age
FROM videogames;
```
In this example:
- `strftime('%Y', 'now')` returns the **current (`'now'`)** **year (`'%Y'`)** **as text** and `CAST(... AS INTEGER)` turns it into **integer** to be subtracted.  
		This `strftime()` command is specific to [[SQLite]]

## Text Concatenation in SQL

**SQLite** uses `||` to concatenate **text strings**:
```SQLite
SELECT v.title || ' (' || v.year || ')' AS info
FROM videogames v;
```


## CASE WHEN: Conditional Logic in SQL

**`CASE WHEN`** allows creating columns with **values based in conditions**, similar to an `if/else`:

```SQLite
SELECT title,
	CASE
		WHEN year < 2000 THEN 'Classic'
		WHEN year < 2015 THEN 'Modern'
		ELSE 'Recent'
	END AS category
FROM videogames;
```

>[!example]- Example from AprendeSQL:
>```SQL
>SELECT v.title, v.playtime_hours, 
 > CASE
 >   WHEN v.playtime_hours < 20 THEN 'Casual'
 >   WHEN v.playtime_hours >= 20 AND v.playtime_hours <= 60 THEN 'Normal'
 >   WHEN v.playtime_hours > 60 THEN 'Hardcore'
 >   ELSE NULL
 > END AS dedicacion
>FROM videogames v;
>```
## Alias (`AS`) in SQL

**`AS`** allows to give a custom name to a computed column. Already seen before.

## Text Functions

| Function                            | Description                 | Example                                            |
| ----------------------------------- | --------------------------- | -------------------------------------------------- |
| `UPPER(texto)`                      | Uppercase                   | `UPPER('hola')` → `'HOLA'`                         |
| `LOWER(texto)`                      | Lowercase                   | `LOWER('HOLA')` → `'hola'`                         |
| `LENGTH(texto)`                     | Return length of string     | `LENGTH('Zelda')` → `5`                            |
| `SUBSTR(texto, inicio, longitud)`   | Return a Substring          | `SUBSTR('Minecraft', 1, 4)` → `'Mine'`             |
| `TRIM(texto)`                       | Trim whitespaces both sides | `TRIM(' hola ')` → `'hola'`                        |
| `REPLACE(texto, buscar, reemplazo)` | Replace                     | `REPLACE('2015-03-20', '-', '/')` → `'2015/03/20'` |
>[!example]- Example: `UPPER` videogame titles:
>```SQL
>SELECT UPPER(title) AS uppercase_title FROM videogames;
>```

## Numeric Functions

| Function               | Description                      | Example                      |
| ---------------------- | -------------------------------- | ---------------------------- |
| `ROUND(int, decimals)` | Round to specified decimal point | `ROUND(3.14159, 2)` → `3.14` |
| `ABS(int)`             | Return Absolute value            | `ABS(-42)` → `42`            |
| `MIN(a, b)`            | Return smallest from a, b        | `MIN(3, 5)` → `3`            |
| `MAX(a, b)`            | Return biggest from a, b         | `MAX(3, 5)` → `5`            |
>[!example]- Example: `ROUND` monthly hours:
>```SQL
>SELECT title, ROUND(playtime_hours * 1.0 / 12, 1) AS monthly_hours
>FROM videogames;
>```






---
# References
[AprendeSQL from midudev - Expresiones en SQL: Cálculos y manipulaciones](https://www.aprendesql.dev/expresiones-en-sql)
