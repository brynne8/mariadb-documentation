# DAYOFWEEK

## Syntax

```sql
DAYOFWEEK(date)
```

## Description

Returns the day of the week index for the date (1 = Sunday, 2 = Monday, ..., 7 =
Saturday). These index values correspond to the ODBC standard.

This contrasts with [WEEKDAY()](/built-in-functions/date-time-functions/weekday) which follows a different index numbering
(`0` = Monday, `1` = Tuesday, ... `6` = Sunday).

## Examples

```sql
SELECT DAYOFWEEK('2007-02-03');
+-------------------------+
| DAYOFWEEK('2007-02-03') |
+-------------------------+
|                       7 |
+-------------------------+
```

```sql
CREATE TABLE t1 (d DATETIME);
INSERT INTO t1 VALUES
    ("2007-01-30 21:31:07"),
    ("1983-10-15 06:42:51"),
    ("2011-04-21 12:34:56"),
    ("2011-10-30 06:31:41"),
    ("2011-01-30 14:03:25"),
    ("2004-10-07 11:19:34");
```

```sql
SELECT d, DAYNAME(d), DAYOFWEEK(d), WEEKDAY(d) from t1;
+---------------------+------------+--------------+------------+
| d                   | DAYNAME(d) | DAYOFWEEK(d) | WEEKDAY(d) |
+---------------------+------------+--------------+------------+
| 2007-01-30 21:31:07 | Tuesday    |            3 |          1 |
| 1983-10-15 06:42:51 | Saturday   |            7 |          5 |
| 2011-04-21 12:34:56 | Thursday   |            5 |          3 |
| 2011-10-30 06:31:41 | Sunday     |            1 |          6 |
| 2011-01-30 14:03:25 | Sunday     |            1 |          6 |
| 2004-10-07 11:19:34 | Thursday   |            5 |          3 |
+---------------------+------------+--------------+------------+
```