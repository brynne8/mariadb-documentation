# CHAR_LENGTH

## Syntax

```sql
CHAR_LENGTH(str)
CHARACTER_LENGTH(str)
```

## Description

Returns the length of the given string argument, measured in characters. A multi-byte character counts as a single character. This means that for a string containing five two-byte characters, [LENGTH()](/built-in-functions/string-functions/length/) (or [OCTET_LENGTH()](/built-in-functions/string-functions/octet_length/) in [Oracle mode](/kb/en/sql_modeoracle/)) returns 10, whereas `CHAR_LENGTH()` returns 5. If the argument is `NULL`, it returns `NULL`.

If the argument is not a string value, it is converted into a string.

It is synonymous with the `CHARACTER_LENGTH()` function.

## Examples

```sql
SELECT CHAR_LENGTH('MariaDB');
+------------------------+
| CHAR_LENGTH('MariaDB') |
+------------------------+
|                      7 |
+------------------------+
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

- [LENGTH()](/built-in-functions/string-functions/length/)
- [LENGTHB()](/built-in-functions/string-functions/lengthb/)
- [OCTET_LENGTH()](/built-in-functions/string-functions/octet_length/)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle/#simple-syntax-compatibility)