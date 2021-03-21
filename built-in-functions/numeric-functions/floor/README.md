# FLOOR

## Syntax

```sql
FLOOR(X)
```

## Description

Returns the largest integer value not greater than X.

## Examples

```sql
SELECT FLOOR(1.23);
+-------------+
| FLOOR(1.23) |
+-------------+
|           1 |
+-------------+

SELECT FLOOR(-1.23);
+--------------+
| FLOOR(-1.23) |
+--------------+
|           -2 |
+--------------+
```