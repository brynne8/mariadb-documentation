# WEEKDAY

## Syntax

```sql
WEEKDAY(date)
```

## Description

Returns the weekday index for `date` 
(`0` = Monday, `1` = Tuesday, ... `6` = Sunday).

This contrasts with [DAYOFWEEK()](/built-in-functions/date-time-functions/dayofweek) which follows the ODBC standard
(`1` = Sunday, `2` = Monday, ..., `7` = Saturday).

## Examples

```sql
SELECT WEEKDAY('2008-02-03 22:23:00');
+--------------------------------+
| WEEKDAY('2008-02-03 22:23:00') |
+--------------------------------+
|                              6 |
+--------------------------------+

SELECT WEEKDAY('2007-11-06');
+-----------------------+
| WEEKDAY('2007-11-06') |
+-----------------------+
|                     1 |
+-----------------------+
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
SELECT d FROM t1 where WEEKDAY(d) = 6;
+---------------------+
| d                   |
+---------------------+
| 2011-10-30 06:31:41 |
| 2011-01-30 14:03:25 |
+---------------------+
```