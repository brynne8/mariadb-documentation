# CONVERT

## Syntax

```sql
CONVERT(expr,type), CONVERT(expr USING transcoding_name)
```

## Description

The	`CONVERT()` and [CAST()](/built-in-functions/string-functions/cast) functions take a value of one type and produce a value of another type.

The type can be one of the following values:

- [BINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/binary)
- [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char)
- [DATE](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/date)
- [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime)
- [DECIMAL[(M[,D])](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/decimal)]
- [DOUBLE](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double)
- [FLOAT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/float) (from [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/))
- [INTEGER](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int) 
<ul start="1"><li>Short for `SIGNED INTEGER`
</li></ul>
- SIGNED [INTEGER]
- UNSIGNED [INTEGER]
- [TIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/time)
- [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar) (in [Oracle mode](/kb/en/sql_modeoracle/), from [MariaDB 10.3](/kb/en/what-is-mariadb-103/))

Note that in MariaDB, `INT` and `INTEGER` are the same thing.

`BINARY` produces a string with the [BINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/binary) data type.  If the optional length is given, `BINARY(N)` causes the cast to use no more than `N` bytes of the argument. Values shorter than the given number in bytes are padded with 0x00 bytes to make them equal the length value.

`CHAR(N)` causes the cast to use no more than the number of characters given in the argument.

The main difference between the [CAST()](/built-in-functions/string-functions/cast) and `CONVERT()` is that `CONVERT(expr,type)` is ODBC syntax while [CAST(expr as type)](/built-in-functions/string-functions/cast) and `CONVERT(... USING ...)` are SQL92 syntax.

`CONVERT()` with `USING` is used to convert data between different [character sets](/kb/en/data-types-character-sets-and-collations/). In MariaDB, transcoding names are the same as the
corresponding character set names. For example, this statement
converts the string 'abc' in the default character set to the
corresponding string in the utf8 character set:

```sql
SELECT CONVERT('abc' USING utf8);
```

## Examples

```sql
SELECT enum_col FROM tbl_name 
ORDER BY CAST(enum_col AS CHAR);
```

Converting a [BINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/binary) to string to permit the [LOWER](/built-in-functions/string-functions/lower) function to work:

```sql
SET @x = 'AardVark';

SET @x = BINARY 'AardVark';

SELECT LOWER(@x), LOWER(CONVERT (@x USING latin1));
+-----------+----------------------------------+
| LOWER(@x) | LOWER(CONVERT (@x USING latin1)) |
+-----------+----------------------------------+
| AardVark  | aardvark                         |
+-----------+----------------------------------+
```

## See Also

- [Character Sets and Collations](/kb/en/data-types-character-sets-and-collations/)