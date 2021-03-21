# DATE_ADD

## Syntax

```sql
DATE_ADD(date,INTERVAL expr unit)
```

## Description

Performs date arithmetic. The <em>date</em> argument specifies the
starting date or datetime value. <em>expr</em> is an expression specifying the
interval value to be added or subtracted from the starting date. <em>expr</em> is a
string; it may start with a "`-`" for negative intervals. <em>unit</em> is a
keyword indicating the units in which the expression should be interpreted. See [Date and Time Units](/built-in-functions/date-time-functions/date-and-time-units/) for a complete list of permitted units.

See also [DATE_SUB()](/built-in-functions/date-time-functions/date_sub/).

## Examples

```sql
SELECT '2008-12-31 23:59:59' + INTERVAL 1 SECOND;
+-------------------------------------------+
| '2008-12-31 23:59:59' + INTERVAL 1 SECOND |
+-------------------------------------------+
| 2009-01-01 00:00:00                       |
+-------------------------------------------+
```

```sql
SELECT INTERVAL 1 DAY + '2008-12-31';
+-------------------------------+
| INTERVAL 1 DAY + '2008-12-31' |
+-------------------------------+
| 2009-01-01                    |
+-------------------------------+
```

```sql
SELECT '2005-01-01' - INTERVAL 1 SECOND;
+----------------------------------+
| '2005-01-01' - INTERVAL 1 SECOND |
+----------------------------------+
| 2004-12-31 23:59:59              |
+----------------------------------+
```

```sql
SELECT DATE_ADD('2000-12-31 23:59:59', INTERVAL 1 SECOND);
+----------------------------------------------------+
| DATE_ADD('2000-12-31 23:59:59', INTERVAL 1 SECOND) |
+----------------------------------------------------+
| 2001-01-01 00:00:00                                |
+----------------------------------------------------+
```

```sql
SELECT DATE_ADD('2010-12-31 23:59:59', INTERVAL 1 DAY);
+-------------------------------------------------+
| DATE_ADD('2010-12-31 23:59:59', INTERVAL 1 DAY) |
+-------------------------------------------------+
| 2011-01-01 23:59:59                             |
+-------------------------------------------------+
```

```sql
SELECT DATE_ADD('2100-12-31 23:59:59', INTERVAL '1:1' MINUTE_SECOND);
+---------------------------------------------------------------+
| DATE_ADD('2100-12-31 23:59:59', INTERVAL '1:1' MINUTE_SECOND) |
+---------------------------------------------------------------+
| 2101-01-01 00:01:00                                           |
+---------------------------------------------------------------+
```

```sql
SELECT DATE_ADD('1900-01-01 00:00:00', INTERVAL '-1 10' DAY_HOUR);
+------------------------------------------------------------+
| DATE_ADD('1900-01-01 00:00:00', INTERVAL '-1 10' DAY_HOUR) |
+------------------------------------------------------------+
| 1899-12-30 14:00:00                                        |
+------------------------------------------------------------+
```

```sql
SELECT DATE_ADD('1992-12-31 23:59:59.000002', INTERVAL '1.999999' SECOND_MICROSECOND);
+--------------------------------------------------------------------------------+
| DATE_ADD('1992-12-31 23:59:59.000002', INTERVAL '1.999999' SECOND_MICROSECOND) |
+--------------------------------------------------------------------------------+
| 1993-01-01 00:00:01.000001                                                     |
+--------------------------------------------------------------------------------+
```