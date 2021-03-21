# CRC32

## Syntax

```sql
CRC32(expr)
```

## Description

Computes a cyclic redundancy check value and returns a 32-bit unsigned
value. The result is NULL if the argument is NULL. The argument is
expected to be a string and (if possible) is treated as one if it is
not.

## Examples

```sql
SELECT CRC32('MariaDB');
+------------------+
| CRC32('MariaDB') |
+------------------+
|       4227209140 |
+------------------+

SELECT CRC32('mariadb');
+------------------+
| CRC32('mariadb') |
+------------------+
|       2594253378 |
+------------------+
```