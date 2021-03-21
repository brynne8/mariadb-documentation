# IFNULL

## Syntax

```sql
IFNULL(expr1,expr2)
NVL(expr1,expr2)
```

## Description

If <em>`expr1`</em> is not NULL, IFNULL() returns <em>`expr1`</em>; otherwise it returns
<em>`expr2`</em>. IFNULL() returns a numeric or string value, depending on the
context in which it is used.

From [MariaDB 10.3](/kb/en/what-is-mariadb-103/), NVL() is an alias for IFNULL().

## Examples

```sql
SELECT IFNULL(1,0); 
+-------------+
| IFNULL(1,0) |
+-------------+
|           1 |
+-------------+

SELECT IFNULL(NULL,10);
+-----------------+
| IFNULL(NULL,10) |
+-----------------+
|              10 |
+-----------------+

SELECT IFNULL(1/0,10);
+----------------+
| IFNULL(1/0,10) |
+----------------+
|        10.0000 |
+----------------+

SELECT IFNULL(1/0,'yes');
+-------------------+
| IFNULL(1/0,'yes') |
+-------------------+
| yes               |
+-------------------+
```

## See Also

- [NULL values](/columns-storage-engines-and-plugins/data-types/null-values)
- [IS NULL operator](/sql-statements-structure/operators/comparison-operators/is-null)
- [IS NOT NULL operator](/sql-statements-structure/operators/comparison-operators/is-not-null)
- [COALESCE function](/sql-statements-structure/operators/comparison-operators/coalesce)
- [NULLIF function](/built-in-functions/control-flow-functions/nullif)
- [CONNECT data types](/kb/en/connect-data-types/#null-handling)