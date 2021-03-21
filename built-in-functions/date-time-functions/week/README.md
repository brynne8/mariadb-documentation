# WEEK

## Syntax

```sql
WEEK(date[,mode])
```

## Description

This function returns the week number for `date`. The two-argument form of
`WEEK()` allows you to specify whether the week starts on Sunday or Monday
and whether the return value should be in the range from 0 to 53 or from 1 to
53. If the `mode` argument is omitted, the value of the [default_week_format](/kb/en/server-system-variables/#default_week_format) system variable is used.

### Modes

<table><tbody><tr><th>Mode</th><th>1st day of week</th><th>Range</th><th>Week 1 is the 1st week with</th></tr>
<tr><td>0</td><td>Sunday</td><td>0-53</td><td>a Sunday in this year</td></tr>
<tr><td>1</td><td>Monday</td><td>0-53</td><td>more than 3 days this year</td></tr>
<tr><td>2</td><td>Sunday</td><td>1-53</td><td>a Sunday in this year</td></tr>
<tr><td>3</td><td>Monday</td><td>1-53</td><td>more than 3 days this year</td></tr>
<tr><td>4</td><td>Sunday</td><td>0-53</td><td>more than 3 days this year</td></tr>
<tr><td>5</td><td>Monday</td><td>0-53</td><td>a Monday in this year</td></tr>
<tr><td>6</td><td>Sunday</td><td>1-53</td><td>more than 3 days this year</td></tr>
<tr><td>7</td><td>Monday</td><td>1-53</td><td>a Monday in this year</td></tr>
</tbody></table>

With the mode value of 3, which means “more than 3 days this year”,  weeks are numbered according to ISO 8601:1988.

## Examples

```sql
SELECT WEEK('2008-02-20');
+--------------------+
| WEEK('2008-02-20') |
+--------------------+
|                  7 |
+--------------------+

SELECT WEEK('2008-02-20',0);
+----------------------+
| WEEK('2008-02-20',0) |
+----------------------+
|                    7 |
+----------------------+

SELECT WEEK('2008-02-20',1);
+----------------------+
| WEEK('2008-02-20',1) |
+----------------------+
|                    8 |
+----------------------+

SELECT WEEK('2008-12-31',0);
+----------------------+
| WEEK('2008-12-31',0) |
+----------------------+
|                   52 |
+----------------------+

SELECT WEEK('2008-12-31',1);
+----------------------+
| WEEK('2008-12-31',1) |
+----------------------+
|                   53 |
+----------------------+

 SELECT WEEK('2019-12-30',3);
+----------------------+
| WEEK('2019-12-30',3) |
+----------------------+
|                    1 |
+----------------------+
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
SELECT d, WEEK(d,0), WEEK(d,1) from t1;
+---------------------+-----------+-----------+
| d                   | WEEK(d,0) | WEEK(d,1) |
+---------------------+-----------+-----------+
| 2007-01-30 21:31:07 |         4 |         5 |
| 1983-10-15 06:42:51 |        41 |        41 |
| 2011-04-21 12:34:56 |        16 |        16 |
| 2011-10-30 06:31:41 |        44 |        43 |
| 2011-01-30 14:03:25 |         5 |         4 |
| 2004-10-07 11:19:34 |        40 |        41 |
+---------------------+-----------+-----------+
```