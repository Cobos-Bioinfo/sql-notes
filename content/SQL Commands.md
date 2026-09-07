**Date**: 2026-08-20
**Tags**: [[SQL]], [[PostgreSQL]], [[PostgreSQL Commands]]

**Create** a **Database**:
```PostgreSQL
CREATE DATABASE <database_name>;
```

**Rename** a **Database**:
```PostgreSQL
ALTER DATABASE <old_db_name> RENAME TO <new_db_name>;
```

When oeprating within a Database (in PostgreSQL: `\c <database_name>`):
**Create** a **Table**:
[[Create Tables]]
```PostgreSQL
CREATE TABLE <table_name()>;
```

```PostgreSQL
CREATE TABLE <table_name(colum_name DATATYPE CONSTRAINTS);
```

**Drop** a **Table**:
```PostgreSQL
DROP TABLE <table_name>;
```

**Add** a **Column** to a table with specific [[SQL#Basic Data Types in PostgreSQL]]:
```PostgreSQL
ALTER TABLE <table_name> ADD COLUMN <column_name> <data_type>;
```

>[!tip]- Add a Constraint
>Add a Constraint to a column by adding it right after the `data_type`:
>```SQL
ALTER TABLE <table_name> ADD COLUMN <column_name> <data_type> <constraint>;
>
>-- Example:
>ALTER TABLE characters ADD COLUMN name VARCHAR(30) NOT NULL;
>```

**Drop** a **Column** from a table:
```PostgreSQL
ALTER TABLE <table_name> DROP COLUMN <column_name>;
```

**Rename** a **Column** from a table:
```PostgreSQL
ALTER TABLE <table_name> RENAME COLUMN <old_column_name> TO <new_column_name>;
```

**Insert** records into a table:
```PostgreSQL
INSERT INTO <table_name(column_1, column_2)> VALUES(value_1, value_2);
```

**Delete** a record from a table:
```PostgreSQL
DELETE FROM <table_name> WHERE <condition>;
```

>[!warning]- Warning: `DELETE` without `WHERE`
>Running a `DELETE FROM` without a `WHERE` will erase **ALL** records from a table.
>```SQL
>DELETE FROM table;
>```

**Update** record values:
[[Update Data in SQL]]
```PostgreSQL
UPDATE <table_name> SET <column_name=new_value> WHERE <condition>;
```

>[!example]- Example: `UPDATE`:
>```SQL
>SELECT * FROM characters;
>```                            
>
| character_id |   name    |     homeland     | favorite_color |
> | ---------- | ---------------- | ----------- | ----------- | 
|            1 | Mario     | Mushroom Kingdom | Red            |
|            2 | Luigi     | Mushroom Kingdom | Green          |
|            3 | Peach     | Mushroom Kingdom | Pink           |
|            4 | Toadstool | Mushroom Kingdom | Red            |
|            5 | Bowser    | Mushroom Kingdom | Green          |
|            6 | Daisy     | Sarasaland       | Yellow         |
|            7 | Yoshi     | Dinosaur Land    | Green          |
(7 rows)
>
>```SQL
UPDATE characters SET favorite_color='Orange' WHERE name='Daisy';
>```

**Add Primary Key (PK)** (See [[Primary and Foreign Keys]])
```PostgreSQL
ALTER TABLE <table_name> ADD PRIMARY KEY(<column_name>);
```

**Drop** a **constraint**:
```PostgreSQL
ALTER TABLE <table_name> DROP CONSTRAINT <constraint_name>;
```

>[!example]- Example: Drop a `Primary Key` from a column:
>```SQL
>ALTER TABLE characters DROP CONSTRAINT characters_pkey;
>```

**Add Foreign Key**:
```PostgreSQL
ALTER TABLE <table_name> ADD COLUMN <column_name> <datatype> REFERENCES <referenced_table_name(referenced_column_name)>;
```

**Add** a **Uniqueness Constraint**
```PostgreSQL
ALTER TABLE <table_name> ADD UNIQUE(<column_name>);
```

**Add** a **Not `NULL` Constraint**
```PostgreSQL
ALTER TABLE <table_name> ALTER COLUMN <column_name> SET NOT NULL;
```

## Querying

View full table from `<table_name>`:
```PostgreSQL
SELECT * FROM <table_name>
```

View full table from `<table_name>` ordered by `<column>`:
```PostgreSQL
SELECT * FROM <table_name> ORDER BY <column>;
```

Query **multiple** tables with a **one-to-one** (See [[Relationship Types in Relational Databases]]) on a **Primary Key** via **`JOIN`** operations (See [[Join Tables in SQL]]):
	In this example we use `FULL JOIN`
```PostgreSQL
	SELECT <columns> FROM <table_1> FULL JOIN <table_2> ON <table_1.primary_key> = <table_2.primary_key>
```

>[!example]- Example: One-to-one tables query via `FULL JOIN`:
>Tables are: **characters** and **sounds**.
>	`characters` PK is `character_id` while `sounds` has `sound_id` as a PK and `character_id` as a foreign key.
>
>```SQL
>SELECT * FROM characters FULL JOIN sounds ON characters.character_id = sounds.character_id;
>```

Query **multiple** tables with a **many-to-many** relation via a **junction table** that has a **one-to-many** relation with each table, via **`JOIN`** operations:
```PostgreSQL
SELECT <columns> FROM <junction_table>
FULL JOIN <table_1> ON <junction_table.foreign_key_column> = <table_1.primary_key_column>
FULL JOIN <table_2> ON <junction_table.foreign_key_column> = <table_2.primary_key_column>;
```

>[!example]- Example: Many-tomany tables query via **junction table**:
>```SQL
>SELECT * FROM character_actions 
>FULL JOIN characters ON character_actions.character_id = characters.character_id
>FULL JOIN actions ON character_actions.action_id = actions.action_id;
>```






---
# References
[AprendeSQL from midudev](https:/aprendesql.dev)
