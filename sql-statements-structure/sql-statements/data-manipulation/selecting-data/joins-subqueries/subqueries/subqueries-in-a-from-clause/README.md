# Subqueries in a FROM Clause

Although [subqueries](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/subqueries/) are more commonly placed in a WHERE clause, they can also form part of the FROM clause. Such subqueries are commonly called derived tables.

If a subquery is used in this way, you must also use an AS clause to name the result of the subquery.

## ORACLE mode

##### MariaDB starting with [10.6.0](/kb/en/mariadb-1060-release-notes/)

From [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/), [anonymous subqueries in a FROM clause](/kb/en/sql_modeoracle/#simple-syntax-compatibility) (no AS clause) are permitted in [ORACLE mode](/kb/en/sql_modeoracle-from-mariadb-103/).

## Examples

```sql
CREATE TABLE student (name CHAR(10), test CHAR(10), score TINYINT); 

INSERT INTO student VALUES 
  ('Chun', 'SQL', 75), ('Chun', 'Tuning', 73), 
  ('Esben', 'SQL', 43), ('Esben', 'Tuning', 31), 
  ('Kaolin', 'SQL', 56), ('Kaolin', 'Tuning', 88), 
  ('Tatiana', 'SQL', 87), ('Tatiana', 'Tuning', 83);
```

Assume that, given the data above, you want to return the average total for all students. In other words, the average of Chun's 148 (75+73), Esben's 74 (43+31), etc.

You cannot do the following:

```sql
SELECT AVG(SUM(score)) FROM student GROUP BY name;
ERROR 1111 (HY000): Invalid use of group function
```

A subquery in the FROM clause is however permitted:

```sql
SELECT AVG(sq_sum) FROM (SELECT SUM(score) AS sq_sum FROM student GROUP BY name) AS t;
+-------------+
| AVG(sq_sum) |
+-------------+
|    134.0000 |
+-------------+
```

From [MariaDB 10.6](/kb/en/what-is-mariadb-106/) in [ORACLE mode](/kb/en/sql_modeoracle-from-mariadb-103/), the following is permitted:

```sql
SELECT * FROM (SELECT 1 FROM DUAL), (SELECT 2 FROM DUAL);
```