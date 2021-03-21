# SECOND

## Syntax

```sql
SECOND(time)
```

## Description

Returns the second for a given `time` (which can include [microseconds](/built-in-functions/date-time-functions/microseconds-in-mariadb/)), in the range 0 to 59, or `NULL` if not given a valid time value.

## Examples

```sql
SELECT SECOND('10:05:03');
+--------------------+
| SECOND('10:05:03') |
+--------------------+
|                  3 |
+--------------------+

SELECT SECOND('10:05:01.999999');
+---------------------------+
| SECOND('10:05:01.999999') |
+---------------------------+
|                         1 |
+---------------------------+
```