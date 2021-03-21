# POW

## Syntax

```sql
POW(X,Y)
```

## Description

Returns the value of X raised to the power of Y.

POWER() is a synonym.

## Examples

```sql
SELECT POW(2,3);
+----------+
| POW(2,3) |
+----------+
|        8 |
+----------+

SELECT POW(2,-2);
+-----------+
| POW(2,-2) |
+-----------+
|      0.25 |
+-----------+
```