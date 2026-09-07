**Date**: 2026-08-06
**Tags**: [[Computer Science]], [[Databases]], [[000 Zettelkasten/_Tags/NoSQL]]

## NoSQL - Non-Relational Databases

Nowadays **NoSQL** is interpreted as **Not Only SQL**, a series of [[Database Engines]] that don't follow the [[Relational Databases]] model.

**NoSQL** databases are more flexible than **Relational Databases** (**RDs**). They store data in **individual files** that are not connected. Their data model is also flexible.

Some NoSQL DBs may have a basic schema or data model, it is often less rigid than the schema defined in RDs. Data fields can be added, modified or removed if necessary.

## Tables vs Documents
In the **relational model**, data is distributed between multiple **tables** to avoid repeating information ([[Normalization]]). `JOIN` is then used to reconstruct it all.

In a **NoSQL** database, all data live together in a **single nested document**.

## Main Differences with SQL
| Aspect            | Relational (SQL)                                   | NoSQL                                               |
| ----------------- | -------------------------------------------------- | --------------------------------------------------- |
| **Structure**     | Tables with rows and columns                       | Documents, key-value pairs, graphs...               |
| **Schema**        | **Rigid**: You define columns and types in advance | **Flexible**: each record can have different fields |
| **Relationships** | `JOIN`between tables                               | Embedded data or manual references                  |
| **Scaling**       | Mainly **vertical**                                | Designed for **horizontal** scaling                 |
| **Guarantees**    | **ACID** (strong consistency)                      | Often **BASE** (eventual consistency)               |
| **Language**      | Standard SQL                                       | Specific to each base (although many imitate SQL)   |

## Rigid vs Flexible Schema

In **SQL** if a table is defined with columns `id` and `name`, **ALL** records will have exactly those columns. Adding a new field (column) will require an `ALTER TABLE`

In a **NoSQL** database, two records in the same collection can be **different**

```JavaScript
// In the same "users" collection these may coexist
{ "_id": 1, "name": "Ana" }
{ "_id": 2, "name": "Bea", "email": "bea@web.dev", "premium": true }
```

## ACID vs BASE

Unlike **ACID** (Seen in [[Transactions]]), **BASE** (**Basically Available Soft state Eventual consistency**) model prioritizes **availability** and **scaling** over immediate consistency.

## Scaling: Horizontal vs Vertical

- **Vertical** (**Scale Up**): Better server.
- **Horizontal** (**Scale Out**): Distribute data between **many servers**

## The Four NoSQL Data Base Types

1. **Document**
2. **Key-Value**
3. **Columnar**
4. **Graph**

### Document
Store **JSON**-like **documents**.
- Most popular NoSQL
- Examples: [[MongoDB]], [[CouchDB]]

### Key-Value
A huge **dictionary**:
- Ask query a key for its value and instantly obtain it
- Ideal for **cache**
- Examples: [[Redis]], [[DynamoDB]]

### Columnar
Optimized for huge data **read** and **write** distributed in many servers
- Examples: [[Cassandra]], [[HBase]]

### Graph
Model **nodes and their relations**
- Ideal for social media or recomendations where conections matter
- Examples: [[Neo4j]], [[ArangoDB]]

---
# References
[AprendeSQL from midudev - nosql](https://www.aprendesql.dev/nosql)
