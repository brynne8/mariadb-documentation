# Precedence Control in Table Operations

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

Beginning in [MariaDB 10.4](/kb/en/what-is-mariadb-104/), you can control the ordering of execution on table operations using parentheses.

## Syntax

```sql
(  expression )
[ORDER BY [column[, column...]]]
[LIMIT {[offset,] row_count | row_count OFFSET offset}]
```

## Description

Using parentheses in your SQL allows you to control the order of execution for [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statements and [Table Value Constructor](/sql-statements-structure/sql-language-structure/table-value-constructors/), including [UNION](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/union/), [EXCEPT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/except/), and [INTERSECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/intersect/) operations.  MariaDB executes the parenthetical expression before the rest of the statement.  You can then use [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) and [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/) clauses the further organize the result-set.

<strong>Note</strong>: In practice, the Optimizer may rearrange the exact order in which MariaDB executes different parts of the statement.  When it calculates the result-set, however, it returns values as though the parenthetical expression were executed first.

## Example

```sql
CREATE TABLE test.t1 (num INT);

INSERT INTO test.t1 VALUES (1),(2),(3);

(SELECT * FROM test.t1 
 UNION 
 VALUES (10)) 
INTERSECT 
VALUES (1),(3),(10),(11);
+------+
| num  |
+------+
|    1 |
|    3 |
|   10 |
+------+

((SELECT * FROM test.t1 
  UNION 
  VALUES (10)) 
 INTERSECT 
 VALUES (1),(3),(10),(11)) 
ORDER BY 1 DESC;
+------+
| num  |
+------+
|   10 |
|    3 |
|    1 |
+------+
```