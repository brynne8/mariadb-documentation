# YEARWEEK

## Syntax

```sql
YEARWEEK(date), YEARWEEK(date,mode)
```

## Description

Returns year and week for a date. The mode argument works exactly like the mode
argument to [WEEK()](/built-in-functions/date-time-functions/week). The year in the result may be different from the
year in the date argument for the first and the last week of the year.

## Examples

```sql
SELECT YEARWEEK('1987-01-01');
+------------------------+
| YEARWEEK('1987-01-01') |
+------------------------+
|                 198652 |
+------------------------+
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
SELECT * FROM t1;
+---------------------+
| d                   |
+---------------------+
| 2007-01-30 21:31:07 |
| 1983-10-15 06:42:51 |
| 2011-04-21 12:34:56 |
| 2011-10-30 06:31:41 |
| 2011-01-30 14:03:25 |
| 2004-10-07 11:19:34 |
+---------------------+
6 rows in set (0.02 sec)
```

```sql
SELECT YEARWEEK(d) FROM t1 WHERE YEAR(d) = 2011;
+-------------+
| YEARWEEK(d) |
+-------------+
|      201116 |
|      201144 |
|      201105 |
+-------------+
3 rows in set (0.03 sec)
```