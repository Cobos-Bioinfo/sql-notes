**Date**: 2026-09-06
**Tags**: [[SQL]]

A **Transaction** is a **group of database SQL operations executed as a single unit**.

There are **four properties** that ensure database reliabality, known as **ACID**:

| Propiedad       | Significado                                                   |
| --------------- | ------------------------------------------------------------- |
| **A**tomicity   | All operations succeed or none do                             |
| **C**onsistency | The database moves from one valid state to another            |
| **I**solation   | Concurrent transactions do not interfere with each other      |
| **D**urability  | Committed changes remain permanent even after system failures |

## Implementation

**`BEGIN TRANSACTION;`**: Starts the Transaction block
- All operations after `BEGIN` will **wait** until confirmed with `COMMIT` or undone with `ROLLBACK`
**`COMMIT;`**: Saves all changes permanently
- Once executed there is no undoing.
**`ROLLBACK`**: Undoes all changes made during the transaction (since `BEGIN`)
	Note that if an error occurs during execution, most db engines automatically trigger a `ROLLBACK`

```SQL
BEGIN TRANSACTION;

-- Deduct from Account A
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;

-- Add to Account B
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

-- Finalize changes
COMMIT;
```

## Autocommit
By default, unless a **Transaction** is explicitly stated, SQL is executed in its **own implicit transaction** that is **confirmed automatically**
```SQL
DELETE FROM videogames WHERE id = 1;
-- This is confirmed automatically, no ROLLBACK is possible
```

Therefore, when using `BEGIN` **autocommit** is disabled until `COMMIT` or `ROLLBACK` is used.

>[!example]- More examples:
>
>```SQL
>BEGIN TRANSACTION;
>INSERT INTO orders(cliend_id, total) VALUES(5, 59.99);
>UPDATE clients SET total_orders = total_orders + 59.99 WHERE id = 5;
>COMMIT;
>```
>
>If both operations are executed without error, `COMMIT` will record them permanently.
>
>**Classic Example: Bank Transfer:**
>
>```SQL
>BEGIN TRANSACTION;
>
>-- 1. Deduct from origin account
>UPDATE accounts SET balance = balance - 500
>WHERE id = 1 AND balance >= 500;
>
>-- 2. Add to destination account
>UPDATE accounts SET balance = balance + 500
>WHERE id = 2;
>
>COMMIT;
>```
>Without an **explicit transaction**, if the system fails between 1. and 2., the **money dissapears**. With transaction, if something goes wrong `ROLLBACK` ensures no changes are made.

## When to use Transactions
- **Many** operations must be **atomic**
- **Safety net** needed: Before executing a massive `UPDATE` or `DELETE` so a `ROLLBACK` can be used if the result is not what expected
- **Performance** in **massive `INSERT`**: Wrapping many `INSERT` operations into a single transaction is **much faster** that running them separately. [[SQLite]] will write to disc only once instead for each `INSERT`


---
# References
[AprendeSQL from midudev - Transacciones](https://www.aprendesql.dev/transacciones)
