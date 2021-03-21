# NULLIF

## Syntax

```sql
NULLIF(expr1,expr2)
```

## Description

Returns NULL if expr1 = expr2 is true, otherwise returns expr1. This is
the same as [CASE](/built-in-functions/control-flow-functions/case-operator) WHEN expr1 = expr2 THEN NULL ELSE expr1 END.

## Examples

```sql
SELECT NULLIF(1,1);
+-------------+
| NULLIF(1,1) |
+-------------+
|        NULL |
+-------------+

SELECT NULLIF(1,2);
+-------------+
| NULLIF(1,2) |
+-------------+
|           1 |
+-------------+
```

## See Also

- [NULL values](/columns-storage-engines-and-plugins/data-types/null-values)
- [IS NULL operator](/sql-statements-structure/operators/comparison-operators/is-null)
- [IS NOT NULL operator](/sql-statements-structure/operators/comparison-operators/is-not-null)
- [COALESCE function](/sql-statements-structure/operators/comparison-operators/coalesce)
- [IFNULL function](/built-in-functions/control-flow-functions/ifnull)
- [CONNECT data types](/kb/en/connect-data-types/#null-handling)