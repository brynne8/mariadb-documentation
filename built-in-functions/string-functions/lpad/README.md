# LPAD

## Syntax

```sql
LPAD(str, len [,padstr])
```

## Description

Returns the string `str`, left-padded with the string `padstr` to a length
of `len` characters. If `str` is longer than `len`, the return value is
shortened to `len` characters. If `padstr` is omitted, the LPAD function pads spaces.

Prior to [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), the `padstr` parameter was mandatory.

Returns NULL if given a NULL argument. If the result is empty (zero length), returns either an empty string or, from [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/) with [SQL_MODE=Oracle](/kb/en/sql_modeoracle-from-mariadb-103/), NULL.

The Oracle mode version of the function can be accessed outside of Oracle mode by using `LPAD_ORACLE` as the function name.

## Examples

```sql
SELECT LPAD('hello',10,'.');
+----------------------+
| LPAD('hello',10,'.') |
+----------------------+
| .....hello           |
+----------------------+

SELECT LPAD('hello',2,'.');
+---------------------+
| LPAD('hello',2,'.') |
+---------------------+
| he                  |
+---------------------+
```

From [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), with the pad string defaulting to space.

```sql
SELECT LPAD('hello',10);
+------------------+
| LPAD('hello',10) |
+------------------+
|      hello       |
+------------------+
```

Oracle mode version from [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/):

```sql
SELECT LPAD('',0),LPAD_ORACLE('',0);
+------------+-------------------+
| LPAD('',0) | LPAD_ORACLE('',0) |
+------------+-------------------+
|            | NULL              |
+------------+-------------------+
```

## See Also

- [RPAD](/built-in-functions/string-functions/rpad) - Right-padding instead of left-padding.