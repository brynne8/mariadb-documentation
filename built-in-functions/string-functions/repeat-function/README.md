# REPEAT Function

## Syntax

```sql
REPEAT(str,count)
```

## Description

Returns a string consisting of the string `str` repeated `count` times. If
`count` is less than 1, returns an empty string. Returns NULL if `str` or
`count` are NULL.

## Examples

```sql
SELECT QUOTE(REPEAT('MariaDB ',4));
+------------------------------------+
| QUOTE(REPEAT('MariaDB ',4))        |
+------------------------------------+
| 'MariaDB MariaDB MariaDB MariaDB ' |
+------------------------------------+
```