# EXP

## Syntax

```sql
EXP(X)
```

## Description

Returns the value of e (the base of natural logarithms) raised to the
power of X. The inverse of this function is [LOG()](/built-in-functions/numeric-functions/log/) (using a single
argument only) or [LN()](/built-in-functions/numeric-functions/ln/).

If `X` is `NULL`, this function returns `NULL`.

## Examples

```sql
SELECT EXP(2);
+------------------+
| EXP(2)           |
+------------------+
| 7.38905609893065 |
+------------------+

SELECT EXP(-2);
+--------------------+
| EXP(-2)            |
+--------------------+
| 0.1353352832366127 |
+--------------------+

SELECT EXP(0);
+--------+
| EXP(0) |
+--------+
|      1 |
+--------+

SELECT EXP(NULL);
+-----------+
| EXP(NULL) |
+-----------+
|      NULL |
+-----------+
```