# RANK

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

The RANK() function was first introduced with [window functions](/built-in-functions/special-functions/window-functions/) in [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/).

## Syntax

```sql
RANK() OVER (
  [ PARTITION BY partition_expression ]
  [ ORDER BY order_list ]
) 
```

## Description

RANK() is a [window function](/built-in-functions/special-functions/window-functions/) that displays the number of a given row, starting at one and following the [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) sequence of the window function, with identical values receiving the same result. It is similar to the [ROW_NUMBER()](/built-in-functions/special-functions/window-functions/row_number/) function except that in that function, identical values will receive a different row number for each result.

## Examples

The distinction between [DENSE_RANK()](/built-in-functions/special-functions/window-functions/dense_rank/), RANK() and [ROW_NUMBER()](/built-in-functions/special-functions/window-functions/row_number/):

```sql
CREATE TABLE student(course VARCHAR(10), mark int, name varchar(10));

INSERT INTO student VALUES 
  ('Maths', 60, 'Thulile'),
  ('Maths', 60, 'Pritha'),
  ('Maths', 70, 'Voitto'),
  ('Maths', 55, 'Chun'),
  ('Biology', 60, 'Bilal'),
   ('Biology', 70, 'Roger');

SELECT 
  RANK() OVER (PARTITION BY course ORDER BY mark DESC) AS rank, 
  DENSE_RANK() OVER (PARTITION BY course ORDER BY mark DESC) AS dense_rank, 
  ROW_NUMBER() OVER (PARTITION BY course ORDER BY mark DESC) AS row_num, 
  course, mark, name 
FROM student ORDER BY course, mark DESC;
+------+------------+---------+---------+------+---------+
| rank | dense_rank | row_num | course  | mark | name    |
+------+------------+---------+---------+------+---------+
|    1 |          1 |       1 | Biology |   70 | Roger   |
|    2 |          2 |       2 | Biology |   60 | Bilal   |
|    1 |          1 |       1 | Maths   |   70 | Voitto  |
|    2 |          2 |       2 | Maths   |   60 | Thulile |
|    2 |          2 |       3 | Maths   |   60 | Pritha  |
|    4 |          3 |       4 | Maths   |   55 | Chun    |
+------+------------+---------+---------+------+---------+
```

## See Also

- [DENSE_RANK()](/built-in-functions/special-functions/window-functions/dense_rank/)
- [ROW_NUMBER()](/built-in-functions/special-functions/window-functions/row_number/)
- [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/)