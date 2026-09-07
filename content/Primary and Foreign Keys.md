**Date**: 2026-08-17
**Tags**: [[SQL]]

- Used to establish relationships between tables
- Optimize data retrieval
- Ensure data uniquess and integrity

## Primary Keys (PK)
Column or set of columns that uniquely identifies each **record** (row) in a table, ensuring no records will have the same value for the PK.

- A table can only have **one** PK.
- PK values can't be `NULL`, always have a valid value.

In PostgreSQL, to make a column the PK of your table add `PRIMARY KEY` after the data type:
```PostgreSQL
column_name data_type PRIMARY KEY
```

>[!example] Example: Students table:
>```PostgreSQL
>CREATE TABLE students (
>	student_id SERIAL PRIMARY KEY,
>	name VARCHAR(100)
>);
>```

*Note that we used `SERIAL` for the PK data type to ensure `student_id` always has a unique value (See [[SQL#Numeric Value Data Types]]).*

A **composite PK** is for tables with no single column to identify each record. In this case you use a combination of **two or more** columns as the PK that **together are unique,** like this:

```PostgreSQL
CREATE TABLE table_nae (
	column1 data_type column_constraint,
	column2 data_type column_constraint,
	column3 data_type column_constraint,
	...
	PRIMARY KEY (column1, column2)
);
```

>[!example]- Example: Table with a `student_id` and `course_id`
>Each student will be enrolled in several classes, and each class will have several students enrolled in it. **Neither column is unique**. Since no student can be enrolled in the same class more than once, we use the combination of the two columns as the PK:
>```SQL
>CREATE TABLE course_enrollments (
>	student_id INT,
>	course_id INT,
>	...
>	PRIMARY KEY (student_id, course_id),
>);
>```

## Foreign Keys

- Column or set of columns in a table that references the **PK of another table**.
- A table can have **multiple foreign keys**.
- You **CAN'T** insert a value that does not exist in the reference table.

```SQL
CREATE TABLE <table_name>(
 <column_name_1> <DATA TYPE> >CONSTRAINTS>,
 <column_name_2> >DATA TYPE> <CONSTRAINTS>,
 ...
 FOREIGN KEY (<column_name_2>) REFERENCES <reference_table>(<column_name_1>)
);
```

*This indicates that `column_name_2` must exist as `column_name_1` in `reference_table`*.

>[!example]+ Example: Tables `customers` and `orders`
>
>```SQL
>CREATE TABLE customers (
>	customer_id SERIAL PRIMARY KEY,
>	first_name VARCHAR(100) NOT NULL,
>	...
>);
>```
>
>```SQL
>CREATE TABLE orders (
>	order_id SERIAL PRIMARY KEY,
>	customer_id INTEGER,
>	...
>	FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
>);
>```

>[!important]- Important: In [[SQLite]] Foreign Keys are disabled by default
>You must manually run the following command at the start of every conection:
>```SQLite
>PRAGMA foreign_keys = ON;
>```

**STEP BY STEP EXAMPLE:**
1. Create Main (Reference) Table
```SQL
CREATE TABLE developers(
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL
);

INSERT INTO developers VALUES(1, 'Nintendo'), (2, 'Valve');
```

2. Create Secondary Table (Foreign Key)
```SQLite
CREATE TABLE videogames(
  id INTEGER PRIMARY KEY,
  title TEXT NOT NULL,
  developer_id INTEGER NOT NULL,
  FOREIGN KEY (developer_id) REFERENCES developers(id)
);
```

3. Inserting **Valid Data**:
``` SQLite
INSERT INTO videogames VALUES (1, 'Zelda', 1);    -- ✓ Nintendo exists
INSERT INTO videogames VALUES (2, 'Portal 2', 2); -- ✓ Valve exists
```

4. Inserting **Invalid Data** (`developer_id` does NOT exist):
```SQLite
INSERT INTO videogames 
VALUES (3, 'Game X', 99); -- ✗ ERROR: 99 does not exist
```


### What happens when deleting a referenced record?

| Approach   | Behavior                                                               |
| ---------- | ---------------------------------------------------------------------- |
| `RESTRICT` | Restricts deletion if record is referenced by another record (Default) |
| `CASCADE`  | Automatically delete all child records                                 |
| `SET NULL` | Change to `NULL` the Foreign Key of child records                      |
>[!example]- Example: **`ON DELETE CASCADE`**
>If a developer is deleted, all their games are deleted automatically.
>```SQL
>CREATE TABLE videogames(
>  id INTEGER PRIMARY KEY,
>  title TEXT NOT NULL,
>  developer_id INTEGER,
>  FOREIGN KEY (developer_id) REFERENCES developers(id)
>    ON DELETE CASCADE
>);
>```
>
>```SQL
>DELETE FROM developers WHERE id = 1;
>```
>*This will also delete all videogames with `developer_id = 1`*

>[!example]- Example: `ON DELETE SET NULL`
>If a developer is deleted, their games persist with `developer_id = NULL`
>
>```SQL
>CREATE TABLE videogames(
>  id INTEGER PRIMARY KEY,
>  title TEXT NOT NULL,
>  developer_id INTEGER,
>  FOREIGN KEY (developer_id) REFERENCES developers(id)
>    ON DELETE SET NULL
>);
>```
>
>```SQL
>DELETE FROM developers WHERE id = 1;
>```
>
>*Nintendo games now have `developer_id = NULL`*

>[!tip] Tip: Choose the correct strategy:
>`CASCADE` is useful when the *children* make no sense without their *parent* (e.g., comments in a post).
>`SET` is better when *children* can exist without the relation (e.g., an employee who's department is dissolved).

### Multiple Foreign Keys
A table can have **multiple foreign keys**:
```SQL
CREATE TABLE scores(
  id INTEGER PRIMARY KEY,
  player_id INTEGER NOT NULL,
  game_id INTEGER NOT NULL,
  points INTEGER NOT NULL,
  
  FOREIGN KEY (player_id) REFERENECS players(id),
  FOREIGN KEY (game_id) REFERENECS videogames(id)
);
```

Every `INSERT` in `scores` will first verify that `player_id` exists in `players` and `game_id` exists in `videogames`.



Next: [[Relationship Types in Relational Databases]]

---
# References
[freeCodeCamp Primary and Foreign Keys in SQL](https://www.freecodecamp.org/learn/relational-databases-v9/lecture-working-with-relational-databases/what-are-primary-and-foreign-keys-in-sql-and-how-do-they-work)
[AprendeSQL from midudev - Claves Foraneas](https://www.aprendesql.dev/claves-foraneas)
