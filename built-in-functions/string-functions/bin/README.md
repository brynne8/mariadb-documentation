# BIN

## Syntax

```sql
BIN(N)
```

## Description

Returns a string representation of the binary value of the given longlong (that is, [BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint/)) number. This is equivalent to [CONV(N,10,2)](/built-in-functions/numeric-functions/conv/). The argument should be positive. If it is a `FLOAT`, it will be truncated. Returns `NULL` if the argument is `NULL`.

## Examples

```sql
SELECT BIN(12);
+---------+
| BIN(12) |
+---------+
| 1100    |
+---------+
```

## See Also

- [Binary literals](/sql-statements-structure/sql-language-structure/binary-literals/)
- [CONV()](/built-in-functions/numeric-functions/conv/)
- [OCT()](/built-in-functions/numeric-functions/oct/)
- [HEX()](/built-in-functions/string-functions/hex/)