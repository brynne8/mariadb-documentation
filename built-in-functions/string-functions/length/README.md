# LENGTH

## Syntax

```sql
LENGTH(str)
```

## Description

Returns the length of the string `str`.

In the default mode, when [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle/#functions) is not set, the length is measured in bytes. In this case, a multi-byte character counts as multiple bytes. This means that for a string
containing five two-byte characters, `LENGTH()` returns 10, whereas [CHAR_LENGTH()](/built-in-functions/string-functions/char_length) returns 5.

When running [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle/#functions), the length is measured in characters, and `LENGTH` is a synonym for [CHAR_LENGTH()](/built-in-functions/string-functions/char_length).

If `str` is not a string value, it is converted into a string. If `str` is `NULL`, the function returns `NULL`.

## Examples

```sql
SELECT LENGTH('MariaDB');
+-------------------+
| LENGTH('MariaDB') |
+-------------------+
|                 7 |
+-------------------+
```

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
- [LENGTHB()](/built-in-functions/string-functions/lengthb)
- [OCTET_LENGTH()](/built-in-functions/string-functions/octet_length)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle/#simple-syntax-compatibility)