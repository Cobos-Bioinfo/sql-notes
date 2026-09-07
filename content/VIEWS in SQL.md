**Date**: 2026-08-28
**Tags**: [[SQL]]

In SQL, a `VIEW` is a `SELECT` **query** saved with a name. 
- It functions as a **virtual table**: it does not store data by itself, it executes the query every time that is used
- Simplify complex queries by reusing logic
- Readability: Providing a View with a descriptive name is clearer than the whole query

## Create VIEW:
```SQL
CREATE VIEW <name_view> AS
SELECT <columns>
FROM <table_name>
WHERE <constraints>;
```

>[!example]- Example: Create `VIEW` for modern games (post-2010):
>
>```SQL
>CREATE VIEW modern_games AS
>SELECT title, developer, year, playtime_hours
>FROM videogames
>WHERE year > 2010;
>```
>
>Now it can be used as any other table:
>
>```SQL
>SELECT * FROM modern_games;
>
>SELECT title FROM modern_games WHERE playtime_hours > 50;
>```

**VIEWS** can contain any **query** including `JOIN`s:

```SQL
CREATE VIEW catalogue AS
SELECT v.title, d.name AS developer, d.country, v.year
FROM videogames v
JOIN developers d ON v.developer_id = d.id;
```

After a view is created you can use it as if it was a table:

```SQL
SELECT * from catalogue;
```

### Views with `JOIN`
Views can contain any kind of query, including `JOIN`:
```SQL
CREATE VIEW catalogue AS
SELECT v.title, d.name AS developer, d.country, v.year
FROM videogames v
JOIN developers d ON v.developer_id = d.id;
```

*Now you could:*

```SQL
SELECT * FROM catalogue WHERE country = 'Japan';
```

### Delete a View

Dropping a view does not delete the original data since views do not store data.

```SQL
DROP VIEW <view_name>;
```

*Safer approach:*

```SQL
DROP VIEW IF EXISTS <view_name>;
```

## Notes:
- [[SQLite]] does not support `CREATE OR REPLACE VIEW`. If you need to modify a view you first need to delete it (`DROP`) and create it anew.
- [[SQLite]] views are **read-only** you CAN'T `INSERT`, `UPDATE` or `DELETE` directly onto a view.


---
# References
[AprendeSQL from midudev - vistas](https://www.aprendesql.dev/vistas)
