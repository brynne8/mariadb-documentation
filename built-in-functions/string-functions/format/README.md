# FORMAT

## Syntax

```sql
FORMAT(num, decimal_position[, locale])
```

## Description

Formats the given number for display as a string, adding separators to appropriate position and rounding the results to the given decimal position.  For instance, it would format `15233.345` to `15,233.35`.

If the given decimal position is `0`, it rounds to return no decimal point or fractional part.  You can optionally specify a [locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale) value to format numbers to the pattern appropriate for the given region.

## Examples

```sql
SELECT FORMAT(1234567890.09876543210, 4) AS 'Format';
+--------------------+
| Format             |
+--------------------+
| 1,234,567,890.0988 |
+--------------------+

SELECT FORMAT(1234567.89, 4) AS 'Format';
+----------------+
| Format         |
+----------------+
| 1,234,567.8900 |
+----------------+

SELECT FORMAT(1234567.89, 0) AS 'Format';
+-----------+
| Format    |
+-----------+
| 1,234,568 |
+-----------+

SELECT FORMAT(123456789,2,'rm_CH') AS 'Format';
+----------------+
| Format         |
+----------------+
| 123'456'789,00 |
+----------------+
```