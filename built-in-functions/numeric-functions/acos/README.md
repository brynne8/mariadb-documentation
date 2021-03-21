# ACOS

## Syntax

```sql
ACOS(X)
```

## Description

Returns the arc cosine of `X`, that is, the value whose cosine is `X`.
Returns `NULL` if `X` is not in the range `-1` to `1`.

## Examples

```sql
SELECT ACOS(1);
+---------+
| ACOS(1) |
+---------+
|       0 |
+---------+

SELECT ACOS(1.0001);
+--------------+
| ACOS(1.0001) |
+--------------+
|         NULL |
+--------------+

SELECT ACOS(0);
+-----------------+
| ACOS(0)         |
+-----------------+
| 1.5707963267949 |
+-----------------+

SELECT ACOS(0.234);
+------------------+
| ACOS(0.234)      |
+------------------+
| 1.33460644244679 |
+------------------+
```