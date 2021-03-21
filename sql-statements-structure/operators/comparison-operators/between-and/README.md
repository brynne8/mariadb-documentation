# BETWEEN AND

## Syntax

```sql
expr BETWEEN min AND max
```

## Description

If expr is greater than or equal to min and expr is less than or equal
to max, BETWEEN returns 1, otherwise it returns 0. This is equivalent
to the expression (min &lt;= expr AND expr &lt;= max) if all the arguments
are of the same type. Otherwise type conversion takes place according
to the rules described at [Type Conversion](/built-in-functions/string-functions/type-conversion/), but
applied to all the three arguments.

## Examples

```sql
SELECT 1 BETWEEN 2 AND 3;
+-------------------+
| 1 BETWEEN 2 AND 3 |
+-------------------+
|                 0 |
+-------------------+
```

```sql
SELECT 'b' BETWEEN 'a' AND 'c';
+-------------------------+
| 'b' BETWEEN 'a' AND 'c' |
+-------------------------+
|                       1 |
+-------------------------+
```

```sql
SELECT 2 BETWEEN 2 AND '3';
+---------------------+
| 2 BETWEEN 2 AND '3' |
+---------------------+
|                   1 |
+---------------------+
```

```sql
SELECT 2 BETWEEN 2 AND 'x-3';
+-----------------------+
| 2 BETWEEN 2 AND 'x-3' |
+-----------------------+
|                     0 |
+-----------------------+
1 row in set, 1 warning (0.00 sec)

Warning (Code 1292): Truncated incorrect DOUBLE value: 'x-3'
```

NULL:

```sql
SELECT 1 BETWEEN 1 AND NULL;
+----------------------+
| 1 BETWEEN 1 AND NULL |
+----------------------+
|                 NULL |
+----------------------+
```

DATE, DATETIME and TIMESTAMP examples. Omitting the time component compares against `00:00`, so later times on the same date are not returned:

```sql
CREATE TABLE `x` (
  a date ,
  b datetime,
  c timestamp
)

INSERT INTO x VALUES 
 ('2018-11-11', '2018-11-11 05:15', '2018-11-11 05:15'), 
 ('2018-11-12', '2018-11-12 05:15', '2018-11-12 05:15'); 

SELECT * FROM x WHERE a BETWEEN '2018-11-11' AND '2018-11-12';
+------------+---------------------+---------------------+
| a          | b                   | c                   |
+------------+---------------------+---------------------+
| 2018-11-11 | 2018-11-11 05:15:00 | 2018-11-11 05:15:00 |
| 2018-11-12 | 2018-11-12 05:15:00 | 2018-11-12 05:15:00 |
+------------+---------------------+---------------------+

SELECT * FROM x WHERE b BETWEEN '2018-11-11' AND '2018-11-12';
+------------+---------------------+---------------------+
| a          | b                   | c                   |
+------------+---------------------+---------------------+
| 2018-11-11 | 2018-11-11 05:15:00 | 2018-11-11 05:15:00 |
+------------+---------------------+---------------------+

SELECT * FROM x WHERE c BETWEEN '2018-11-11' AND '2018-11-12';
+------------+---------------------+---------------------+
| a          | b                   | c                   |
+------------+---------------------+---------------------+
| 2018-11-11 | 2018-11-11 05:15:00 | 2018-11-11 05:15:00 |
+------------+---------------------+---------------------+
```