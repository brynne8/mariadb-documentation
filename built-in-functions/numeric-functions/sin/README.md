# SIN

## Syntax

```sql
SIN(X)
```

## Description

Returns the sine of X, where X is given in radians.

## Examples

```sql
SELECT SIN(1.5707963267948966);
+-------------------------+
| SIN(1.5707963267948966) |
+-------------------------+
|                       1 |
+-------------------------+

SELECT SIN(PI());
+----------------------+
| SIN(PI())            |
+----------------------+
| 1.22460635382238e-16 |
+----------------------+

SELECT ROUND(SIN(PI()));
+------------------+
| ROUND(SIN(PI())) |
+------------------+
|                0 |
+------------------+
```