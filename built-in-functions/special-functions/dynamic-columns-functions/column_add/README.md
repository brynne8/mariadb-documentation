# COLUMN_ADD

##### MariaDB starting with [5.3](/kb/en/what-is-mariadb-53/)

The [Dynamic columns](/sql-statements-structure/nosql/dynamic-columns/) feature was introduced in [MariaDB 5.3](/kb/en/what-is-mariadb-53/).

## Syntax

```sql
COLUMN_ADD(dyncol_blob, column_nr, value [as type], [column_nr, value [as type]]...);
COLUMN_ADD(dyncol_blob, column_name, value [as type], [column_name, value [as type]]...);
```

## Description

Adds or updates [dynamic columns](/sql-statements-structure/nosql/dynamic-columns/).

- <strong>`dyncol_blob`</strong> must be either a valid dynamic columns blob (for example, `COLUMN_CREATE` returns such blob), or an empty string.
- <strong>`column_name`</strong> specifies the name of the column to be added. If `dyncol_blob` already has a column with this name, it will be overwritten.
- <strong>`value`</strong> specifies the new value for the column.  Passing a NULL value will cause the column to be deleted.
- <strong>`as type`</strong> is optional. See [#datatypes](#datatypes) section for a discussion about types.

The return value is a dynamic column blob after the modifications.

## Examples

```sql
-- MariaDB 5.3+:
UPDATE tbl SET dyncol_blob=COLUMN_ADD(dyncol_blob, 1 /*column id*/, "value") WHERE id=1;
-- MariaDB 10.0.1+:
UPDATE t1 SET dyncol_blob=COLUMN_ADD(dyncol_blob, "column_name", "value") WHERE id=1;
```

Note: `COLUMN_ADD()` is a regular function (just like
[CONCAT()](/built-in-functions/string-functions/concat/)), hence, in order to update the value in the table
you have to use the <code>UPDATE ... SET dynamic_col=COLUMN_ADD(dynamic_col,
....) </code> pattern.