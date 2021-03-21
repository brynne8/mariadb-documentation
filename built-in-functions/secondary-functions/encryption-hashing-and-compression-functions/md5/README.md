# MD5

## Syntax

```sql
MD5(str)
```

## Description

Calculates an MD5 128-bit checksum for the string.

The return value is a 32-hex digit string, and as of [MariaDB 5.5](/kb/en/what-is-mariadb-55/), is a nonbinary string in the connection [character set and collation](/kb/en/data-types-character-sets-and-collations/), determined by the values of the [character_set_connection](/kb/en/server-system-variables/#character_set_connection) and [collation_connection](/kb/en/server-system-variables/#collation_connection) system variables. Before 5.5, the return value was a binary string.

NULL is returned if the argument was NULL.

## Examples

```sql
SELECT MD5('testing');
+----------------------------------+
| MD5('testing')                   |
+----------------------------------+
| ae2b1fca515949e5d54fb22b8ed95575 |
+----------------------------------+
```