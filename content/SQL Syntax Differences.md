**Date**: 2026-09-06
**Tags**: [[SQL]], [[Relational Databases]]

There are many [[Common Relational Databases]], choosing one is an important decision ([[Database Engines]]). However, there are some **Syntax Differences** between them that are important to keep in mind:

We will see some examples for: [[SQLite]], [[PostgreSQL]], [[MySQL]] / [[MariaDB]]
## Autoincrease

```SQL
-- SQLite
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT
);

-- PostgreSQL
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name TEXT
);
-- Or in modern PostgreSQL:
CREATE TABLE users (
  id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  name TEXT
);

-- MySQL / MariaDB
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255)
);
```

## Text Types
| Engine         | Main type            | Does it have a limit?            |
| -------------- | -------------------- | -------------------------------- |
| **SQLite**     | `TEXT`               | No (dynamic)                     |
| **PostgreSQL** | `TEXT`/ `VARCHAR(n)` | `TEXT` is unlimited              |
| **MySQL**      | `VARCHAR(n)`/ `TEXT` | `VARCHAR`requires maximum length |
## Boolean 

```SQL
-- SQLite: no native BOOLEAN type, uses 0 and 1
SELECT * FROM users WHERE active = 1;

-- PostgreSQL: native BOOLEAN
SELECT * FROM users WHERE active = TRUE;

-- MySQL: BOOLEAN is an alias of TINYINT(1)
SELECT * FROM users WHERE active = TRUE; -- TRUE is 1
```

## Text Concatenation

```SQL
-- SQLite and PostgreSQL
SELECT name || ' ' || surname FROM users;

-- MySQL / MariaDB
SELECT CONCAT(name, ' ', surname) FROM users;
```

## **`UPSERT`** (`INSERT` or `UPDATE`)

```SQL
-- SQLite
INSERT INTO users (id, name) VALUES (1, 'Ana')
ON CONFLICT(id) DO UPDATE SET name = excluded.name;

-- PostgreSQL
INSERT INTO users (id, name) VALUES (1, 'Ana')
ON CONFLICT(id) DO UPDATE SET name = EXCLUDED.name;

-- MySQL / MariaDB
INSERT INTO users (id, name) VALUES (1, 'Ana')
ON DUPLICATE KEY UPDATE name = VALUES(name);
```

## Limit Results

```SQL
-- SQLite, PostgreSQL and MySQL
SELECT * FROM users LIMIT 10;

-- PostgreSQL also supports the SQL standard:
SELECT * FROM users FETCH FIRST 10 ROWS ONLY;
```

## JSON

```SQL
-- SQLite (basic support)
SELECT json_extract(data, '$.name') FROM config;

-- PostgreSQL (advanced support with JSONB)
SELECT data->>'name' FROM config;
SELECT * FROM config WHERE data @> '{"active": true}';

-- MySQL (intermediate support)
SELECT JSON_EXTRACT(data, '$.name') FROM config;
-- Or with the shortcut:
SELECT data->>"$.name" FROM config;
```

**PostgreSQL** has the advantage: its type **`JSONB`** stores JSON in binary format: allows **indexes** on JSON fields and has specific operators (`@>`, `?`, `#>`) that make it extremelly powerful.

## When to use each engine

| Scenario                  | Recommended engine       | Because                                        |
| ------------------------- | ------------------------ | ---------------------------------------------- |
| Mobile/desktop app        | **SQLite**               | Embedded, serverless, single file              |
| Modern web app            | **PostgreSQL**           | Robust, extensible, great ecosystem            |
| Legacy LAMP project       | **MariaDB**              | Compatible with MySQL, clean license           |
| Simple microservice       | **SQLite**               | Minimum infrastructure                         |
| Geospatial data           | **PostgreSQL** + PostGIS | The industry standard                          |
| Analytics /data warehouse | **PostgreSQL**           | Window functions, recursive CTEs, JSONB        |
| Start a new project       | **PostgreSQL**           | The most versatile option with the best future |

## License Summary

- **SQLite**: Public domain. Without restrictions of any kind.
- **PostgreSQL**: PostgreSQL license (MIT/BSD type). You can do whatever you want, even closed commercial products.
- **MariaDB**: GPL v2. If you distribute software that includes MariaDB, you must publish the source code (or purchase a commercial license).
- **MySQL**: GPL v2 + Oracle Commercial License. Same requirement as MariaDB, but if you don't want to comply with GPL, you must pay Oracle.

In practice, if you use the database as a service (your app connects to a database server), the GPL generally **it doesn't force you** to release your code. But if you embed the engine inside your software and distribute it, yes.

**PostgreSQL** and **SQLite** have **zero legal concerns**.


---
# References
[AprendeSQL from midudev - Motores de Bases de Datos - Diferencias de Sintaxis Importantes](https://www.aprendesql.dev/motores-de-bases-de-datos)
