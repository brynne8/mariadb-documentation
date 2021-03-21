# COUNT

## Syntax

```sql
COUNT(expr)
```

## Description

Returns a count of the number of non-NULL values of expr in the rows retrieved by a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) statement. The result is a [BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint) value. It is an [aggregate function](/built-in-functions/aggregate-functions), and so can be used with the [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by) clause.

COUNT(*) counts the total number of rows in a table.

COUNT() returns 0 if there were no matching rows.

From [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/), COUNT() can be used as a [window function](/built-in-functions/special-functions/window-functions).

## Examples

```sql
CREATE TABLE student (name CHAR(10), test CHAR(10), score TINYINT); 

INSERT INTO student VALUES 
  ('Chun', 'SQL', 75), ('Chun', 'Tuning', 73), 
  ('Esben', 'SQL', 43), ('Esben', 'Tuning', 31), 
  ('Kaolin', 'SQL', 56), ('Kaolin', 'Tuning', 88), 
  ('Tatiana', 'SQL', 87), ('Tatiana', 'Tuning', 83);

SELECT COUNT(*) FROM student;
+----------+
| COUNT(*) |
+----------+
|        8 |
+----------+
```

[COUNT(DISTINCT)](/built-in-functions/aggregate-functions/count-distinct) example:

```sql
SELECT COUNT(DISTINCT (name)) FROM student;
+------------------------+
| COUNT(DISTINCT (name)) |
+------------------------+
|                      4 |
+------------------------+
```

As a [window function](/built-in-functions/special-functions/window-functions)

```sql
CREATE OR REPLACE TABLE student_test (name CHAR(10), test CHAR(10), score TINYINT);

INSERT INTO student_test VALUES 
    ('Chun', 'SQL', 75), ('Chun', 'Tuning', 73), 
    ('Esben', 'SQL', 43), ('Esben', 'Tuning', 31), 
    ('Kaolin', 'SQL', 56), ('Kaolin', 'Tuning', 88), 
    ('Tatiana', 'SQL', 87);

SELECT name, test, score, COUNT(score) OVER (PARTITION BY name) 
    AS tests_written FROM student_test;
+---------+--------+-------+---------------+
| name    | test   | score | tests_written |
+---------+--------+-------+---------------+
| Chun    | SQL    |    75 |             2 |
| Chun    | Tuning |    73 |             2 |
| Esben   | SQL    |    43 |             2 |
| Esben   | Tuning |    31 |             2 |
| Kaolin  | SQL    |    56 |             2 |
| Kaolin  | Tuning |    88 |             2 |
| Tatiana | SQL    |    87 |             1 |
+---------+--------+-------+---------------+
```

## See Also

- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select)
- [COUNT DISTINCT](/built-in-functions/aggregate-functions/count-distinct)
- [Window Functions](/built-in-functions/special-functions/window-functions)