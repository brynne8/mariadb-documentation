# LOCATE

## Syntax

```sql
LOCATE(substr,str), LOCATE(substr,str,pos)
```

## Description

The first syntax returns the position of the first occurrence of
substring `substr` in string `str`. The second syntax returns the position
of the first occurrence of substring `substr` in string `str`, starting at
position `pos`. Returns 0 if `substr` is not in `str`.

`LOCATE()` performs a case-insensitive search.

If any argument is `NULL`, returns `NULL.`

[INSTR()](/built-in-functions/string-functions/instr) is a synonym of `LOCATE()` without the third argument.

## Examples

```sql
SELECT LOCATE('bar', 'foobarbar');
+----------------------------+
| LOCATE('bar', 'foobarbar') |
+----------------------------+
|                          4 |
+----------------------------+

SELECT LOCATE('My', 'Maria');
+-----------------------+
| LOCATE('My', 'Maria') |
+-----------------------+
|                     0 |
+-----------------------+

SELECT LOCATE('bar', 'foobarbar', 5);
+-------------------------------+
| LOCATE('bar', 'foobarbar', 5) |
+-------------------------------+
|                             7 |
+-------------------------------+
```

## See Also

- [INSTR()](/built-in-functions/string-functions/instr) ; Returns the position of a string withing a string
- [SUBSTRING_INDEX()](/built-in-functions/string-functions/substring_index) ; Returns the substring from string before count occurrences of a delimiter