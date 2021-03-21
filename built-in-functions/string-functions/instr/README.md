# INSTR

## Syntax

```sql
INSTR(str,substr)
```

## Description

Returns the position of the first occurrence of substring <em>substr</em> in
string <em>str</em>. This is the same as the two-argument form of [LOCATE()](/built-in-functions/string-functions/locate),
except that the order of the arguments is reversed.

`INSTR()` performs a case-insensitive search.

If any argument is `NULL`, returns `NULL`.

## Examples

```sql
SELECT INSTR('foobarbar', 'bar');
+---------------------------+
| INSTR('foobarbar', 'bar') |
+---------------------------+
|                         4 |
+---------------------------+

SELECT INSTR('My', 'Maria');
+----------------------+
| INSTR('My', 'Maria') |
+----------------------+
|                    0 |
+----------------------+
```

## See Also

- [INSTR()](/built-in-functions/string-functions/instr) ; Returns the position of a string withing a string
- [SUBSTRING_INDEX()](/built-in-functions/string-functions/substring_index) ; Returns the substring from string before count occurrences of a delimiter