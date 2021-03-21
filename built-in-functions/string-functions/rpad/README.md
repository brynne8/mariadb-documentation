# RPAD

## Syntax

```sql
RPAD(str, len [, padstr])
```

## Description

Returns the string `str`, right-padded with the string `padstr` to a
length of `len` characters. If `str` is longer than `len`, the return value
is shortened to `len` characters. If `padstr` is omitted, the RPAD function pads spaces.

Prior to [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), the `padstr` parameter was mandatory.

Returns NULL if given a NULL argument. If the result is empty (a length of zero), returns either an empty string, or, from [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/) with [SQL_MODE=Oracle](/kb/en/sql_modeoracle-from-mariadb-103/), NULL.

The Oracle mode version of the function can be accessed outside of Oracle mode by using `RPAD_ORACLE` as the function name.

## Examples

```sql
SELECT RPAD('hello',10,'.');
+----------------------+
| RPAD('hello',10,'.') |
+----------------------+
| hello.....           |
+----------------------+

SELECT RPAD('hello',2,'.');
+---------------------+
| RPAD('hello',2,'.') |
+---------------------+
| he                  |
+---------------------+
```

From [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), with the pad string defaulting to space.

```sql
SELECT RPAD('hello',30);
+--------------------------------+
| RPAD('hello',30)               |
+--------------------------------+
| hello                          |
+--------------------------------+
```

Oracle mode version from [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/):

```sql
SELECT RPAD('',0),RPAD_ORACLE('',0);
+------------+-------------------+
| RPAD('',0) | RPAD_ORACLE('',0) |
+------------+-------------------+
|            | NULL              |
+------------+-------------------+
```

## See Also

- [LPAD](/built-in-functions/string-functions/lpad/) - Left-padding instead of right-padding.