# Postgres (and PostGIS)

Postgres organizes things into **schemas**. The `public` schema is the default for user created data tables, and the `pg_catalog` and `information_schema` schemas are used for holding functions, metadata, and other implementation information.

* More about the [Information Schema](https://www.postgresql.org/docs/13/information-schema.html)
* More about [System Catalogs (pg_catalog schema)](https://www.postgresql.org/docs/13/catalogs.html)


## Util Queries
### Show all tables in the public schema
```sql
SELECT table_name 
FROM information_schema.tables
WHERE table_schema = 'public';
```

### Show all columns and column metadata
```sql
SELECT *
FROM information_schema.columns;
```

### Show all database roles
```sql
SELECT *
FROM pg_catalog.pg_roles;
```

### Get the primary key of a table
```sql
SELECT c.column_name, c.data_type
FROM information_schema.table_constraints tc 
JOIN information_schema.constraint_column_usage AS ccu USING (constraint_schema, constraint_name) 
JOIN information_schema.columns AS c ON c.table_schema = tc.constraint_schema
  AND tc.table_name = c.table_name AND ccu.column_name = c.column_name
WHERE constraint_type = 'PRIMARY KEY' and tc.table_name = '<insert_table_name_here>';
```

### Create a schema

```sql
CREATE SCHEMA <schema_name>;
```