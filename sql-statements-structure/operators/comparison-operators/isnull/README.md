# ISNULL

## Syntax

```sql
ISNULL(expr)
```

## Description

If <em>`expr`</em> is NULL, ISNULL() returns 1, otherwise it returns 0.

See also [NULL Values in MariaDB](/kb/en/null-values-in-mariadb/).

## Examples

```sql
SELECT ISNULL(1+1);
+-------------+
| ISNULL(1+1) |
+-------------+
|           0 |
+-------------+

SELECT ISNULL(1/0);
+-------------+
| ISNULL(1/0) |
+-------------+
|           1 |
+-------------+
```