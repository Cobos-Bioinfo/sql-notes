**Date**: 2026-08-06
**Tags**: [[Computer Science]], [[Databases]], [[SQL]]

Data organized into tables made of **rows** and **columns**.

- Relational Databases (RD) allow linking tables through shared attributes (columns)
- RDs require a **schema**. It defines the overall structure of the db: tables, columns, data types, relationships and constraints.
- All **tables** in a RD have a **Primary Key (PK)**. A unique identifier for each row. PKs enable relationships between **tables** via foreign keys (See [[Primary and Foreign Keys]]).

>[!example]- Example: Database for a Wildlife Conservation Center for Critically Endangered Animals
Table 1: `animals`: Store general information about the animals. Each row represents an animal while each column represent an attribute (species, size, weight, age, etc.)
Typically you would add an `id` column to uniquely identify each animal. This ID would also be the **PK** and could also be used to **link** its record in this table to related records in other tables
Table 2: `species`: Store information about each species, such as their habitat and conservation status
Table 3,...: Create different tables to store other kinds of information.
Once you have this general schema, link these different tables to the `animals` table wich would act as the central point for animal-related data.

- RDs offer several advantages, including **scalability**. Enforce data integrity through PKs, foreign keys and data types.

In addition to RDs, you can also work with [[100 Raw Notes/NoSQL]]

Next: [[Common Relational Databases]]

---
# References
[freeCodeCamp Relational Databases](https://www.freecodecamp.org/learn/relational-databases-v9/lecture-working-with-relational-databases/what-are-relational-databases-and-how-do-they-differ-from-non-relational-databases)
