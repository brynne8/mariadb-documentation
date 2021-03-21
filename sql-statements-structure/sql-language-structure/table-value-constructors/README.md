# Table Value Constructors

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

Table Value Constructors were introduced in [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

## Syntax

```sql
VALUES ( row_value[, row_value...]), (...)...
```

## Description

In Unions, Views, and sub-queries, a Table Value Constructor (TVC) allows you to inject arbitrary values into the result-set.  The given values must have the same number of columns as the result-set, otherwise it returns Error 1222.

## Examples

Using TVC's with <a undefined>UNION</a> operations:

```sql
CREATE TABLE test.t1 (val1 INT, val2 INT);
INSERT INTO test.t1 VALUES(5, 8), (3, 4), (1, 2);

SELECT * FROM test.t1
UNION
VALUES (70, 90), (100, 110);

+------+------+
| val1 | val2 |
+------+------+
|    5 |    8 | 
|    3 |    4 |
|    1 |    2 |
|   70 |   90 |
|  100 |  110 |
+------+------+
```

Using TVC's with a [CREATE VIEW](/programming-customizing-mariadb/views/create-view/) statement:

```sql
CREATE VIEW v1 AS VALUES (7, 9), (9, 10);

SELECT * FROM v1;
+---+----+
| 7 |  9 |
+---+----+
| 7 |  9 |
| 9 | 10 |
+---+----+
```

Using TVC with an [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) clause:

```sql
SELECT * FROM test.t1
UNION
VALUES (10, 20), (30, 40), (50, 60), (70, 80)
ORDER BY val1 DESC;
```

Using TVC with [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/) clause:

```sql
SELECT * FROM test.t1
UNION
VALUES (10, 20), (30, 40), (50, 60), (70, 80)
LIMIT 2 OFFSET 4;

+------+------+
| val1 | val2 |
+------+------+
|   30 |   40 | 
|   50 |   60 |
+------+------+
```