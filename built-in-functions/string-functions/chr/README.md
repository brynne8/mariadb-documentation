# CHR

##### MariaDB starting with [10.3.1](/kb/en/mariadb-1031-release-notes/)

The `CHR()` function was introduced in [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/) to provide Oracle compatibility

## Syntax

```sql
CHR(N)
```

## Description

`CHR()` interprets each argument N as an integer and returns a [VARCHAR(1)](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar) string consisting of the character given by the code values of the integer. The character set and collation of the string are set according to the values of the <a undefined>character_set_database</a> and <a undefined>collation_database</a> system variables.

`CHR()` is similar to the [CHAR()](/built-in-functions/string-functions/char-function) function, but only accepts a single argument.

`CHR()` is available in all [sql_modes](/mariadb-administration/variables-and-modes/sql-mode).

## Examples

```sql
SELECT CHR(67);
+---------+
| CHR(67) |
+---------+
| C       |
+---------+

SELECT CHR('67');
+-----------+
| CHR('67') |
+-----------+
| C         |
+-----------+

SELECT CHR('C');
+----------+
| CHR('C') |
+----------+
|          |
+----------+
1 row in set, 1 warning (0.000 sec)

SHOW WARNINGS;
+---------+------+----------------------------------------+
| Level   | Code | Message                                |
+---------+------+----------------------------------------+
| Warning | 1292 | Truncated incorrect INTEGER value: 'C' |
+---------+------+----------------------------------------+
```

## See Also

- [Character Sets and Collations](/kb/en/data-types-character-sets-and-collations/)
- [ASCII()](/built-in-functions/string-functions/ascii)  - Return ASCII value of first character
- [ORD()](/built-in-functions/string-functions/ord) - Return value for character in single or multi-byte character sets
- [CHAR()](/built-in-functions/string-functions/char-function) - Similar function which accepts multiple integers