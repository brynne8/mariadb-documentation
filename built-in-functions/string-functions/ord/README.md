# ORD

## Syntax

```sql
ORD(str)
```

## Description

If the leftmost character of the string `str` is a multi-byte character,
returns the code for that character, calculated from the numeric
values of its constituent bytes using this formula:

```sql
  (1st byte code)
+ (2nd byte code x 256)
+ (3rd byte code x 256 x 256) ...
```

If the leftmost character is not a multi-byte character, ORD() returns
the same value as the [ASCII()](/built-in-functions/string-functions/ascii) function.

## Examples

```sql
SELECT ORD('2');
+----------+
| ORD('2') |
+----------+
|       50 |
+----------+
```

## See Also

- [ASCII()](/built-in-functions/string-functions/ascii)  - Return ASCII value of first character
- [CHAR()](/built-in-functions/string-functions/char-function) - Create a character from an integer value