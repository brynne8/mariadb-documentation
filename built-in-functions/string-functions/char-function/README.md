# CHAR Function

## Syntax

```sql
CHAR(N,... [USING charset_name])
```

## Description

`CHAR()` interprets each argument as an [INT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int) and returns a string consisting of the characters given by the code values of those integers. `NULL` values are skipped.  By default, `CHAR()` returns a binary string. To produce a string in a given [character set](/kb/en/data-types-character-sets-and-collations/), use the optional `USING` clause:

```sql
SELECT CHARSET(CHAR(0x65)), CHARSET(CHAR(0x65 USING utf8));
+---------------------+--------------------------------+
| CHARSET(CHAR(0x65)) | CHARSET(CHAR(0x65 USING utf8)) |
+---------------------+--------------------------------+
| binary              | utf8                           |
+---------------------+--------------------------------+
```

If `USING` is given and the result string is illegal for the given character set, a warning is issued. Also, if strict [SQL mode](/kb/en/sql_mode/) is enabled, the result from `CHAR()` becomes `NULL`.

## Examples

```sql
SELECT CHAR(77,97,114,'105',97,'68',66);
+----------------------------------+
| CHAR(77,97,114,'105',97,'68',66) |
+----------------------------------+
| MariaDB                          |
+----------------------------------+

SELECT CHAR(77,77.3,'77.3');
+----------------------+
| CHAR(77,77.3,'77.3') |
+----------------------+
| MMM                  |
+----------------------+
1 row in set, 1 warning (0.00 sec)

Warning (Code 1292): Truncated incorrect INTEGER value: '77.3'
```

## See Also

- [Character Sets and Collations](/kb/en/data-types-character-sets-and-collations/)
- [ASCII()](/built-in-functions/string-functions/ascii)  - Return ASCII value of first character
- [ORD()](/built-in-functions/string-functions/ord) - Return value for character in single or multi-byte character sets
- [CHR](/built-in-functions/string-functions/chr) - Similar, Oracle-compatible, function