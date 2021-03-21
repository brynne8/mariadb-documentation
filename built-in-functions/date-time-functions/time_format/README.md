# TIME_FORMAT

## Syntax

```sql
TIME_FORMAT(time,format)
```

## Description

This is used like the [DATE_FORMAT()](/built-in-functions/date-time-functions/date_format/) function, but the format string
may contain format specifiers only for hours, minutes, and seconds.
Other specifiers produce a NULL value or 0.

## Examples

```sql
SELECT TIME_FORMAT('100:00:00', '%H %k %h %I %l');
+--------------------------------------------+
| TIME_FORMAT('100:00:00', '%H %k %h %I %l') |
+--------------------------------------------+
| 100 100 04 04 4                            |
+--------------------------------------------+
```