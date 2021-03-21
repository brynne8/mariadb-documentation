# OCTET_LENGTH

## Syntax

```sql
OCTET_LENGTH(str)
```

## Description

`OCTET_LENGTH()` returns the length of the given string, in octets (bytes). This is a synonym for [LENGTHB()](/built-in-functions/string-functions/lengthb/), and, when [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle/#functions) is not set, a synonym for [LENGTH()](/built-in-functions/string-functions/length/).

A multi-byte character counts as multiple bytes. This means that for a string containing five two-byte characters, `OCTET_LENGTH()` returns 10, whereas [CHAR_LENGTH()](/built-in-functions/string-functions/char_length/) returns 5.

If `str` is not a string value, it is converted into a string. If `str` is `NULL`, the function returns `NULL`.

## Examples

When [Oracle mode](/kb/en/sql_modeoracle/) from [MariaDB 10.3](/kb/en/what-is-mariadb-103/) is not set:

```sql
SELECT CHAR_LENGTH('π'), LENGTH('π'), LENGTHB('π'), OCTET_LENGTH('π');
+-------------------+--------------+---------------+--------------------+
| CHAR_LENGTH('π')  | LENGTH('π')  | LENGTHB('π')  | OCTET_LENGTH('π')  |
+-------------------+--------------+---------------+--------------------+
|                 1 |            2 |             2 |                  2 |
+-------------------+--------------+---------------+--------------------+
```

In [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle/#functions):

```sql
SELECT CHAR_LENGTH('π'), LENGTH('π'), LENGTHB('π'), OCTET_LENGTH('π');
+-------------------+--------------+---------------+--------------------+
| CHAR_LENGTH('π')  | LENGTH('π')  | LENGTHB('π')  | OCTET_LENGTH('π')  |
+-------------------+--------------+---------------+--------------------+
|                 1 |            1 |             2 |                  2 |
+-------------------+--------------+---------------+--------------------+
```

## See Also

- [CHAR_LENGTH()](/built-in-functions/string-functions/char_length/)
- [LENGTH()](/built-in-functions/string-functions/length/)
- [LENGTHB()](/built-in-functions/string-functions/lengthb/)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#simple-syntax-compatibility)