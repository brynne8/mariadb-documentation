# RIGHT

## Syntax

```sql
RIGHT(str,len)
```

## Description

Returns the rightmost <em>`len`</em> characters from the string <em>`str`</em>, or NULL if
any argument is NULL.

## Examples

```sql
SELECT RIGHT('MariaDB', 2);
+---------------------+
| RIGHT('MariaDB', 2) |
+---------------------+
| DB                  |
+---------------------+
```