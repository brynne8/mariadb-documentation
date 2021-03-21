# HEX

## Syntax

```sql
HEX(N_or_S)
```

## Description

If `N_or_S` is a number, returns a string representation of the hexadecimal
value of `N`, where `N` is a `longlong` ([BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint/)) number. This is equivalent to [CONV](/built-in-functions/numeric-functions/conv/)(N,10,16).

If `N_or_S` is a string, returns a hexadecimal string representation of
`N_or_S` where each byte of each character in `N_or_S` is converted to two hexadecimal
digits. If `N_or_S` is NULL, returns NULL. The inverse of this operation is performed by the [UNHEX](/built-in-functions/string-functions/unhex/)()
function.

<br>

##### MariaDB starting with [10.5.0](/kb/en/mariadb-1050-release-notes/)

HEX() with an [INET6](/columns-storage-engines-and-plugins/data-types/string-data-types/inet6/) argument returns a hexadecimal representation of the underlying 16-byte binary string.

## Examples

```sql
SELECT HEX(255);
+----------+
| HEX(255) |
+----------+
| FF       |
+----------+

SELECT 0x4D617269614442;
+------------------+
| 0x4D617269614442 |
+------------------+
| MariaDB          |
+------------------+

SELECT HEX('MariaDB');
+----------------+
| HEX('MariaDB') |
+----------------+
| 4D617269614442 |
+----------------+
```

From [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/):

```sql
SELECT HEX(CAST('2001:db8::ff00:42:8329' AS INET6));
+----------------------------------------------+
| HEX(CAST('2001:db8::ff00:42:8329' AS INET6)) |
+----------------------------------------------+
| 20010DB8000000000000FF0000428329             |
+----------------------------------------------+
```

## See Also

- [Hexadecimal literals](/sql-statements-structure/sql-language-structure/hexadecimal-literals/)
- [UNHEX()](/built-in-functions/string-functions/unhex/)
- [CONV()](/built-in-functions/numeric-functions/conv/)
- [BIN()](/built-in-functions/string-functions/bin/)
- [OCT()](/built-in-functions/numeric-functions/oct/)