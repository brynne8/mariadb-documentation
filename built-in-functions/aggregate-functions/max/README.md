# MAX

## Syntax

```sql
MAX([DISTINCT] expr)
```

## Description

Returns the largest, or maximum, value of <em>`expr`</em>. `MAX()` can also take a string
argument in which case it returns the maximum string value. The `DISTINCT`
keyword can be used to find the maximum of the distinct values of <em>`expr`</em>,
however, this produces the same result as omitting `DISTINCT`.

Note that [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set) and [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum) fields are currently compared by their string value rather than their relative position in the set, so MAX() may produce a different highest result than ORDER BY DESC.

It is an [aggregate function](/built-in-functions/aggregate-functions), and so can be used with the [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by) clause.

From [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), MAX() can be used as a [window function](/built-in-functions/special-functions/window-functions).

`MAX()` returns `NULL` if there were no matching rows.

## Examples

```sql
CREATE TABLE student (name CHAR(10), test CHAR(10), score TINYINT); 

INSERT INTO student VALUES 
  ('Chun', 'SQL', 75), ('Chun', 'Tuning', 73), 
  ('Esben', 'SQL', 43), ('Esben', 'Tuning', 31), 
  ('Kaolin', 'SQL', 56), ('Kaolin', 'Tuning', 88), 
  ('Tatiana', 'SQL', 87), ('Tatiana', 'Tuning', 83);

SELECT name, MAX(score) FROM student GROUP BY name;
+---------+------------+
| name    | MAX(score) |
+---------+------------+
| Chun    |         75 |
| Esben   |         43 |
| Kaolin  |         88 |
| Tatiana |         87 |
+---------+------------+
```

MAX string:

```sql
SELECT MAX(name) FROM student;
+-----------+
| MAX(name) |
+-----------+
| Tatiana   |
+-----------+
```

Be careful to avoid this common mistake, not grouping correctly and returning mismatched data:

```sql
SELECT name,test,MAX(SCORE) FROM student;
+------+------+------------+
| name | test | MAX(SCORE) |
+------+------+------------+
| Chun | SQL  |         88 |
+------+------+------------+
```

Difference between ORDER BY DESC and MAX():

```sql
CREATE TABLE student2(name CHAR(10),grade ENUM('b','c','a'));

INSERT INTO student2 VALUES('Chun','b'),('Esben','c'),('Kaolin','a');

SELECT MAX(grade) FROM student2;
+------------+
| MAX(grade) |
+------------+
| c          |
+------------+

SELECT grade FROM student2 ORDER BY grade DESC LIMIT 1;
+-------+
| grade |
+-------+
| a     |
+-------+
```

As a [window function](/built-in-functions/special-functions/window-functions):

```sql
CREATE OR REPLACE TABLE student_test (name CHAR(10), test CHAR(10), score TINYINT);
INSERT INTO student_test VALUES 
    ('Chun', 'SQL', 75), ('Chun', 'Tuning', 73), 
    ('Esben', 'SQL', 43), ('Esben', 'Tuning', 31), 
    ('Kaolin', 'SQL', 56), ('Kaolin', 'Tuning', 88), 
    ('Tatiana', 'SQL', 87);

SELECT name, test, score, MAX(score) 
  OVER (PARTITION BY name) AS highest_score FROM student_test;
+---------+--------+-------+---------------+
| name    | test   | score | highest_score |
+---------+--------+-------+---------------+
| Chun    | SQL    |    75 |            75 |
| Chun    | Tuning |    73 |            75 |
| Esben   | SQL    |    43 |            43 |
| Esben   | Tuning |    31 |            43 |
| Kaolin  | SQL    |    56 |            88 |
| Kaolin  | Tuning |    88 |            88 |
| Tatiana | SQL    |    87 |            87 |
+---------+--------+-------+---------------+
```

## See Also

- [AVG](/built-in-functions/aggregate-functions/avg) (average)
- [MIN](/built-in-functions/aggregate-functions/min) (minimum)
- [SUM](/built-in-functions/aggregate-functions/sum) (sum total)
- [MIN/MAX optimization](/replication/optimization-and-tuning/query-optimizations/minmax-optimization) used by the optimizer
- [GREATEST()](/sql-statements-structure/operators/comparison-operators/greatest) returns the largest value from a list