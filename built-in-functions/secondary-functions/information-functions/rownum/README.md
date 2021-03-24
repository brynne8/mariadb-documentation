# ROWNUM

##### MariaDB starting with [10.6.0](/kb/en/mariadb-1060-release-notes/)

From version 10.6.0, MariaDB supports the <code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> function.

## Syntax

```sql
ROWNUM()
```

In [Oracle mode](/kb/en/sql_modeoracle/) one can just use <code class="fixed" style="white-space:pre-wrap">ROWNUM</code>, without the parentheses.

## Description

<code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> returns the current number of accepted rows in the
current context. It main purpose is to emulate the <code class="fixed" style="white-space:pre-wrap">ROWNUM</code>
[pseudo column in Oracle](https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/ROWNUM-Pseudocolumn.html#GUID-2E40EC12-3FCF-4A4F-B5F2-6BC669021726). For MariaDB native applications, we recommend the usage of [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/), as it is easier to use and gives more predictable results than the usage of <code class="fixed" style="white-space:pre-wrap">ROWNUM()</code>.

The main difference between using <code class="fixed" style="white-space:pre-wrap">LIMIT</code> and
<code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> to limit the rows in the result is that
<code class="fixed" style="white-space:pre-wrap">LIMIT</code> works on the result set while <code class="fixed" style="white-space:pre-wrap">ROWNUM</code>
works on the number of accepted rows (before any <code class="fixed" style="white-space:pre-wrap">ORDER</code> or
<code class="fixed" style="white-space:pre-wrap">GROUP BY</code> clauses).

The following queries will return the same results:

```sql
SELECT * from t1 LIMIT 10;
SELECT * from t1 WHERE ROWNUM() <= 10;
```

While the following may return different results based on in which orders the rows are found:

```sql
SELECT * from t1 ORDER BY a LIMIT 10;
SELECT * from t1 ORDER BY a WHERE ROWNUM() <= 10;
```

The recommended way to use <code class="fixed" style="white-space:pre-wrap">ROWNUM</code> to limit the number of returned rows and get predictable results is to have the query in a sub query and test for <code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> in the outer query:

```sql
SELECT * FROM (select * from t1 ORDER BY a) WHERE ROWNUM() <= 10;
```

<code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> can be used in the following context:

- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
- [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/)
- [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/)
- [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/)
- [LOAD DATA INFILE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/load-data-into-tables-or-index/load-data-infile/)

Used in other contexts, <code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> will return 0.

## Examples

```sql
INSERT INTO t1 VALUES (1,ROWNUM()),(2,ROWNUM()),(3,ROWNUM());

INSERT INTO t1 VALUES (1),(2) returning a, ROWNUM();

UPDATE t1 SET row_num_column=ROWNUM();

DELETE FROM t1 WHERE a < 10 AND ROWNUM() < 2;

LOAD DATA INFILE 'filename' into table t1 fields terminated by ',' 
  lines terminated by "\r\n" (a,b) set c=ROWNUM();
```

## Optimizations

In many cases where <code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> is used, MariaDB will use the same optimizations it uses with [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/).

<code class="fixed" style="white-space:pre-wrap">LIMIT</code> optimization is possible when using <code class="fixed" style="white-space:pre-wrap">ROWNUM</code> in the following manner:

- When one is in a top level <code class="fixed" style="white-space:pre-wrap">WHERE</code> clause comparing <code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> with a numerical constant using any of the following expressions:
<ul start="1"><li><code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> &lt; number
</li><li><code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> &lt;= number
</li><li><code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> = 1
<code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> can be also be the right argument to the comparison function.
</li></ul>

In the above cases, <code class="fixed" style="white-space:pre-wrap">LIMIT</code> optimization can be done in the
following cases:

- For the current sub query when the ROWNUM comparison is done on the top level:

```sql
SELECT * from t1 WHERE ROWNUM() <= 2 AND t1.a > 0
```

- For an inner sub query, when the upper level has only a <code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> comparison in the <code class="fixed" style="white-space:pre-wrap">WHERE</code> clause:

```sql
SELECT * from (select * from t1) as t WHERE ROWNUM() <= 2
```

## Other Changes Related to ROWNUM

When <code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> is used anywhere in a query, the optimization to ignore `ORDER BY` in subqueries are disabled.

This was done to get the following common Oracle query to work as expected:

```sql
 select * from (select * from t1 order by a desc) as t where rownum() <= 2;
```

By default MariaDB ignores any `ORDER BY` in subqueries both because the SQL standard defines results sets in subqueries to be un-ordered and because of performance reasons (especially when using views in subqueries).  See [MDEV-3926](https://jira.mariadb.org/browse/MDEV-3926) "Wrong result with GROUP BY ... WITH ROLLUP" for a discussion of this topic.

## Other Considerations

While MariaDB tries to emulate Oracle's usage of <code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> as closely as possible, there are cases where the result is different:

- When the optimizer finds rows in a different order (because of different storage methods or optimization). This may also happen in Oracle if one adds or deletes an index, in which case the rows may be found in a different order.

Note that usage of <code class="fixed" style="white-space:pre-wrap">ROWNUM()</code> in functions or [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/) will use their own context, not the caller's context.

## See Also

- [MDEV-24089](https://jira.mariadb.org/browse/MDEV-24089) support oracle syntax: rownum
- [LIMIT clause](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/)