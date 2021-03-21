# MEDIAN

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

The MEDIAN() [window function](/built-in-functions/special-functions/window-functions) was first introduced with in [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/).

## Syntax

```sql
MEDIAN(median expression) OVER (
  [ PARTITION BY partition_expression ] 
)
```

## Description

MEDIAN() is a [window function](/built-in-functions/special-functions/window-functions) that returns the median value of a range of values.

It is a specific case of [PERCENTILE_CONT](/built-in-functions/special-functions/window-functions/percentile_cont), with an argument of 0.5 and the [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by) column the one in `MEDIAN`'s argument. <br>

```sql
MEDIAN(<median-arg>) OVER ( [ PARTITION BY partition_expression] )
```

Is equivalent to:<br>

```sql
PERCENTILE_CONT(0.5) WITHIN 
  GROUP (ORDER BY <median-arg>) OVER ( [ PARTITION BY partition_expression ])
```

## Examples

```sql
CREATE TABLE book_rating (name CHAR(30), star_rating TINYINT);

INSERT INTO book_rating VALUES ('Lord of the Ladybirds', 5);
INSERT INTO book_rating VALUES ('Lord of the Ladybirds', 3);
INSERT INTO book_rating VALUES ('Lady of the Flies', 1);
INSERT INTO book_rating VALUES ('Lady of the Flies', 2);
INSERT INTO book_rating VALUES ('Lady of the Flies', 5);

SELECT name, median(star_rating) OVER (PARTITION BY name) FROM book_rating;
+-----------------------+----------------------------------------------+
| name                  | median(star_rating) OVER (PARTITION BY name) |
+-----------------------+----------------------------------------------+
| Lord of the Ladybirds |                                 4.0000000000 |
| Lord of the Ladybirds |                                 4.0000000000 |
| Lady of the Flies     |                                 2.0000000000 |
| Lady of the Flies     |                                 2.0000000000 |
| Lady of the Flies     |                                 2.0000000000 |
+-----------------------+----------------------------------------------+
```

## See Also

- [PERCENTILE_CONT](/built-in-functions/special-functions/window-functions/percentile_cont)