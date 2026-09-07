**Date**: 2026-08-23
**Tags**: [[SQL]]

As see in [[SQL Commands]], `CREATE TABLE` defines the structure of a new table.
- Specify **Columns**, **Data Types** and **Constraints**

```SQL
CREATE TABLE <table_name>(
  <column_name_1> <DATA TYPE> >CONSTRAINTS>,
  <column_name_2> >DATA TYPE> <CONSTRAINTS>,
  ...
);
```

See [[SQL Data Types]]

## Constraints

| Restricción   | Descripción                               |
| ------------- | ----------------------------------------- |
| `PRIMARY KEY` | Unique reccord identifier                 |
| `NOT NULL`    | Enforce non-nullity for records           |
| `UNIQUE`      | Enforce uniqueness for records            |
| `DEFAULT`     | Default value unless specified            |
| `CHECK`       | Validate that value satisfies a condition |
`CHECK` constraint allows to to define validation rules in the table itself.

>[!example]- Example: Use `CHECK` constraint to ensure player level remains positive:
>```SQL
>CREATE TABLE players(
>    ...
>    level INTEGER NOT NULL CHECK(level > 0)
>);
>```

## **`IF NOT EXISTS`**

If you try to create an already-existing table, SQL errors.
- Use `IF NOT EXISTS` to avoid it:

```SQL
CREATE TABLE IF NOT EXISTS players(
  id integer PRIMARY KEY,
  name TEXT NOT NULL,
  level INTEGER NOT NULL DEFAULT 1
);
```

With `IF NOT EXISTS` the command will simply do nothing if the table already exists.
- This is very useful for **initialization scripts** that can be ran more than once.





---
# References
[AprendeSQL from midudev - Creando Tablas](https://www.aprendesql.dev/creando-tablas)
