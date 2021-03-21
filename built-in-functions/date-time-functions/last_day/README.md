# LAST_DAY

## Syntax

```sql
LAST_DAY(date)
```

## Description

Takes a date or datetime value and returns the corresponding value for
the last day of the month. Returns NULL if the argument is invalid.

## Examples

```sql
SELECT LAST_DAY('2003-02-05');
+------------------------+
| LAST_DAY('2003-02-05') |
+------------------------+
| 2003-02-28             |
+------------------------+

SELECT LAST_DAY('2004-02-05');
+------------------------+
| LAST_DAY('2004-02-05') |
+------------------------+
| 2004-02-29             |
+------------------------+

SELECT LAST_DAY('2004-01-01 01:01:01');
+---------------------------------+
| LAST_DAY('2004-01-01 01:01:01') |
+---------------------------------+
| 2004-01-31                      |
+---------------------------------+

SELECT LAST_DAY('2003-03-32');
+------------------------+
| LAST_DAY('2003-03-32') |
+------------------------+
| NULL                   |
+------------------------+
1 row in set, 1 warning (0.00 sec)

Warning (Code 1292): Incorrect datetime value: '2003-03-32'
```