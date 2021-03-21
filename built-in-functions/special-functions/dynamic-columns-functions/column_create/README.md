# COLUMN_CREATE

##### MariaDB starting with [5.3](/kb/en/what-is-mariadb-53/)

The [Dynamic columns](/sql-statements-structure/nosql/dynamic-columns/) feature was introduced in [MariaDB 5.3](/kb/en/what-is-mariadb-53/).

## Syntax

```sql
COLUMN_CREATE(column_nr, value [as type], [column_nr, value [as type]]...);
COLUMN_CREATE(column_name, value [as type], [column_name, value [as type]]...);
```

## Description

Returns a [dynamic columns](/sql-statements-structure/nosql/dynamic-columns/) blob that stores the specified columns with values.

The return value is suitable for

- storing in a table
- further modification with other dynamic columns functions

The <strong>`as type`</strong> part allows one to specify the value type. In most cases,
this is redundant because MariaDB will be able to deduce the type of the
value. Explicit type specification may be needed when the type of the value is
not apparent. For example, a literal `'2012-12-01'` has a CHAR type by
default, one will need to specify `'2012-12-01' AS DATE` to have it stored as
a date. See [Dynamic Columns:Datatypes](/kb/en/dynamic-columns/#datatypes) for further details.

## Examples

```sql
-- MariaDB 5.3+:
INSERT INTO tbl SET dyncol_blob=COLUMN_CREATE(1 /*column id*/, "value");
-- MariaDB 10.0.1+:
INSERT INTO tbl SET dyncol_blob=COLUMN_CREATE("column_name", "value");
```