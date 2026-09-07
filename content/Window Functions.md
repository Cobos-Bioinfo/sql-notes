**Date**: 2026-09-04
**Tags**: [[SQL]]

**Window Functions**  compute over a group of **records** related to the current record.
- They do **NOT collapse** the result like `GROUP BY` or [[Aggregation Functions]] 
- Each record keeps their ID and gets a **computed value**

## **`OVER()`**

**Window Functions** use `OVER()` to define the "window" of records:

```SQL
SELECT title, playtime_hours,
  SUM(playtime_hours) OVER() AS total_hours
FROM videogames;
```

With `OVER()` empty, the window is the **whole table**.
- Each record will show their own `playtime_hours` AND the global total (sum).
- Unlike `SUM()` with `GROUP BY`, in this case records are **NOT grouped**. All records are shown with an **extra column**.

## **`PARTITION BY`** - Dividing Groups

`PARTITION BY` divides records in **partitions** (groups) and applies the function inside **each** group.

>[!example]- Example: Dividing groups via **`PARTITION BY`**
>
>```SQL
SELECT title, developer, playtime_hours,
  SUM(playtime_hours) OVER(PARTITION BY developer) AS dev_hours
FROM videogames;
>```
>- Each record shows their dev total hours without loosing their individual details.

## **`ORDER BY`** Inside **`OVER`** - Ranking Functions

Adding `ORDER BY` inside an `OVER()`, you can use **ranking functions** that assign a number to each record based on their possition:
### ROW_NUMBER()
Assigns a **unique sequential** number to each record:

>[!example]- Example: **`ROW_NUMBER()`**
>
>```SQL
>SELECT title, playtime_hours,
>  ROW_NUMBER() OVER(ORDER BY playtime_hours DESC) AS position
>FROM videogames;
>```

### RANK() and DENSE_RANK()
In case of a **tie**, **`RANK()`** leaves the numbering **empty** while **`DENSE_RANK()`** does not.
*See the following example for a clearer explanation:*

>[!example]- Example: **`RANK() vs DENSE_RANK()`**
>
>```SQL
>SELECT title, year,
>  RANK() OVER(ORDER BY year) AS rank,
>  DENSE_RANK() OVER(ORDER BY year) AS dense_rank
>FROM videogames;
>```
>
>In this table there are **3 games from 2011**. Since before them there are 4 games from other previous years, all 3 **tie** for the **5th position**:
>- **`RANK()`** assigns `5, 5, 5` and then **jumps to `8`** for the following year (leaving the corresponding `6, 7` positions empty.)
>- **`DENSE_RANK()`** assigns `5, 5, 5` and then **continues with `6`** (leaving no empty numbering) 

## Group Ranking

Combining **`PARTITION BY`** with **`ORDER BY`** you can create rankings **inside each group**:

>[!example]- Example: Group rankings via `PARTITION BY` + `ORDER BY`:
>
>```SQL
>SELECT title, developer, playtime_hours,
>  ROW_NUMBER() OVER(
>    PARTITION BY developer
>    ORDER BY playtime_hours DESC
>  ) AS rank_in_dev
>FROM videogames
>```
>
>This will rank each dev's games by playtime hours
>- Nintendo's longest game will be 1, then 2, etc. Regardless of Rockstar's ranking

## **`LAG()`** and **`LEAD()`** - Accessing Neighboring records

**`LAG()`** accesses the **previous** record and **`LEAD()`** the **next**.

>[!example]- `LAG()` and `LEAD()`:
>
>```SQL
>SELECT title, year,
>  LAG(title) OVER(ORDER BY year) AS previous_game,
>  LEAD(title) OVER(ORDER BY year) AS next_game
>FROM videogames;
>```
>
>Useful to compute differences between consecutive records:
> >[!example]+ Example: Computing the year difference between consecutive games:
> >
> >```SQL
> >SELECT title, year,
> >  year - LAG(year) OVER(ORDER BY year) AS years_since_previous
> >FROM videogames;
> >```

## Aggregation Functions as Window Functions

All [[Aggregation Functions]] (`SUM`, `AVG`, `COUNT`, `MIN`, `MAX`) can be used as **window functions**:

>[!example]- Example: **Aggregation Functions** as **Window Functions**:
>
>```SQL
>SELECT title, playtime_hours,
>  AVG(playtime_hours) OVER() AS avg_global.
>  playtime_hours - AVG(playtime_hours) OVER() AS difference
>FROM videogames
>```
>
>Each record will show the mean deviation, something not possible to get with a simple `GROUP BY`




---
# References
[AprendeSQL from midudev - Funciones de Ventana](https://www.aprendesql.dev/funciones-de-ventana)
