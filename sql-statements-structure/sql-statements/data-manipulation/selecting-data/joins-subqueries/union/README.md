# UNION

`UNION` is used to combine the results from multiple [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statements into a single result set.

## Syntax

```sql
SELECT ...
UNION [ALL | DISTINCT] SELECT ...
[UNION [ALL | DISTINCT] SELECT ...]
[ORDER BY [column [, column ...]]]
[LIMIT {[offset,] row_count | row_count OFFSET offset}]
```

## Description

`UNION` is used to combine the results from multiple [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statements into a single result set.

The column names from the first <code class="fixed" style="white-space:pre-wrap">SELECT</code> statement are used as the column names for the results returned. Selected columns listed in corresponding positions of each SELECT statement should have the same data type. (For example, the first column selected by the first statement should have the same type as the first column selected by the other statements.)

If they don't, the type and length of the columns in the result take into account the values returned by all of the SELECTs, so there is no need for explicit casting. Note that currently this is not the case for [recursive CTEs](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/recursive-common-table-expressions-overview/) - see [MDEV-12325](https://jira.mariadb.org/browse/MDEV-12325).

Table names can be specified as `db_name`.`tbl_name`. This permits writing `UNION`s which involve multiple databases. See [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers/) for syntax details.

UNION queries cannot be used with [aggregate functions](/kb/en/functions-and-modifiers-for-use-with-group-by/).

`EXCEPT` and `UNION` have the same operation precedence and `INTERSECT` has a higher precedence, unless [running in Oracle mode](/kb/en/sql_modeoracle/), in which case all three have the same precedence.

### ALL/DISTINCT

The `ALL` keyword causes duplicate rows to be preserved. The `DISTINCT` keyword (the default if the keyword is omitted) causes duplicate rows to be removed by the results.

UNION ALL and UNION DISTINCT can both be present in a query. In this case, UNION DISTINCT will override any UNION ALLs to its left.

##### MariaDB starting with [10.1.1](/kb/en/mariadb-1011-release-notes/)

Until [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), all `UNION ALL` statements required the server to create a temporary table. Since [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), the server can in most cases execute `UNION ALL` without creating a temporary table, improving performance (see [MDEV-334](https://jira.mariadb.org/browse/MDEV-334)).

### ORDER BY and LIMIT

Individual SELECTs can contain their own [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) and [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/) clauses. In this case, the individual queries need to be wrapped between parentheses. However, this does not affect the order of the UNION, so they only are useful to limit the record read by one SELECT.

The UNION can have global [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) and [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/) clauses, which affect the whole resultset. If the columns retrieved by individual SELECT statements have an alias (AS), the ORDER BY must use that alias, not the real column names.

### HIGH_PRIORITY

Specifying a query as [HIGH_PRIORITY](/kb/en/select/#high-priority) will not work inside a UNION. If applied to the first SELECT, it will be ignored. Applying to a later SELECT results in a syntax error:

```sql
ERROR 1234 (42000): Incorrect usage/placement of 'HIGH_PRIORITY'
```

### SELECT ... INTO ...

Individual SELECTs cannot be written [INTO DUMPFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-dumpfile/) or [INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile/). If the last SELECT statement specifies INTO DUMPFILE or INTO OUTFILE, the entire result of the UNION will be written. Placing the clause after any other SELECT will result in a syntax error.

If the result is a single row, [SELECT ... INTO @var_name](/kb/en/select-into-variable/) can also be used.
<br>
<br>

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

### Parentheses

From [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/), parentheses can be used to specify precedence. Before this, a syntax error would be returned.

## Examples

`UNION` between tables having different column names:

```sql
(SELECT e_name AS name, email FROM employees)
UNION
(SELECT c_name AS name, email FROM customers);
```

Specifying the `UNION`'s global order and limiting total rows:

```sql
(SELECT name, email FROM employees)
UNION
(SELECT name, email FROM customers)
ORDER BY name LIMIT 10;
```

Adding a constant row:

```sql
(SELECT 'John Doe' AS name, 'john.doe@example.net' AS email)
UNION
(SELECT name, email FROM customers);
```

Differing types:

```sql
SELECT CAST('x' AS CHAR(1)) UNION SELECT REPEAT('y',4);
+----------------------+
| CAST('x' AS CHAR(1)) |
+----------------------+
| x                    |
| yyyy                 |
+----------------------+
```

Returning the results in order of each individual SELECT by use of a sort column:

```sql
(SELECT 1 AS sort_column, e_name AS name, email FROM employees)
UNION
(SELECT 2, c_name AS name, email FROM customers) ORDER BY sort_column;
```

Difference between UNION, [EXCEPT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/except/) and [INTERSECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/intersect/). `INTERSECT ALL` and `EXCEPT ALL` are available from [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/).

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

- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
- [EXCEPT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/except/)
- [INTERSECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/intersect/)
- [Recursive Common Table Expressions Overview](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/recursive-common-table-expressions-overview/)
- [Get Set for Set Theory: UNION, INTERSECT and EXCEPT in SQL](https://www.youtube.com/watch?v=UNi-fVSpRm0) (video tutorial)