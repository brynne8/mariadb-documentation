# LTRIM

## Syntax

```sql
LTRIM(str)
```

## Description

Returns the string `str` with leading space characters removed.

Returns NULL if given a NULL argument. If the result is empty, returns either an empty string, or, from [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/) with [SQL_MODE=Oracle](/kb/en/sql_modeoracle-from-mariadb-103/), NULL.

The Oracle mode version of the function can be accessed outside of Oracle mode by using `LTRIM_ORACLE` as the function name.

## Examples

```sql
SELECT QUOTE(LTRIM('   MariaDB   '));
+-------------------------------+
| QUOTE(LTRIM('   MariaDB   ')) |
+-------------------------------+
| 'MariaDB   '                  |
+-------------------------------+
```

Oracle mode version from [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/):

```sql
SELECT LTRIM(''),LTRIM_ORACLE('');
+-----------+------------------+
| LTRIM('') | LTRIM_ORACLE('') |
+-----------+------------------+
|           | NULL             |
+-----------+------------------+
```

## See Also

- [RTRIM](/built-in-functions/string-functions/rtrim/) - trailing spaces removed
- [TRIM](/built-in-functions/string-functions/trim/) - removes all given prefixes or suffixes