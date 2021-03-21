# HOUR

## Syntax

```sql
HOUR(time)
```

## Description

Returns the hour for time. The range of the return value is 0 to 23
for time-of-day values. However, the range of `TIME` values actually is
much larger, so HOUR can return values greater than 23.

The return value is always positive, even if a negative `TIME` value is provided.

## Examples

```sql
SELECT HOUR('10:05:03');
+------------------+
| HOUR('10:05:03') |
+------------------+
|               10 |
+------------------+

SELECT HOUR('272:59:59');
+-------------------+
| HOUR('272:59:59') |
+-------------------+
|               272 |
+-------------------+
```

Difference between [EXTRACT (HOUR FROM ...)](/built-in-functions/date-time-functions/extract) (&gt;= [MariaDB 10.0.7](/kb/en/mariadb-1007-release-notes/) and [MariaDB 5.5.35](/kb/en/mariadb-5535-release-notes/)) and `HOUR`:

```sql
SELECT EXTRACT(HOUR FROM '26:30:00'), HOUR('26:30:00');
+-------------------------------+------------------+
| EXTRACT(HOUR FROM '26:30:00') | HOUR('26:30:00') |
+-------------------------------+------------------+
|                             2 |               26 |
+-------------------------------+------------------+
```

## See Also

- [Date and Time Units](/built-in-functions/date-time-functions/date-and-time-units)
- [Date and Time Literals](/sql-statements-structure/sql-language-structure/date-and-time-literals)
- [EXTRACT()](/built-in-functions/date-time-functions/extract)