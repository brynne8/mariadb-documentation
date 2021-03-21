# MICROSECOND

## Syntax

```sql
MICROSECOND(expr)
```

## Description

Returns the microseconds from the time or datetime expression <em>expr</em> as a number in the range from 0 to 999999.

If <em>expr</em> is a time with no microseconds, zero is returned, while if <em>expr</em> is a date with no time, zero with a warning is returned.

## Examples

```sql
SELECT MICROSECOND('12:00:00.123456');
+--------------------------------+
| MICROSECOND('12:00:00.123456') |
+--------------------------------+
|                         123456 |
+--------------------------------+

SELECT MICROSECOND('2009-12-31 23:59:59.000010');
+-------------------------------------------+
| MICROSECOND('2009-12-31 23:59:59.000010') |
+-------------------------------------------+
|                                        10 |
+-------------------------------------------+

SELECT MICROSECOND('2013-08-07 12:13:14');
+------------------------------------+
| MICROSECOND('2013-08-07 12:13:14') |
+------------------------------------+
|                                  0 |
+------------------------------------+

SELECT MICROSECOND('2013-08-07');
+---------------------------+
| MICROSECOND('2013-08-07') |
+---------------------------+
|                         0 |
+---------------------------+
1 row in set, 1 warning (0.00 sec)

SHOW WARNINGS;
+---------+------+----------------------------------------------+
| Level   | Code | Message                                      |
+---------+------+----------------------------------------------+
| Warning | 1292 | Truncated incorrect time value: '2013-08-07' |
+---------+------+----------------------------------------------+
```

## See Also

- [Microseconds in MariaDB](/built-in-functions/date-time-functions/microseconds-in-mariadb)