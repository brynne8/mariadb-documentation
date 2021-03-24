# INTERSECT

##### MariaDB starting with [10.3.0](/kb/en/mariadb-1030-release-notes/)

INTERSECT was introduced in [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/).

The result of an intersect is the intersection of right and left `SELECT` results, i.e. only records that are present in both result sets will be included in the result of the operation.

# Syntax

```sql
SELECT ...
(INTERSECT [ALL | DISTINCT] | EXCEPT [ALL | DISTINCT] | UNION [ALL | DISTINCT]) SELECT ...
[(INTERSECT [ALL | DISTINCT] | EXCEPT [ALL | DISTINCT] | UNION [ALL | DISTINCT]) SELECT ...]
[ORDER BY [column [, column ...]]]
[LIMIT {[offset,] row_count | row_count OFFSET offset}]
```

## Description

MariaDB has supported `INTERSECT` (as well as [EXCEPT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/except/)) in addition to [UNION](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/union/) since [MariaDB 10.3](/kb/en/what-is-mariadb-103/).

All behavior for naming columns, <code class="fixed" style="white-space:pre-wrap">ORDER BY</code> and <code class="fixed" style="white-space:pre-wrap">LIMIT</code> is the same as for [UNION](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/union/).

`INTERSECT` implicitly supposes a `DISTINCT` operation.

The result of an intersect is the intersection of right and left `SELECT` results, i.e. only records that are present in both result sets will be included in the result of the operation.

`INTERSECT` has higher precedence than `UNION` and `EXCEPT` (unless running [running in Oracle mode](/kb/en/sql_modeoracle/), in which case all three have the same precedence). If possible it will be executed linearly but if not it will be translated to a subquery in the `FROM` clause:

```sql
(select a,b from t1)
union
(select c,d from t2)
intersect
(select e,f from t3)
union
(select 4,4);
```

will be translated to:

```sql
(select a,b from t1)
union
select c,d from
  ((select c,d from t2)
   intersect
   (select e,f from t3)) dummy_subselect
union
(select 4,4)
```

<br>
<br>

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

### Parentheses

From [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/), parentheses can be used to specify precedence. Before this, a syntax error would be returned.

##### MariaDB starting with [10.5.0](/kb/en/mariadb-1050-release-notes/)

### ALL/DISTINCT

`INTERSECT ALL` and `INTERSECT DISTINCT` were introduced in [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/). The `ALL` operator leaves duplicates intact, while the `DISTINCT` operator removes duplicates. `DISTINCT` is the default behavior if neither operator is supplied, and the only behavior prior to [MariaDB 10.5](/kb/en/what-is-mariadb-105/).

## Examples

Show customers which are employees:

```sql
(SELECT e_name AS name, email FROM employees)
INTERSECT
(SELECT c_name AS name, email FROM customers);
```

Difference between [UNION](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/union/), [EXCEPT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/except/) and INTERSECT. `INTERSECT ALL` and `EXCEPT ALL` are available from [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/).

```sql
CREATE TABLE seqs (i INT);
INSERT INTO seqs VALUES (1),(2),(2),(3),(3),(4),(5),(6);

SELECT i FROM seqs WHERE i <= 3 UNION SELECT i FROM seqs WHERE i>=3;
+------+
| i    |
+------+
|    1 |
|    2 |
|    3 |
|    4 |
|    5 |
|    6 |
+------+

SELECT i FROM seqs WHERE i <= 3 UNION ALL SELECT i FROM seqs WHERE i>=3;
+------+
| i    |
+------+
|    1 |
|    2 |
|    2 |
|    3 |
|    3 |
|    3 |
|    3 |
|    4 |
|    5 |
|    6 |
+------+

SELECT i FROM seqs WHERE i <= 3 EXCEPT SELECT i FROM seqs WHERE i>=3;
+------+
| i    |
+------+
|    1 |
|    2 |
+------+

SELECT i FROM seqs WHERE i <= 3 EXCEPT ALL SELECT i FROM seqs WHERE i>=3;
+------+
| i    |
+------+
|    1 |
|    2 |
|    2 |
+------+

SELECT i FROM seqs WHERE i <= 3 INTERSECT SELECT i FROM seqs WHERE i>=3;
+------+
| i    |
+------+
|    3 |
+------+

SELECT i FROM seqs WHERE i <= 3 INTERSECT ALL SELECT i FROM seqs WHERE i>=3;
+------+
| i    |
+------+
|    3 |
|    3 |
+------+
```

Parentheses for specifying precedence, from [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/)

```sql
CREATE OR REPLACE TABLE t1 (a INT);
CREATE OR REPLACE TABLE t2 (b INT);
CREATE OR REPLACE TABLE t3 (c INT);

INSERT INTO t1 VALUES (1),(2),(3),(4);
INSERT INTO t2 VALUES (5),(6);
INSERT INTO t3 VALUES (1),(6);

((SELECT a FROM t1) UNION (SELECT b FROM t2)) INTERSECT (SELECT c FROM t3);
+------+
| a    |
+------+
|    1 |
|    6 |
+------+

(SELECT a FROM t1) UNION ((SELECT b FROM t2) INTERSECT (SELECT c FROM t3));
+------+
| a    |
+------+
|    1 |
|    2 |
|    3 |
|    4 |
|    6 |
+------+
```

## See Also

- [UNION](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/union/)
- [EXCEPT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/except/)
- [Get Set for Set Theory: UNION, INTERSECT and EXCEPT in SQL](https://www.youtube.com/watch?v=UNi-fVSpRm0) (video tutorial)