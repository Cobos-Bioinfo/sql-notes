**Date**: 2026-08-23
**Tags**: [[SQL]]

As seen in [[SQL Commands]], `UPDATE` allows **modifying existing data** in a table.
- It will usually be **combined** with `WHERE` to only modify specific records.

```SQL
UPDATE <table_name> 
SET <column_name> = <new_value>, <column_name_2> = <new_value_2> 
WHERE <condition>;
```

>[!warning]- Warning: If `WHERE` is **NOT** employed, **ALL** records will be updated.
>*Rarely a desired behavior*

## Update with Calculation

```SQL
UPDATE videogames
SET playtime_hours = playtime_hours + 10
WHERE developer = 'Nintendo';
```

## Update Multiple Columns

Separating the different columns in the **SET** clause with `,`. This is more **efficient** than multiple `UPDATE` queries since the database will only need to locate the record once.

``` SQL
UPDATE videogames
SET develoepr = 'CD Projekt', year = 2016
WHERE title = 'The Witcher 3';
```

>[!tip]- Tip: Run `SELECT` before `UPDATE`
>Using `SELELCT` to view/check the records that you want to `UPDATE`
>
>```
>-- Check what records are about to be modified;
>SELECT * FROM videogames WHERE developer = 'Nintendo'
>
>-- If expected result, run the UPDATE
>UPDATE videogames
>SET playtime_hours = playtime_hours + 10
>WHERE developer = 'Nintendo';
>```




---
# References
[AprendeSQL from midudev - Modificando Datos](https://www.aprendesql.dev/modificando-datos)
