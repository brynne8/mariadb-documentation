# STDDEV_POP

## Syntax

```sql
STDDEV_POP(expr)
```

## Description

Returns the population standard deviation of <em>`expr`</em> (the square root of
[VAR_POP()](/built-in-functions/aggregate-functions/var_pop)). You can also use [STD()](/built-in-functions/aggregate-functions/std) or
[STDDEV()](/built-in-functions/aggregate-functions/stddev), which are equivalent but not standard SQL.

It is an [aggregate function](/built-in-functions/aggregate-functions), and so can be used with the [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by) clause.

From [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), STDDEV_POP() can be used as a [window function](/built-in-functions/special-functions/window-functions).

STDDEV_POP() returns `NULL` if there were no matching rows.

## Examples

As an [aggregate function](/built-in-functions/aggregate-functions):

```sql
CREATE OR REPLACE TABLE stats (category VARCHAR(2), x INT);

INSERT INTO stats VALUES 
  ('a',1),('a',2),('a',3),
  ('b',11),('b',12),('b',20),('b',30),('b',60);

SELECT category, STDDEV_POP(x), STDDEV_SAMP(x), VAR_POP(x) 
  FROM stats GROUP BY category;
+----------+---------------+----------------+------------+
| category | STDDEV_POP(x) | STDDEV_SAMP(x) | VAR_POP(x) |
+----------+---------------+----------------+------------+
| a        |        0.8165 |         1.0000 |     0.6667 |
| b        |       18.0400 |        20.1693 |   325.4400 |
+----------+---------------+----------------+------------+
```

As a [window function](/built-in-functions/special-functions/window-functions):

```sql
CREATE OR REPLACE TABLE student_test (name CHAR(10), test CHAR(10), score TINYINT);

INSERT INTO student_test VALUES 
    ('Chun', 'SQL', 75), ('Chun', 'Tuning', 73), 
    ('Esben', 'SQL', 43), ('Esben', 'Tuning', 31), 
    ('Kaolin', 'SQL', 56), ('Kaolin', 'Tuning', 88), 
    ('Tatiana', 'SQL', 87);

SELECT name, test, score, STDDEV_POP(score) 
  OVER (PARTITION BY test) AS stddev_results FROM student_test;
+---------+--------+-------+----------------+
| name    | test   | score | stddev_results |
+---------+--------+-------+----------------+
| Chun    | SQL    |    75 |        16.9466 |
| Chun    | Tuning |    73 |        24.1247 |
| Esben   | SQL    |    43 |        16.9466 |
| Esben   | Tuning |    31 |        24.1247 |
| Kaolin  | SQL    |    56 |        16.9466 |
| Kaolin  | Tuning |    88 |        24.1247 |
| Tatiana | SQL    |    87 |        16.9466 |
+---------+--------+-------+----------------+
```

## See Also

- [STD](/built-in-functions/aggregate-functions/std) (equivalent, non-standard SQL)
- [STDDEV](/built-in-functions/aggregate-functions/stddev) (equivalent, Oracle-compatible non-standard SQL)
- [VAR_POP](/built-in-functions/aggregate-functions/var_pop) (variance)
- [STDDEV_SAMP](/built-in-functions/aggregate-functions/stddev_samp) (sample standard deviation)