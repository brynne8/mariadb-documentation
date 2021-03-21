# MIN

## Syntax

```sql
MIN([DISTINCT] expr)
```

## Description

Returns the minimum value of <em>`expr`</em>. `MIN()` may take a string
argument, in which case it returns the minimum string value. The `DISTINCT`
keyword can be used to find the minimum of the distinct values of <em>`expr`</em>,
however, this produces the same result as omitting `DISTINCT`.

Note that [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/) and [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum/) fields are currently compared by their string value rather than their relative position in the set, so MIN() may produce a different lowest result than ORDER BY ASC.

It is an [aggregate function](/built-in-functions/aggregate-functions/), and so can be used with the [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by/) clause.

From [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), MIN() can be used as a [window function](/built-in-functions/special-functions/window-functions/).

`MIN()` returns `NULL` if there were no matching rows.

## Examples

```sql
CREATE TABLE student (name CHAR(10), test CHAR(10), score TINYINT); 

INSERT INTO student VALUES 
  ('Chun', 'SQL', 75), ('Chun', 'Tuning', 73), 
  ('Esben', 'SQL', 43), ('Esben', 'Tuning', 31), 
  ('Kaolin', 'SQL', 56), ('Kaolin', 'Tuning', 88), 
  ('Tatiana', 'SQL', 87), ('Tatiana', 'Tuning', 83);

SELECT name, MIN(score) FROM student GROUP BY name;
+---------+------------+
| name    | MIN(score) |
+---------+------------+
| Chun    |         73 |
| Esben   |         31 |
| Kaolin  |         56 |
| Tatiana |         83 |
+---------+------------+
```

MIN() with a string:

```sql
SELECT MIN(name) FROM student;
+-----------+
| MIN(name) |
+-----------+
| Chun      |
+-----------+
```

Be careful to avoid this common mistake, not grouping correctly and returning mismatched data:

```sql
SELECT name,test,MIN(score) FROM student;
+------+------+------------+
| name | test | MIN(score) |
+------+------+------------+
| Chun | SQL  |         31 |
+------+------+------------+
```

Difference between ORDER BY ASC and MIN():

```sql
CREATE TABLE student2(name CHAR(10),grade ENUM('b','c','a'));

INSERT INTO student2 VALUES('Chun','b'),('Esben','c'),('Kaolin','a');

SELECT MIN(grade) FROM student2;
+------------+
| MIN(grade) |
+------------+
| a          |
+------------+

SELECT grade FROM student2 ORDER BY grade ASC LIMIT 1;
+-------+
| grade |
+-------+
| b     |
+-------+
```

As a [window function](/built-in-functions/special-functions/window-functions/):

```sql
CREATE OR REPLACE TABLE student_test (name CHAR(10), test CHAR(10), score TINYINT);
INSERT INTO student_test VALUES 
    ('Chun', 'SQL', 75), ('Chun', 'Tuning', 73), 
    ('Esben', 'SQL', 43), ('Esben', 'Tuning', 31), 
    ('Kaolin', 'SQL', 56), ('Kaolin', 'Tuning', 88), 
    ('Tatiana', 'SQL', 87);


SELECT name, test, score, MIN(score) 
  OVER (PARTITION BY name) AS lowest_score FROM student_test;
+---------+--------+-------+--------------+
| name    | test   | score | lowest_score |
+---------+--------+-------+--------------+
| Chun    | SQL    |    75 |           73 |
| Chun    | Tuning |    73 |           73 |
| Esben   | SQL    |    43 |           31 |
| Esben   | Tuning |    31 |           31 |
| Kaolin  | SQL    |    56 |           56 |
| Kaolin  | Tuning |    88 |           56 |
| Tatiana | SQL    |    87 |           87 |
+---------+--------+-------+--------------+
```

## See Also

- [AVG](/built-in-functions/aggregate-functions/avg/) (average)
- [MAX](/built-in-functions/aggregate-functions/max/) (maximum)
- [SUM](/built-in-functions/aggregate-functions/sum/) (sum total)
- [MIN/MAX optimization](/replication/optimization-and-tuning/query-optimizations/minmax-optimization/) used by the optimizer
- [LEAST()](/sql-statements-structure/operators/comparison-operators/least/) returns the smallest value from a list.