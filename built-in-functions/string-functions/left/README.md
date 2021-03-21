# LEFT

## Syntax

```sql
LEFT(str,len)
```

## Description

Returns the leftmost `len` characters from the string `str`, or NULL if
any argument is NULL.

## Examples

```sql
SELECT LEFT('MariaDB', 5);
+--------------------+
| LEFT('MariaDB', 5) |
+--------------------+
| Maria              |
+--------------------+
```