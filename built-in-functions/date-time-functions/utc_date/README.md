# UTC_DATE

## Syntax

```sql
UTC_DATE, UTC_DATE()
```

## Description

Returns the current [UTC date](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time) as a value in 'YYYY-MM-DD' or YYYYMMDD
format, depending on whether the function is used in a string or numeric context.

## Examples

```sql
SELECT UTC_DATE(), UTC_DATE() + 0;
+------------+----------------+
| UTC_DATE() | UTC_DATE() + 0 |
+------------+----------------+
| 2010-03-27 |       20100327 |
+------------+----------------+
```