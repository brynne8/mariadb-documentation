# ORDER BY

## Description

Use the `ORDER BY` clause to order a resultset, such as that are returned from a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
statement. You can specify just a column or use any expression with functions. If you are
using the `GROUP BY` clause, you can use grouping functions in `ORDER BY`.
Ordering is done after grouping.

You can use multiple ordering expressions, separated by commas. Rows will be sorted by
the first expression, then by the second expression if they have the same value for the
first, and so on.

You can use the keywords `ASC` and `DESC` after each ordering expression to
force that ordering to be ascending or descending, respectively. Ordering is ascending
by default.

You can also use a single integer as the ordering expression. If you use an integer <em>n</em>,
the results will be ordered by the <em>n</em>th column in the select expression.

When string values are compared, they are compared as if by the [STRCMP](/built-in-functions/string-functions/strcmp/)
function. `STRCMP` ignores trailing whitespace and may normalize
characters and ignore case, depending on the [collation](/kb/en/data-types-character-sets-and-collations/) in use.

Duplicated entries in the `ORDER BY` clause are removed.

`ORDER BY` can also be used to order the activities of a [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) or [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) statement (usually with the [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/) clause).

##### MariaDB starting with [10.3.2](/kb/en/mariadb-1032-release-notes/)

Until [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), it was not possible to use `ORDER BY` (or [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/)) in a multi-table [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) statement. This restriction was lifted in [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/).

##### MariaDB starting with [10.5](/kb/en/what-is-mariadb-105/)

From [MariaDB 10.5](/kb/en/what-is-mariadb-105/), MariaDB allows packed sort keys and values of non-sorted fields in the sort buffer. This can make filesort temporary files much smaller when VARCHAR, CHAR or BLOBs are used, notably speeding up some ORDER BY sorts.

## Examples

```sql
CREATE TABLE seq (i INT, x VARCHAR(1));
INSERT INTO seq VALUES (1,'a'), (2,'b'), (3,'b'), (4,'f'), (5,'e');

SELECT * FROM seq ORDER BY i;
+------+------+
| i    | x    |
+------+------+
|    1 | a    |
|    2 | b    |
|    3 | b    |
|    4 | f    |
|    5 | e    |
+------+------+

SELECT * FROM seq ORDER BY i DESC;
+------+------+
| i    | x    |
+------+------+
|    5 | e    |
|    4 | f    |
|    3 | b    |
|    2 | b    |
|    1 | a    |
+------+------+

SELECT * FROM seq ORDER BY x,i;
+------+------+
| i    | x    |
+------+------+
|    1 | a    |
|    2 | b    |
|    3 | b    |
|    5 | e    |
|    4 | f    |
+------+------+
```

ORDER BY in an [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) statement, in conjunction with [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/):

```sql
UPDATE seq SET x='z' WHERE x='b' ORDER BY i DESC LIMIT 1;

SELECT * FROM seq;
+------+------+
| i    | x    |
+------+------+
|    1 | a    |
|    2 | b    |
|    3 | z    |
|    4 | f    |
|    5 | e    |
+------+------+
```

From [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/), `ORDER BY` can be used in a multi-table update:

```sql
CREATE TABLE warehouse (product_id INT, qty INT);
INSERT INTO warehouse VALUES (1,100),(2,100),(3,100),(4,100);

CREATE TABLE store (product_id INT, qty INT);
INSERT INTO store VALUES (1,5),(2,5),(3,5),(4,5);

UPDATE warehouse,store SET warehouse.qty = warehouse.qty-2, store.qty = store.qty+2 
  WHERE (warehouse.product_id = store.product_id AND store.product_id  >= 1) 
    ORDER BY store.product_id DESC LIMIT 2;

SELECT * FROM warehouse;
+------------+------+
| product_id | qty  |
+------------+------+
|          1 |  100 |
|          2 |  100 |
|          3 |   98 |
|          4 |   98 |
+------------+------+

SELECT * FROM store;
+------------+------+
| product_id | qty  |
+------------+------+
|          1 |    5 |
|          2 |    5 |
|          3 |    7 |
|          4 |    7 |
+------------+------+
```

## See Also

- [Why is ORDER BY in a FROM subquery ignored?](/kb/en/why-is-order-by-in-a-from-subquery-ignored/)
- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
- [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/)
- [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/)
- [Improvements to ORDER BY Optimization](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/improvements-to-order-by/)
- [Joins and Subqueries](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/)
- [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/)
- [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by/)
- [Common Table Expressions](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/)
- [SELECT WITH ROLLUP](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-with-rollup/)
- [SELECT INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile/)
- [SELECT INTO DUMPFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-dumpfile/)
- [FOR UPDATE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/for-update/)
- [LOCK IN SHARE MODE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/lock-in-share-mode/)
- [Optimizer Hints](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/optimizer-hints/)