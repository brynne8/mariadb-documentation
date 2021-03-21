# LENGTHB

##### MariaDB starting with [10.3.1](/kb/en/mariadb-1031-release-notes/)

Introduced in [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/) as part of the [Oracle compatibility enhancements](/kb/en/sql_modeoracle/).

## Syntax

```sql
LENGTHB(str)
```

## Description

`LENGTHB()` returns the length of the given string, in bytes. When [Oracle mode](/kb/en/sql_modeoracle/) is not set, this is a synonym for [LENGTH](/built-in-functions/string-functions/length).

A multi-byte character counts as multiple bytes. This means that for a string containing five two-byte characters, `LENGTHB()` returns 10, whereas [CHAR_LENGTH()](/built-in-functions/string-functions/char_length) returns 5.

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

- [CHAR_LENGTH()](/built-in-functions/string-functions/char_length)
- [LENGTH()](/built-in-functions/string-functions/length)
- [OCTET_LENGTH()](/built-in-functions/string-functions/octet_length)