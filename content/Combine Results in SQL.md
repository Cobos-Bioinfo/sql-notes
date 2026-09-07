**Date**: 2026-09-02
**Tags**: [[SQL]]

Combine multiple `SELECT` query results into a single one.

![[{075713FF-588E-4292-956F-447F3C6D0042}.png]]

- **All** `SELECT` must have the **same number of columns**
- Data Types must be **compatible**
- Column **names** are taken from the **first** `SELECT`

## **`UNION`**

`UNION` combines results from both queries and **deletes duplicates**

```SQL
SELECT <columns> FROM <table_1>
UNION
SELECT <columns> FROM <table_2>;
```

Both `SELECT` must have the **same number of columns** and compatible **data types**.

```SQL
SELECT developer from videogames WHERE year < 2000
UNION
SELECT developer FROM videogames WHERE playtime_hours > 50;
```

### **`UNION ALL`**

`UNION ALL` is faster than `UNION` since it does not compare records but will **keep duplicates**.

If a value appears in both `SELECT` it will be repeated in the result.
Use `UNION ALL` when:
- Duplicates don't matter
- You know there won't be any duplicates

>[!info]+ **`ORDER BY` with `UNION`**
>To order results use `ORDER BY` at the end of the query, **OUTSIDE** both `SELECTS`
>
>```SQL
>SELECT title, year FROM videogames WHERE year < 2000
>UNION
>SELECT title, year FROM videogames WHERE year > 2015
>ORDER BY year;
>```

## **`INTERSECT`**

`INTERSECT` returns only records that appear in **both** results.

```SQL
SELECT developer FROM videogames WHERE year < 2015
INTERSECT
SELECT developer FROM videogames WHERE playtime_hours > 50;
```


## **`EXCEPT`**

`EXCEPT` returns records from the **first** `SELECT` that are **NOT** in the **second** `SELECT`

```SQL
SELECT developer FROM videogames WHERE year < 2015
EXCEPT
SELECT developer FROM videogames WHERE playtime_hours > 50;
```





---
# References
[AprendeSQL from midudev - combinando resultados](https://www.aprendesql.dev/combinando-resultados)
