# COLUMN_GET

##### MariaDB starting with [5.3](/kb/en/what-is-mariadb-53/)

The [Dynamic columns](/sql-statements-structure/nosql/dynamic-columns/) feature was introduced in [MariaDB 5.3](/kb/en/what-is-mariadb-53/).

## Syntax

```sql
COLUMN_GET(dyncol_blob, column_nr as type);
COLUMN_GET(dyncol_blob, column_name as type);
```

## Description

Gets the value of a dynamic column by its name. If no column with the given name exists, `NULL` will be returned.

<strong>`column_name as type`</strong> requires that one specify the datatype of the dynamic column they are reading.

This may seem counter-intuitive: why would one need to specify which datatype they're retrieving? Can't the dynamic columns system figure the datatype from the data being stored?

The answer is: SQL is a statically-typed language. The SQL interpreter needs to know the datatypes of all expressions before the query is run (for example, when one is using prepared statements and runs `"select COLUMN_GET(...)"`, the prepared statement API requires the server to inform the client about the datatype of the column being read before the query is executed and the server can see what datatype the column actually has).

### A note about lengths

If you're running queries like:

```sql
SELECT COLUMN_GET(blob, 'colname' as CHAR) ...
```

without specifying a maximum length (i.e. using #as CHAR#, not `as CHAR(n)`), MariaDB will report the maximum length of the resultset column to be 53,6870,911 for [MariaDB 5.3](/kb/en/what-is-mariadb-53/)-10.0.0 and 16,777,216 for [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/)+. This may cause excessive memory usage in some client libraries, because they try to pre-allocate a buffer of maximum resultset width. To avoid this problem, use CHAR(n) whenever you're using COLUMN_GET in the select list.

See [Dynamic Columns:Datatypes](/kb/en/dynamic-columns/#datatypes) for more information about datatypes.