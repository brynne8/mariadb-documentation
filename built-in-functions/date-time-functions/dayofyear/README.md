# DAYOFYEAR

## Syntax

```sql
DAYOFYEAR(date)
```

## Description

Returns the day of the year for date, in the range 1 to 366.

## Examples

```sql
SELECT DAYOFYEAR('2018-02-16');
+-------------------------+
| DAYOFYEAR('2018-02-16') |
+-------------------------+
|                      47 |
+-------------------------+
```