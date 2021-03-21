# OCT

## Syntax

```sql
OCT(N)
```

## Description

Returns a string representation of the octal value of N, where N is a longlong ([BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint)) number. This is equivalent to [CONV(N,10,8)](/built-in-functions/numeric-functions/conv). Returns NULL if N is NULL.

## Examples

```sql
SELECT OCT(34);
+---------+
| OCT(34) |
+---------+
| 42      |
+---------+

SELECT OCT(12);
+---------+
| OCT(12) |
+---------+
| 14      |
+---------+
```

## See Also

- [CONV()](/built-in-functions/numeric-functions/conv)
- [BIN()](/built-in-functions/string-functions/bin)
- [HEX()](/built-in-functions/string-functions/hex)