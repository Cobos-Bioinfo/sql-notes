**Date**: 2026-08-31
**Tags**: [[SQL]]

An **index** is a structure the database creates to quickly find records.
- Without an index, when running `WHERE year = 2013`, the database looks through **ALL** records in the table (known as a [[Full Table Scan]]).
- With an index in the `year` column, the database jumps straight to the records that match the query criteria.

## Create INDEX

```SQL
CREATE INDEX <index_name> ON <table(<column>)>;
```

Standard naming convention for indexes is: **`idx_table_column`** 

```SQL
CREATE INDEX idx_videogames_year ON videogames(year);
```

## Indexes on Multiple Columns

An index can be created for multiple columns.
- **Order matters**: The index is more efficient when **filtering** in the **same order** as the index was **defined**.

```SQL
CREATE INDEX idx_videogames_dev_year ON videogames(developer, year);
```

*The previous index would:*
- **Accelerate** queries such:
```SQL
WHERE developer = 'Nintendo' -- ✓ Uses the first column
```
or
```SQL
WHERE developer = 'Nintendo' AND year > 2000 -- ✓ Uses both columns
```

- **Would no NOTHING** for queries such:
```SQL
WHERE year > 2000 -- ✗ year is NOT the index first column
```

## Unique Indexes

`UNIQUE INDEX` besides accelerating queries also impedes **duplicate values** in that column:

```SQL
CREATE UNIQUE INDEX idx_videogames_title ON videogames(title);
```

With this index, attempting to **insert** (See [[How to Insert and View-Query Data in a Table]]) two games with thee **same title** will **error**.
- Note that **Primary Keys** (See [[Primary and Foreign Keys]]) create a **Unique Index** automatically

## Delete an Index

```SQL
DROP INDEX <index_name>;
```

*safer approach:*

```SQL
DROP INDEX IF EXISTS <index_name>;
```

## When to Create Indexes?

You **should** create index for:
- Columns that are frequently used with `WHERE`
- Columns of `JOIN` (**foreign keys**)
- Columns used in `ORDER BY` in large tables
- Columns with many different values (**high cardinality**)

You should **NOT** create index for:
- Small tables (no distinguishible improvement)
- Columns with **few distinct values** (e.g., Boolean column)
- Tables where many `INSERT` / `UPDATE` / `DELETE` are performed and few **reads**. Every write has to update all indexes.

>[!tip]- Tip: Indexes speed up **reads** but slow down **writes**!
>Every `INSERT` or `UPDATE` needs to update all indexes.

## See Indexes from a Table

In [[SQLite]] you can check existing indexes with:

```SQLite
SELECT name, tbl_name
FROM sqlite_master
WHERE type = 'index';
```


---
# References
[AprendeSQL from midudev - indices](https://www.aprendesql.dev/indices)
