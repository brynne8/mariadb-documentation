# CHARSET

## Syntax

```sql
CHARSET(str)
```

## Description

Returns the [character set](/kb/en/data-types-character-sets-and-collations/) of the string argument. If `str` is not a string, it is considered as a binary string (so the function returns 'binary'). This applies to `NULL`, too.  The return value is a string in the utf8 [character set](/kb/en/data-types-character-sets-and-collations/).

## Examples

```sql
SELECT CHARSET('abc');
+----------------+
| CHARSET('abc') |
+----------------+
| latin1         |
+----------------+

SELECT CHARSET(CONVERT('abc' USING utf8));
+------------------------------------+
| CHARSET(CONVERT('abc' USING utf8)) |
+------------------------------------+
| utf8                               |
+------------------------------------+

SELECT CHARSET(USER());
+-----------------+
| CHARSET(USER()) |
+-----------------+
| utf8            |
+-----------------+
```