**Date**: 2026-08-17
**Tags**: [[SQL]], [[Relational Databases]], [[Query Databases]]

## Insert Data
To **view** data, you first need to **insert** it.

Let's use the following table as a base case example to explain all:
```PostgreSQL
CREATE TABLE dogs(
	id SERIAL,
	name VARCHAR(100),
	age INTEGER
);
```
*Note that since we used PostgreSQL's`SERIAL` for the `id`, its value will be incremented automatically and we don't need to pass it a value when inserting a record.

>[!note]- Note: **SQLite** automatically assigns an *autoincrement* value to a column if defined with **`INTEGER PRIMARY KEY`**

We use `INSERT INTO <NAME OF TABLE> (<COLUMNS>) VALUES (<VALUES>);` 
```PostgreSQL
INSERT INTO dogs (name, age)
VALUES ('Gino', 3);
```
This is usually the safest option since we specify columns explicitly.
*Note that 'Gino' needs to be in single quotes because it is a `VARCHAR` type*

Alternatively we could insert the record without specifying the columns such that:
```PostgreSQL
INSERT INTO dogs
VALUES ('Gino', 3);
```
This syntax is valid SQL but error-prone.

Since values are assigned on column order it will try to assign 'Gino' into the `id` column which expects an integer. This will result in a `invalid input syntax for type integer` error.

You can also insert multiple records in the same SQL command:
```PostgreSQL
INSERT INTO dogs(name, age)
VALUES
	('Gino', 3),
	('Nora', 2);
```

### `INSERT OR IGNORE` and `INSERT OR REPLACE`

When inserting a record that infringes a constraint (`UNIQUE` or `PRIMARY KEY`), SQL errors.
[[SQLite]] offers two ways to handle these conflicts:

- **`INSERT OR IGNORE`**
Ignores the conflicting record, without erroring:
```SQLite
INSERT OR IGNORE INTO videogames(id, title, developer, year, playtime_hours)
VALUES(1, 'Other Game', 'Other Dev', 2020, 50);

-- Nothing happens because id = 1 record exists
```

- **`INSERT OR REPLACE`**
Replaces the existing record with the new data:
```SQLite
INSERT OR REPLACE INTO videogames(id, title, developer, year, playtime_hour)
VALUES(1, 'Zelda', 'Nintendo', 2024, 30);

-- id = 1 record is replaced
```
## Query (`SELECT`) Databases

The same way you can insert records, you can **query** information from the database. In the context of databases a query is a **request** for data.

`SELECT` statement is used for querying data from one or more tables. 
- To query all the information from the `dogs` table you write:
```PostgreSQL
SELECT *
FROM dogs;
```
In this command the `*`  represents all columns

>[!info]- You will get all the data of all the records in the table:
>```PostgreSQL
 id | name | age 
----+------+-----
  1 | Gino |   3
  2 | Nora |   2
  >```
  
To query **specific** columns, write column names after `SELECT`:
```PostgreSQL
SELECT name, age
FROM dogs;
```

>[!info]- This result will only include the selected columns:
>```Postgresql
 name | age 
------+-----
 Gino |   3
 Nora |   2
 >```
 
You can use **conditions** when querying the data via the `WHERE` keyword like:
```PostgreSQL
SELECT *
FROM dogs
WHERE age < 3;
```

>[!info]- You will get all the data of all the records in the table:
>```PostgreSQL
 id | name | age 
----+------+-----
  2 | Nora |   2
  >```

If you want to know the `age` of `'Gino'` use the `=`:
```PostgreSQL
SELECT age
FROM dogs
WHERE name = 'Gino';
```

>[!info]- Result:
>```sql
>  age 
>-----
  >   3
  >```

Next: [[Primary and Foreign Keys]]

---
# References
[freeCodeCamp How Do You Insert and View Data in a Table](https://www.freecodecamp.org/learn/relational-databases-v9/lecture-working-with-relational-databases/how-do-you-insert-and-view-data-in-a-table)
[AprendeSQL from midudev - Insertando Registros ](https://www.aprendesql.dev/insertando-registros)
