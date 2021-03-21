# UNHEX

## Syntax

```sql
UNHEX(str)
```

## Description

Performs the inverse operation of [HEX](/built-in-functions/string-functions/hex)(str). That is, it interprets
each pair of hexadecimal digits in the argument as a number and
converts it to the character represented by the number. The resulting
characters are returned as a binary string.

If `str` is `NULL`, `UNHEX()` returns `NULL`.

## Examples

```sql
SELECT HEX('MariaDB');
+----------------+
| HEX('MariaDB') |
+----------------+
| 4D617269614442 |
+----------------+

SELECT UNHEX('4D617269614442');
+-------------------------+
| UNHEX('4D617269614442') |
+-------------------------+
| MariaDB                 |
+-------------------------+

SELECT 0x4D617269614442;
+------------------+
| 0x4D617269614442 |
+------------------+
| MariaDB          |
+------------------+

SELECT UNHEX(HEX('string'));
+----------------------+
| UNHEX(HEX('string')) |
+----------------------+
| string               |
+----------------------+

SELECT HEX(UNHEX('1267'));
+--------------------+
| HEX(UNHEX('1267')) |
+--------------------+
| 1267               |
+--------------------+
```

## See Also

- [Hexadecimal literals](/sql-statements-structure/sql-language-structure/hexadecimal-literals)
- [HEX()](/built-in-functions/string-functions/hex)
- [CONV()](/built-in-functions/numeric-functions/conv)