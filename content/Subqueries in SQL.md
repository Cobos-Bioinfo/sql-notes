**Date**: 2026-08-31
**Tags**: [[SQL]], [[Query DataBases]]

**Subqueries** are **`SELECT` queries** inside a different SQL query. 
- They are written in between **parenthesis** `( )`
- They are **executed first**

## **`WHERE`** Subqueries (`=`, `>`, `<`)

Most commonly use for subqueries are inside a `WHERE` to dynamically compute a value
- Will return a **single value** (**Scalar subquery**)

```SQL
SELECT title, year
FROM videogames
WHERE year = (SELECT MAX(year) FROM videogames);
```

>[!done]- Result:
>
>|title|year|
|---|---|
|Red Dead Redemption 2|2018|
|God of War|2018|
>

The subquery first returns `2018`
Then, the main query will return all games and year (2018) from that year

## **`IN`** Subqueries

When the subquery returns **multiple records**. Use **`IN`** instead of `=`

```SQL
SELECT title, developer
FROM videogames
WHERE developer IN (
  SELECT developer FROM videogames
  GROUP BY developer
  HAVING COUNT (*) > 1
);
```

>[!done]- Result:
>|title|developer|
|---|---|
|The Legend of Zelda|Nintendo|
|Super Mario Bros.|Nintendo|
|Grand Theft Auto V|Rockstar Games|
|Red Dead Redemption 2|Rockstar Games|
>
The subquery first returns all developers with more than one game
Then, the main query will return game and developer from those developers

## **`SELECT`** Subqueries

You can use a subquery as a **computed column** with **`SELECT`**

```SQL
SELECT title,
       playtime_hours,
       (SELECT MAX(playtime_hours) FROM videogames) AS max_hours
FROM videogames;
```

>[!done]- Result
>|title|playtime_hours|max_horas|
|---|---|---|
|The Legend of Zelda|20|9999|
|Super Mario Bros.|10|9999|
|Final Fantasy VII|40|9999|
|The Witcher 3|100|9999|
>| ... | ... | ... |
>
>Every record will show its own playtime hours with the maximum.

## **`EXISTS`** and **`NOT EXISTS`**

`EXISTS` checks if a subquery returns **at least one record**.
- Unlike `IN`, it does not compare values. It checks they exist

```SQL
SELECT title
FROM videogames v
WHERE EXISTS (
  SELECT 1
  FROM videogames v2
  WHERE v2.developer = v.developer
    AND v2.id != v.id
);
```

This query returns games whose developer has at least another game in the table.
- The subquery uses `SELECT 1` because it does not matter what value it returns, simply if there are records or not

`NOT EXISTS` does the opposite: Returns those records where the subquery does **NOT find results**

```SQL
SELECT title, developer
FROM videogames v
WHERE NOT EXISTS (
  SELECT 1
  FROM videogames v2
  WHERE v2.developer = v.developer
    AND v2.id != v.id
);
```

This query will return games whose developer only has 1 game in the table.

>[!tip]- Tip: Most **subqueries** can be rewritten as `JOIN`
>Use subqueries when the logic is clearer or you need an aggregate value to compare


---
# References
[AprendeSQL from midudev - subconsultas](https://www.aprendesql.dev/subconsultas)
