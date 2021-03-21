# CONV

## Syntax

```sql
CONV(N,from_base,to_base)
```

## Description

Converts numbers between different number bases. Returns a string
representation of the number <em>`N`</em>, converted from base <em>`from_base`</em>
to base <em>`to_base`</em>.

Returns `NULL` if any argument is `NULL`, or if the second or third argument are not in the allowed range.

The argument <em>`N`</em> is interpreted as an integer, but may be specified as an
integer or a string. The minimum base is 2 and the maximum base is 36. If
<em>`to_base`</em> is a negative number, <em>`N`</em> is regarded as a signed number.
Otherwise, <em>`N`</em> is treated as unsigned. `CONV()` works with 64-bit
precision.

Some shortcuts for this function are also available: [BIN()](/built-in-functions/string-functions/bin), [OCT()](/built-in-functions/numeric-functions/oct), [HEX()](/built-in-functions/string-functions/hex), [UNHEX()](/built-in-functions/string-functions/unhex). Also, MariaDB allows [binary](/sql-statements-structure/sql-language-structure/binary-literals) literal values and [hexadecimal](/sql-statements-structure/sql-language-structure/hexadecimal-literals) literal values.

## Examples

```sql
SELECT CONV('a',16,2);
+----------------+
| CONV('a',16,2) |
+----------------+
| 1010           |
+----------------+

SELECT CONV('6E',18,8);
+-----------------+
| CONV('6E',18,8) |
+-----------------+
| 172             |
+-----------------+

SELECT CONV(-17,10,-18);
+------------------+
| CONV(-17,10,-18) |
+------------------+
| -H               |
+------------------+

SELECT CONV(12+'10'+'10'+0xa,10,10);
+------------------------------+
| CONV(12+'10'+'10'+0xa,10,10) |
+------------------------------+
| 42                           |
+------------------------------+
```