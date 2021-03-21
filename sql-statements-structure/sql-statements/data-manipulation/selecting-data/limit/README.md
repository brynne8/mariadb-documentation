# LIMIT

## Description

Use the `LIMIT` clause to restrict the number of returned rows. When you use a single
integer <em>n</em> with `LIMIT`, the first <em>n</em> rows will be returned. Use the [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/)
clause to control which rows come first. You can also select a number of rows after an offset
using either of the following:

```sql
LIMIT offset, row_count
LIMIT row_count OFFSET offset
```

When you provide an offset <em>m</em> with a limit <em>n</em>, the first <em>m</em> rows will be ignored, and the
following <em>n</em> rows will be returned.

Executing an [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) with the `LIMIT` clause is not safe for replication.

##### MariaDB starting with [10.0.11](/kb/en/mariadb-10011-release-notes/)

Since [MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/), `LIMIT 0` has been an exception to this rule (see [MDEV-6170](https://jira.mariadb.org/browse/MDEV-6170)).

There is a [LIMIT ROWS EXAMINED](/replication/optimization-and-tuning/query-optimizations/limit-rows-examined/) optimization which provides the
means to terminate the execution of [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statements which examine too
many rows, and thus use too many resources. See [LIMIT ROWS EXAMINED](/replication/optimization-and-tuning/query-optimizations/limit-rows-examined/).

### Multi-Table Updates

##### MariaDB starting with [10.3.2](/kb/en/mariadb-1032-release-notes/)

Until [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), it was not possible to use `LIMIT` (or [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/)) in a multi-table [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) statement. This restriction was lifted in [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/).

### GROUP_CONCAT

##### MariaDB starting with [10.3.2](/kb/en/mariadb-1032-release-notes/)

Starting from [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/), it is possible to use `LIMIT` with [GROUP_CONCAT()](/built-in-functions/aggregate-functions/group_concat/).

## Examples

```sql
CREATE TABLE members (name VARCHAR(20));
INSERT INTO members VALUES('Jagdish'),('Kenny'),('Rokurou'),('Immaculada');

SELECT * FROM members;
+------------+
| name       |
+------------+
| Jagdish    |
| Kenny      |
| Rokurou    |
| Immaculada |
+------------+
```

Select the first two names (no ordering specified):

```sql
SELECT * FROM members LIMIT 2;
+---------+
| name    |
+---------+
| Jagdish |
| Kenny   |
+---------+
```

All the names in alphabetical order:

```sql
SELECT * FROM members ORDER BY name;
+------------+
| name       |
+------------+
| Immaculada |
| Jagdish    |
| Kenny      |
| Rokurou    |
+------------+
```

The first two names, ordered alphabetically:

```sql
SELECT * FROM members ORDER BY name LIMIT 2;
+------------+
| name       |
+------------+
| Immaculada |
| Jagdish    |
+------------+
```

The third name, ordered alphabetically (the first name would be offset zero, so the third is offset two):

```sql
SELECT * FROM members ORDER BY name LIMIT 2,1;
+-------+
| name  |
+-------+
| Kenny |
+-------+
```

From [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/), `LIMIT` can be used in a multi-table update:

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

From [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/), `LIMIT` can be used with [GROUP_CONCAT](/built-in-functions/aggregate-functions/group_concat/), so, for example, given the following table:

```sql
CREATE TABLE d (dd DATE, cc INT);

INSERT INTO d VALUES ('2017-01-01',1);
INSERT INTO d VALUES ('2017-01-02',2);
INSERT INTO d VALUES ('2017-01-04',3);
```

the following query:

```sql
SELECT SUBSTRING_INDEX(GROUP_CONCAT(CONCAT_WS(":",dd,cc) ORDER BY cc DESC),",",1) FROM d;
+----------------------------------------------------------------------------+
| SUBSTRING_INDEX(GROUP_CONCAT(CONCAT_WS(":",dd,cc) ORDER BY cc DESC),",",1) |
+----------------------------------------------------------------------------+
| 2017-01-04:3                                                               |
+----------------------------------------------------------------------------+
```

can be more simply rewritten as:

```sql
SELECT GROUP_CONCAT(CONCAT_WS(":",dd,cc) ORDER BY cc DESC LIMIT 1) FROM d;
+-------------------------------------------------------------+
| GROUP_CONCAT(CONCAT_WS(":",dd,cc) ORDER BY cc DESC LIMIT 1) |
+-------------------------------------------------------------+
| 2017-01-04:3                                                |
+-------------------------------------------------------------+
```

## See Also

- [ROWNUM() function](/built-in-functions/secondary-functions/information-functions/rownum/)
- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
- [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/)
- [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/)
- [Joins and Subqueries](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/)
- [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/)
- [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by/)
- [Common Table Expressions](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/)
- [SELECT WITH ROLLUP](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-with-rollup/)
- [SELECT INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile/)
- [SELECT INTO DUMPFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-dumpfile/)
- [FOR UPDATE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/for-update/)
- [LOCK IN SHARE MODE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/lock-in-share-mode/)
- [Optimizer Hints](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/optimizer-hints/)