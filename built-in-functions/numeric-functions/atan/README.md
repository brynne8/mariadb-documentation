# ATAN

## Syntax

```sql
ATAN(X)
```

## Description

Returns the arc tangent of X, that is, the value whose tangent is X.

## Examples

```sql
SELECT ATAN(2);
+--------------------+
| ATAN(2)            |
+--------------------+
| 1.1071487177940904 |
+--------------------+

SELECT ATAN(-2);
+---------------------+
| ATAN(-2)            |
+---------------------+
| -1.1071487177940904 |
+---------------------+
```