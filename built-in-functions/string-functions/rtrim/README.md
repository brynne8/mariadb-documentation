# RTRIM

## Syntax

```sql
RTRIM(str)
```

## Description

Returns the string `str` with trailing space characters removed.

Returns NULL if given a NULL argument. If the result is empty, returns either an empty string, or, from [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/) with [SQL_MODE=Oracle](/kb/en/sql_modeoracle-from-mariadb-103/), NULL.

The Oracle mode version of the function can be accessed outside of Oracle mode by using `RTRIM_ORACLE` as the function name.

## Examples

```sql
SELECT QUOTE(RTRIM('MariaDB    '));
+-----------------------------+
| QUOTE(RTRIM('MariaDB    ')) |
+-----------------------------+
| 'MariaDB'                   |
+-----------------------------+
```

Oracle mode version from [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/):

```sql
SELECT RTRIM(''),RTRIM_ORACLE('');
+-----------+------------------+
| RTRIM('') | RTRIM_ORACLE('') |
+-----------+------------------+
|           | NULL             |
+-----------+------------------+
```

## See Also

- [LTRIM](/built-in-functions/string-functions/ltrim/) - leading spaces removed
- [TRIM](/built-in-functions/string-functions/trim/) - removes all given prefixes or suffixes