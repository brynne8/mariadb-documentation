# DATE_SUB

## Syntax

```sql
DATE_SUB(date,INTERVAL expr unit)
```

## Description

Performs date arithmetic. The <em>date</em> argument specifies the
starting date or datetime value. <em>expr</em> is an expression specifying the
interval value to be added or subtracted from the starting date. <em>expr</em> is a
string; it may start with a "`-`" for negative intervals. <em>unit</em> is a
keyword indicating the units in which the expression should be interpreted. See [Date and Time Units](/built-in-functions/date-time-functions/date-and-time-units) for a complete list of permitted units.

See also [DATE_ADD()](/built-in-functions/date-time-functions/date_add).

## Examples

```sql
SELECT DATE_SUB('1998-01-02', INTERVAL 31 DAY);
+-----------------------------------------+
| DATE_SUB('1998-01-02', INTERVAL 31 DAY) |
+-----------------------------------------+
| 1997-12-02                              |
+-----------------------------------------+
```

```sql
SELECT DATE_SUB('2005-01-01 00:00:00', INTERVAL '1 1:1:1' DAY_SECOND);
+----------------------------------------------------------------+
| DATE_SUB('2005-01-01 00:00:00', INTERVAL '1 1:1:1' DAY_SECOND) |
+----------------------------------------------------------------+
| 2004-12-30 22:58:59                                            |
+----------------------------------------------------------------+
```