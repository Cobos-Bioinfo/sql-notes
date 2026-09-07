**Date**: 2026-08-18
**Tags**: [[SQL]]

`JOIN`allow combining related information from tables to query data.

Variants of `JOIN` include:
- `INNER JOIN
- `FULL OUTER JOIN`
- `LEFT OUTER JOIN`
- `RIGHT OUTER JOIN`
- `SELF JOIN`
- `CROSS JOIN`

![[{79902255-3483-4328-A868-F31777E78365}.png]]

**`INNER JOIN`**
- Filters the result to include only **records** where the values in the **joining** columns are **equal** in both tables (Intersection of the data).
- *Most common `JOIN` operator (~90%). Useful when you only need records with data in both tables.*

> [!example]- Example: `INNER JOIN` | `products` and `sales` tables.
> | product_id | product_name     | category    | price (USD) | origin       |
> | ---------- | ---------------- | ----------- | ----------- | -------------|
> | 1          | Ice Cream        | Food        | 2.50        | India        |
> | 2          | Pizza Margherita | Food        | 12.00       | Italy        |
> | 3          | Sushi            | Food        | 18.75       | Japan        |
> | 4          | T-Shirt          | Clothing    | 25.00       | USA          |
> | 5          | Jeans            | Clothing    | 60.00       | Argentina    |
> | 6          | Coffee           | Beverages   | 35.00       | France       |
> | 7          | Juice            | Beverages   | 5.00        | Colombia     |
> 
> | sale_id | product_id | quantity | sale_date  |
> | ------- | ---------- | -------- | ---------- |
> | 101     | 1          | 2        | 2025-07-18 |
> | 102     | 2          | 3        | 2025-02-13 |
> | 103     | 6          | 10       | 2025-06-08 |
> | 104     | 5          | 8        | 2025-01-10 |
> | 105     | 2          | 1        | 2025-05-15 |
> 
> ```SQL
> SELECT *
> FROM products
> INNER JOIN sales
> 	ON products.product_id = sales.product_id;
> ```
>
>Result:
>
>| product_id | product_name     | category  | price | origin    | sale_id | product_id | quantity | sale_date  |
| ---------- | ---------------- | --------- | ----- | --------- | ------- | ---------- | -------- | ---------- |
| 1          | Ice Cream        | Food      | 2.50  | India     | 101     | 1          | 2        | 2025-07-18 |
| 2          | Pizza Margherita | Food      | 12.00 | Italy     | 102     | 2          | 3        | 2025-02-13 |
| 2          | Pizza Margherita | Food      | 12.00 | Italy     | 105     | 2          | 1        | 2025-05-15 |
| 5          | Jeans            | Clothing  | 60.00 | Argentina | 104     | 5          | 8        | 2025-01-10 |
| 6          | Coffee           | Beverages | 35.00 | France    | 103     | 6          | 10       | 2025-06-08 |
>
>We only see records of the products that have been sold.

**`FULL OUTER JOIN`**
- Returns **all records** from both tables.
- If a **match** is found in the specified columns, the data is combined and you get all columns for each matching record.
- If **no match** in either one of the tables, the missing columns are filled with **`NULL`** values.
- *Not very common `JOIN` operator. When you need everything, with and without match.*

>[!example]- Example: `FULL OUTER JOIN`
>
> ```SQL
> SELECT *
> FROM products
> FULL OUTER JOIN sales
> 	ON products.product_id = sales.product_id;
> ```
>
>Result:
>
>| product_id | product_name     | category  | price | origin    | sale_id | product_id | quantity | sale_date  |
| ---------- | ---------------- | --------- | ----- | --------- | ------- | ---------- | -------- | ---------- |
| 1          | Ice Cream        | Food      | 2.50  | India     | 101     | 1          | 2        | 2025-07-18 |
| 2          | Pizza Margherita | Food      | 12.00 | Italy     | 102     | 2          | 3        | 2025-02-13 |
| 3          | Sushi            | Food      | 18.75 | Japan     |         |            |          |            |
| 4          | T-Shirt          | Clothing  | 25.00 | USA       |         |            |          |            |
| 5          | Jeans            | Clothing  | 60.00 | Argentina | 104     | 5          | 8        | 2025-01-10 |
| 6          | Coffee           | Beverages | 35.00 | France    | 103     | 6          | 10       | 2025-06-08 |
| 7          | Juice            | Beverages | 5.00  | Colombia  |         |            |          |            |
| 2          | Pizza Margherita | Food      | 12.00 | Italy     | 105     | 2          | 1        | 2025-05-15 |
>
>`Pizza Margherita (2)` has all the data since it was a sale. While `T-Shirt (4)` has empty (`NULL`) columns because no match was found in the `sales` table

**`LEFT OUTER JOIN`**
- Get **all records** from the **left table** and matching information from the right table.
- If **no match**, columns from the **right table** are filled with `NULL` values.

>[!example]- Example: `LEFT OUTER JOIN`
>
> ```SQL
> SELECT *
> FROM products
> LEFT JOIN sales
> 	ON products.product_id = sales.product_id;
> ```
>
>Result:
>
>product_id | product_name     | category  | price | origin    | sale_id | product_id | quantity | sale_date
>---------- | ---------------- | --------- |-------|-----------|---------|------------|----------|------------
 >        1 | Ice Cream        | Food      |  2.50 | India     |     101 |          1 |        2 | 2025-07-18
 >        2 | Pizza Margherita | Food      | 12.00 | Italy     |     102 |          2 |        3 | 2025-02-13
 >        3 | Sushi            | Food      | 18.75 | Japan     |         |            |          |
 >        4 | T-Shirt          | Clothing  | 25.00 | USA       |         |            |          |
 >        5 | Jeans            | Clothing  | 60.00 | Argentina |     104 |          5 |        8 | 2025-01-10
 >        6 | Coffee           | Beverages | 35.00 | France    |     103 |          6 |       10 | 2025-06-08
 >        7 | Juice            | Beverages |  5.00 | Colombia  |         |            |          |
 >        2 | Pizza Margherita | Food      | 12.00 | Italy     |     105 |          2 |        1 | 2025-05-15
>
>Same as `FULL OUTER JOIN` because it includes all the rows from the first table (`products`)

`ANTI-JOIN` **Pattern**:
- Useful pattern to find records without match, combining `LEFT JOIN` with `WHERE ... IS NULL`

>[!example]- Example: `ANTI-JOIN` Pattern:
>
>```SQL
>SELECT v.title
>FROM videogames v
>LEFT JOIN developers d ON v.developer_id = d.id
>WHERE d.id IS NULL;
>```
>
>*This will only return videogames without an assigned developer.*

**`RIGHT OUTER JOIN`**
- Opposite to `LEFT OUTER JOIN`: Get all records from right table and matching information from left.

>[!example]- Example: `RIGHT OUTER JOIN`
>
> ```SQL
> SELECT *
> FROM products
> RIGHT JOIN sales
> 	ON products.product_id = sales.product_id;
> ```
>
>Result:
>
>product_id | product_name     | category  | price | origin    | sale_id | product_id | quantity | sale_date
>---------- | ---------------- | --------- |-------|-----------|---------|------------|----------|------------
 >        1 | Ice Cream        | Food      |  2.50 | India     |     101 |          1 |        2 | 2025-07-18
 >        2 | Pizza Margherita | Food      | 12.00 | Italy     |     102 |          2 |        3 | 2025-02-13
 >        6 | Coffee           | Beverages | 35.00 | France    |     103 |          6 |       10 | 2025-06-08
 >        5 | Jeans            | Clothing  | 60.00 | Argentina |     104 |          5 |        8 | 2025-01-10
 >        2 | Pizza Margherita | Food      | 12.00 | Italy     |     105 |          2 |        1 | 2025-05-15
>
>We see **fewer** records. Because it takes **all records** from the **right** table (`sales`), and this table has fewer rows than `products`table.

Now, for some less commonly-known/used `JOIN` operations:

**`SELF JOIN`**
- Join the table with itself. Helpful for comparing different records within the same table

**`CROSS JOIN`** AKA `Cartesian JOIN`
- Join every row from the first table with every row of the second table. Therefore generating **all possible row combinations**. 
- No need for conditions to join the tables.


---
# References
[freeCodeCamp Join Tables in SQL](https://www.freecodecamp.org/learn/relational-databases-v9/lecture-working-with-relational-databases/what-are-the-different-ways-to-join-tables)
[AprendeSQL from midudev - joins externos](https://www.aprendesql.dev/joins-externos)
