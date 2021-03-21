# CAST

## Syntax

```sql
CAST(expr AS type)
```

## Description

The `CAST()` function takes a value of one [type](/columns-storage-engines-and-plugins/data-types/) and produces a value of another type, similar to the [CONVERT()](/built-in-functions/string-functions/convert/) function.

The type can be one of the following values:

- [BINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/binary/)
- [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char/)
- [DATE](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/date/)
- [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime/)
- [DECIMAL[(M[,D])](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/decimal/)]
- [DOUBLE](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double/)
- [FLOAT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/float/) (from [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/))
- [INTEGER](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int/) 
<ul start="1"><li>Short for `SIGNED INTEGER`
</li></ul>
- SIGNED [INTEGER]
- UNSIGNED [INTEGER]
- [TIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/time/)
- [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/) (in [Oracle mode](/kb/en/sql_modeoracle/), from [MariaDB 10.3](/kb/en/what-is-mariadb-103/))

The main difference between `CAST` and [CONVERT()](/built-in-functions/string-functions/convert/) is that [CONVERT(expr,type)](/built-in-functions/string-functions/convert/) is ODBC syntax while `CAST(expr as type)` and [CONVERT(... USING ...)](/built-in-functions/string-functions/convert/) are SQL92 syntax.

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, you can use the `CAST()` function with the `INTERVAL` keyword.

##### MariaDB starting with [5.5.31](/kb/en/mariadb-5531-release-notes/)

Until [MariaDB 5.5.31](/kb/en/mariadb-5531-release-notes/), `X'HHHH'`, the standard SQL syntax for binary string literals, erroneously worked in the same way as `0xHHHH`. In 5.5.31 it was intentionally changed to behave as a string in all contexts (and never as a number).

This introduces an incompatibility with previous versions of MariaDB, and all versions of MySQL (see the example below).

## Examples

Simple casts:

```sql
SELECT CAST("abc" AS BINARY);
SELECT CAST("1" AS UNSIGNED INTEGER);
SELECT CAST(123 AS CHAR CHARACTER SET utf8)
```

Note that when one casts to [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char/) without specifying the character set, the [collation_connection](/kb/en/server-system-variables/#collation_connection) character set collation will be used. When used with `CHAR CHARACTER SET`, the default collation for that character set will be used.

```sql
SELECT COLLATION(CAST(123 AS CHAR));
+------------------------------+
| COLLATION(CAST(123 AS CHAR)) |
+------------------------------+
| latin1_swedish_ci            |
+------------------------------+

SELECT COLLATION(CAST(123 AS CHAR CHARACTER SET utf8));
+-------------------------------------------------+
| COLLATION(CAST(123 AS CHAR CHARACTER SET utf8)) |
+-------------------------------------------------+
| utf8_general_ci                                 |
+-------------------------------------------------+
```

If you also want to change the collation, you have to use the `COLLATE` operator:

```sql
SELECT COLLATION(CAST(123 AS CHAR CHARACTER SET utf8) 
  COLLATE utf8_unicode_ci);
+-------------------------------------------------------------------------+
| COLLATION(CAST(123 AS CHAR CHARACTER SET utf8) COLLATE utf8_unicode_ci) |
+-------------------------------------------------------------------------+
| utf8_unicode_ci                                                         |
+-------------------------------------------------------------------------+
```

Using `CAST()` to order an [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum/) field as a [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char/) rather than the internal numerical value:

```sql
CREATE TABLE enum_list (enum_field enum('c','a','b'));

INSERT INTO enum_list (enum_field) 
VALUES('c'),('a'),('c'),('b');

SELECT * FROM enum_list 
ORDER BY enum_field;
+------------+
| enum_field |
+------------+
| c          |
| c          |
| a          |
| b          |
+------------+

SELECT * FROM enum_list 
ORDER BY CAST(enum_field AS CHAR);
+------------+
| enum_field |
+------------+
| a          |
| b          |
| c          |
| c          |
+------------+
```

From [MariaDB 5.5.31](/kb/en/mariadb-5531-release-notes/), the following will trigger warnings, since `x'aa'` and `'X'aa'` no longer behave as a number. Previously, and in all versions of MySQL, no warnings are triggered since they did erroneously behave as a number:

```sql
SELECT CAST(0xAA AS UNSIGNED), CAST(x'aa' AS UNSIGNED), CAST(X'aa' AS UNSIGNED);
+------------------------+-------------------------+-------------------------+
| CAST(0xAA AS UNSIGNED) | CAST(x'aa' AS UNSIGNED) | CAST(X'aa' AS UNSIGNED) |
+------------------------+-------------------------+-------------------------+
|                    170 |                       0 |                       0 |
+------------------------+-------------------------+-------------------------+
1 row in set, 2 warnings (0.00 sec)

Warning (Code 1292): Truncated incorrect INTEGER value: '\xAA'
Warning (Code 1292): Truncated incorrect INTEGER value: '\xAA'
```

Casting to intervals:

```sql
SELECT CAST(2019-01-04 INTERVAL AS DAY_SECOND(2)) AS "Cast";

+-------------+
| Cast        |
+-------------+
| 00:20:17.00 |
+-------------+
```

## See Also

- [Supported data types](/columns-storage-engines-and-plugins/data-types/)
- [Microseconds in MariaDB](/built-in-functions/date-time-functions/microseconds-in-mariadb/)
- [String literals](/sql-statements-structure/sql-language-structure/string-literals/)
- [COLLATION()](/built-in-functions/secondary-functions/information-functions/collation/)
- [CONVERT()](/built-in-functions/string-functions/convert/)