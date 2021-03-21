# IS NOT NULL

## Syntax

```sql
IS NOT NULL
```

## Description

Tests whether a value is not NULL. See also [NULL Values in MariaDB](/kb/en/null-values-in-mariadb/).

## Examples

```sql
SELECT 1 IS NOT NULL, 0 IS NOT NULL, NULL IS NOT NULL;
+---------------+---------------+------------------+
| 1 IS NOT NULL | 0 IS NOT NULL | NULL IS NOT NULL |
+---------------+---------------+------------------+
|             1 |             1 |                0 |
+---------------+---------------+------------------+
```

## See also

- [NULL values](/columns-storage-engines-and-plugins/data-types/null-values/)
- [IS NULL operator](/sql-statements-structure/operators/comparison-operators/is-null/)
- [COALESCE function](/sql-statements-structure/operators/comparison-operators/coalesce/)
- [IFNULL function](/built-in-functions/control-flow-functions/ifnull/)
- [NULLIF function](/built-in-functions/control-flow-functions/nullif/)
- [CONNECT data types](/kb/en/connect-data-types/#null-handling)