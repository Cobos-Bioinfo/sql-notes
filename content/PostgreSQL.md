**Date**: 2026-08-06
**Tags**: [[SQL]], [[Relational Databases]], [[Common Relational Databases]]

>[!help]- Personal Note: How to run Postgres in my WSL2?
>`sudo -u postgres psql`: Connect as the default postgres user then create your user role with sudo privileges:
>```
>CREATE USER cobos WITH SUPERUSER CREATEDB CREATEROLE;
>ALTER USER cobos WITH PASSWORD 'your_password_here';  -- Optional
>\q
>```
>Now connect with `psql`

Some **features**:
- **Advanced Data Types**: Native JSON/JSONB, arrays, hstore, geospatial types with PostGIS, ranges, custom types
- **Extensibility**: Create your **own data types**, operators, functions in multiple languages (Python, JS, ...)
- **Data Integrity**: Full support for **ACID** [[Transactions]], advanced [[Create Tables#Constraints]], triggers.
- **Modern ecosystem**: Tools like [[Supabase]], [[Neon]], [[Vercel Postgres]] have made it the default option in modern development.

See [[PostgreSQL Commands]]




---
# References
[]()
[AprendeSQL from midudev - Motores de Bases de Datos](https://www.aprendesql.dev/motores-de-bases-de-datos)
