**Date**: 2026-08-21
**Tags**: [[SQL]]

In **SQL** `NULL` is the lack of value.
- `NULL` **≠** `0`
- `NULL` **≠** `''`
- `NULL` **≠** `false`

`NULL` in **Operations**:
- `5 + NULL` **=** `NULL`
- `NULL = NULL` **=** `NULL` **(!)**
- `NULL IS NULL` **=** `true`
**(!)** You can't compare `NULL` with `=`, you must use **special operators** (`IS`, `IS NOT`).

## COALESCE: Default Value
[[Coalesce in SQL]]
`COALESCE` returns the **first non-NULL** value from a list of arguments. 
- Commonly used to replace `NULL` with a default value.

>[!example]- Example: `COALESCE` to avoid `NULL`:
>```SQL
>SELECT title, COALESCE(developer, 'Unknown') AS developer
>FROM videogames;
>```
>
>*Note: `AS` makes the displayed column be named **developer**, if we didn't include that it would be called **COALESCE(developer, 'Unknown')**.*

## NULLIF: Turn a value into `NULL`
`NULLIF(a, b)` returns `NULL` **if `a = b`**. Otherwise, it returns **`a`**.
- Commonly used to avoid **divisions by 0**.

>[!example]- Example: `NULLIF` to avoid 0-divisions:
>
>```SQL
>SELECT title, playtime_hours / NULLIF(year - 2000, 0) AS ratio
>FROM videogames;
>```
>
>*If `year` is 2000, division would fail (0/0). `NULLIF` turns the denomiator into `NULL` making the result simply `NULL` instead of an error.*


---
# References
[AprendeSQL from midudev - Valores Nulos](https://www.aprendesql.dev/valores-nulos)
