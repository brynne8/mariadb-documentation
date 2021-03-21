# DIV

## Syntax

```sql
DIV
```

## Description

Integer division. Similar to [FLOOR()](/built-in-functions/numeric-functions/floor), but is safe with [BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint) values.
Incorrect results may occur for non-integer operands that exceed BIGINT range.

If the `ERROR_ON_DIVISION_BY_ZERO` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) is used, a division by zero produces an error. Otherwise, it returns NULL.

The remainder of a division can be obtained using the [MOD](/built-in-functions/numeric-functions/mod) operator.

## Examples

```sql
SELECT 300 DIV 7;
+-----------+
| 300 DIV 7 |
+-----------+
|        42 |
+-----------+

SELECT 300 DIV 0;
+-----------+
| 300 DIV 0 |
+-----------+
|      NULL |
+-----------+
```