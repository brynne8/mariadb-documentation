# SUBSTRING_INDEX

## Syntax

```sql
SUBSTRING_INDEX(str,delim,count)
```

## Description

Returns the substring from string <em>`str`</em> before count occurrences of the
delimiter <em>`delim`</em>. If <em>`count`</em> is positive, everything to the left
of the final delimiter (counting from the left) is returned. If <em>`count`</em>
is negative, everything to the right of the final delimiter (counting from the
right) is returned. `SUBSTRING_INDEX()` performs a case-sensitive match when
searching for <em>`delim`</em>.

If any argument is `NULL`, returns `NULL`.

For example

```sql
SUBSTRING_INDEX('www.mariadb.org', '.', 2)
```

means "Return all of the characters up to the 2nd occurrence of ."

## Examples

```sql
SELECT SUBSTRING_INDEX('www.mariadb.org', '.', 2);
+--------------------------------------------+
| SUBSTRING_INDEX('www.mariadb.org', '.', 2) |
+--------------------------------------------+
| www.mariadb                                |
+--------------------------------------------+

SELECT SUBSTRING_INDEX('www.mariadb.org', '.', -2);
+---------------------------------------------+
| SUBSTRING_INDEX('www.mariadb.org', '.', -2) |
+---------------------------------------------+
| mariadb.org                                 |
+---------------------------------------------+
```

## See Also

- [INSTR()](/built-in-functions/string-functions/instr) - Returns the position of a string within a string
- [LOCATE()](/built-in-functions/string-functions/locate) - Returns the position of a string within a string
- [SUBSTRING()](/built-in-functions/string-functions/substring) - Returns a string based on position