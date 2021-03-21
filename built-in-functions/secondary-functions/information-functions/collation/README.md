# COLLATION

## Syntax

```sql
COLLATION(str)
```

## Description

Returns the collation of the string argument. If `str` is not a string, it is considered as a binary string (so the function returns 'binary'). This applies to `NULL`, too.  The return value is a string in the utf8 [character set](/kb/en/data-types-character-sets-and-collations/).

See [Character Sets and Collations](/kb/en/data-types-character-sets-and-collations/).

## Examples

```sql
SELECT COLLATION('abc');
+-------------------+
| COLLATION('abc')  |
+-------------------+
| latin1_swedish_ci |
+-------------------+

SELECT COLLATION(_utf8'abc');
+-----------------------+
| COLLATION(_utf8'abc') |
+-----------------------+
| utf8_general_ci       |
+-----------------------+
```

## See Also

- [String literals](/sql-statements-structure/sql-language-structure/string-literals)
- [CAST()](/built-in-functions/string-functions/cast)
- [CONVERT()](/built-in-functions/string-functions/convert)