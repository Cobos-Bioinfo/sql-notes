**Date**: 2026-08-06
**Tags**: [[Computer Science]], [[Databases]]

SQL (initially known as the Structured English Query Language (SEQUEL)) is a programming language used for storing and managing data in [[Relational Databases]] (RD).

SQL enables you to **add, change, delete, find** and **retrieve** data from a RD.

SQL can help optimize Database (DB) performance by querying only the necessary data and it can be integrated with many programming languages so you can interact with databases directly from your applications.

SQL enforces data integrity and includes many security features, like support for user authentication and data encryption.

It is scalable, portable and compatible with many database management systems.

---
## Fundamental SQL Commands
[[SQL Commands]], [[PostgreSQL]], [[PostgreSQL Commands]]

SQL is based on commands knows as **SQL statements** or **SQL queries**.

>[!tip] Open `psql` shell to interact with the PostgreSQL database directly:
>`psql -U <username> -d <database_name>`
>If you haven't created a database, PostgreSQL includes `postgress` db by default.

- **Create a database** named *my_database*
```PostgreSQL
CREATE DATABASE my_database;
```

- **Connect to the Database**
	How to connect to the db depends on the environment and tool used (see previous tip)

- **Switch DB within `psql`**
```PostgreSQL
\c my_database
```
   *Note that `\c` command and other shortcut commands (those preceeded by `\`) are part of the `psql` shell and not part of the SQL language itself (no `;` needed!).*

- **Create a table** named *products*
```PostgreSQL
CREATE TABLE products (
	id SERIAL,
	name VARCHAR(255)
);
```
Within parenthesis we wrote the **names** of the **columns** for the table and specified the **data type** of the **values** to be stored in each column.

In the previous example the table `products` will have a column for the product ID and another for the name of the product.

>[!info] The standard naming convention for SQL table and column names is snake_case

---

## Basic Data Types in PostgreSQL
[[SQL Data Types]]

There are **six** popular categories of **data types** in SQL:

| **Numeric**               | `INTEGER`, `FLOAT`, `SERIAL`, `DECIMAL`, `NUMERIC` |
| ------------------------- | -------------------------------------------------- |
| **Date / Time**           | `TIMESTAMP`, `DATE`, `TIME`                        |
| **Character / String**    | `CHAR`, `VARCHAR`, `TEXT`                          |
| **Unicode**               | `NTEXT`, `NVARCHAR`                                |
| **Binary**                | *Non-textual data* (images, audio, etc.)           |
| **Other / Miscellaneous** | e.g., `XML`, `TABLE`                               |
*Note that **Unicode** data types are used to ensure text will be stored and retrieved correctly regardless of the character's origin.*

### Numeric Value Data Types

- `INTEGER`: Consume 4 bytes and range from -2,147,483,648 to +2,147,483,648

```PostgreSQL
units_sold INTEGER
```

- `SMALLINT` and `BIGINT` are basically the same as integer but with smaller/bigger range and size
- `SERIAL`: Useful PostgreSQL feature. Helpful for creating unique identifier columns becuase that column will have an `INTEGER`type that will NOT allow `NULL` values and automatically increment when rows are added

```PostgreSQL
id SERIAL
```

>[!info] In MySQL the equivalent of `SERIAL`sould be `AUTO_INCREMENT`

- `NUMERIC(p, s)`: User defined precision and scale. 
Up to `p` total digits of which `s` must be decimals.
### Character Value Data Types

- `VARCHAR(N)`: Used strings of maximum length `N`

```PostgreSQL
name VARCHAR(50)
```

- `TEXT` Store strings of any length


See [[How to Insert and View-Query Data in a Table]]

---
# References
[freeCodeCamp What is SQL](https://www.freecodecamp.org/learn/relational-databases-v9/lecture-working-with-relational-databases/what-is-sql-and-how-can-you-create-a-database-with-tables)
[freeCodeCamp Basic Data Types in SQL](https://www.freecodecamp.org/learn/relational-databases-v9/lecture-working-with-relational-databases/what-are-the-basic-data-types-in-sql)