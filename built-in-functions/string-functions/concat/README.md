# CONCAT

## Syntax

```sql
CONCAT(str1,str2,...)
```

## Description

Returns the string that results from concatenating the arguments. May have one or more arguments. If all arguments are non-binary strings, the result is a non-binary string. If the arguments include any binary strings, the result is a binary string. A numeric argument is converted to its equivalent binary string form; if you want to avoid that, you can use an explicit type cast, as in this example:

```sql
SELECT CONCAT(CAST(int_col AS CHAR), char_col);
```

`CONCAT()` returns `NULL` if any argument is `NULL`.

A `NULL` parameter hides all information contained in other parameters from the result. Sometimes this is not desirable; to avoid this, you can:

- Use the [CONCAT_WS()](/built-in-functions/string-functions/concat_ws) function with an empty separator, because that function is `NULL`-safe.
- Use [IFNULL()](/built-in-functions/control-flow-functions/ifnull) to turn NULLs into empty strings.

### Oracle Mode

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

In [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#null-handling), `CONCAT` ignores [NULL](null).

## Examples

```sql
SELECT CONCAT('Ma', 'ria', 'DB');
+---------------------------+
| CONCAT('Ma', 'ria', 'DB') |
+---------------------------+
| MariaDB                   |
+---------------------------+

SELECT CONCAT('Ma', 'ria', NULL, 'DB');
+---------------------------------+
| CONCAT('Ma', 'ria', NULL, 'DB') |
+---------------------------------+
| NULL                            |
+---------------------------------+

SELECT CONCAT(42.0);
+--------------+
| CONCAT(42.0) |
+--------------+
| 42.0         |
+--------------+
```

Using `IFNULL()` to handle NULLs:

```sql
SELECT CONCAT('The value of @v is: ', IFNULL(@v, ''));
+------------------------------------------------+
| CONCAT('The value of @v is: ', IFNULL(@v, '')) |
+------------------------------------------------+
| The value of @v is:                            |
+------------------------------------------------+
```

In [Oracle mode](/kb/en/sql_modeoracle-from-mariadb-103/#null-handling), from [MariaDB 10.3](/kb/en/what-is-mariadb-103/):

```sql
SELECT CONCAT('Ma', 'ria', NULL, 'DB');
+---------------------------------+
| CONCAT('Ma', 'ria', NULL, 'DB') |
+---------------------------------+
| MariaDB                         |
+---------------------------------+
```

## See Also

- [GROUP_CONCAT()](/built-in-functions/aggregate-functions/group_concat)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#null-handling)