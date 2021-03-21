# CEILING

## Syntax

```sql
CEILING(X)
```

## Description

Returns the smallest integer value not less than X.

## Examples

```sql
SELECT CEILING(1.23);
+---------------+
| CEILING(1.23) |
+---------------+
|             2 |
+---------------+

SELECT CEILING(-1.23);
+----------------+
| CEILING(-1.23) |
+----------------+
|             -1 |
+----------------+
```