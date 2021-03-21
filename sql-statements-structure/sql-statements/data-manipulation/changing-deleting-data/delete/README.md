# DELETE

## Syntax

Single-table syntax:

```sql
DELETE [LOW_PRIORITY] [QUICK] [IGNORE] 
  FROM tbl_name [PARTITION (partition_list)]
  [WHERE where_condition]
  [ORDER BY ...]
  [LIMIT row_count]
  [RETURNING select_expr 
    [, select_expr ...]]
```

Multiple-table syntax:

```sql
DELETE [LOW_PRIORITY] [QUICK] [IGNORE]
    tbl_name[.*] [, tbl_name[.*]] ...
    FROM table_references
    [WHERE where_condition]
```

Or:

```sql
DELETE [LOW_PRIORITY] [QUICK] [IGNORE]
    FROM tbl_name[.*] [, tbl_name[.*]] ...
    USING table_references
    [WHERE where_condition]
```

Trimming history:

```sql
DELETE HISTORY
  FROM tbl_name [PARTITION (partition_list)]
  [BEFORE SYSTEM_TIME [TIMESTAMP|TRANSACTION] expression]
```

## Description

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td>LOW_PRIORITY</td><td>Wait until all SELECT's are done before starting the statement. Used with storage engines that uses table locking (MyISAM, Aria etc). See <a href="/kb/en/high_priority-and-low_priority-clauses/">HIGH_PRIORITY and LOW_PRIORITY clauses</a> for details.</td></tr>
<tr><td>QUICK</td><td>Signal the storage engine that it should expect that a lot of rows are deleted. The storage engine engine can do things to speed up the DELETE like ignoring merging of data blocks until all rows are deleted from the block (instead of when a block is half full). This speeds up things at the expanse of lost space in data blocks. At least <a href="/kb/en/myisam/">MyISAM</a> and <a href="/kb/en/aria/">Aria</a> support this feature.</td></tr>
<tr><td>IGNORE</td><td>Don't stop the query even if a not-critical error occurs (like data overflow). See <a href="/kb/en/ignore/">How IGNORE works</a> for a full description.</td></tr>
</tbody></table>

For the single-table syntax, the <code class="fixed" style="white-space:pre-wrap">DELETE</code> statement deletes rows
from tbl_name and returns a count of the number of deleted rows. This count can
be obtained by calling the [ROW_COUNT()](/kb/en/row-count/) function. The
<code class="fixed" style="white-space:pre-wrap">WHERE</code> clause, if given, specifies the conditions that identify
which rows to delete. With no <code class="fixed" style="white-space:pre-wrap">WHERE</code> clause, all rows are
deleted. If the [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by) clause is specified, the rows are
deleted in the order that is specified. The [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit) clause
places a limit on the number of rows that can be deleted.

For the multiple-table syntax, <code class="fixed" style="white-space:pre-wrap">DELETE</code> deletes from each
<code class="fixed" style="white-space:pre-wrap">tbl_name</code> the rows that satisfy the conditions. In this case,
[ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by) and [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit)&gt; cannot be used. A `DELETE` can also reference tables which are located in different databases; see [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers) for the syntax.

<code class="fixed" style="white-space:pre-wrap">where_condition</code> is an expression that evaluates to true for
each row to be deleted. It is specified as described in [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select).

Currently, you cannot delete from a table and select from the same
table in a subquery.

You need the <code class="fixed" style="white-space:pre-wrap">DELETE</code> privilege on a table to delete rows from
it. You need only the <code class="fixed" style="white-space:pre-wrap">SELECT</code> privilege for any columns that
are only read, such as those named in the <code class="fixed" style="white-space:pre-wrap">WHERE</code> clause. See
[GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant).

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The PARTITION clause was introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/). See [Partition Pruning and Selection](/mariadb-administration/partitioning-tables/partition-pruning-and-selection) for details.

As stated, a <code class="highlight fixed" style="white-space:pre-wrap">DELETE</code> statement with no <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code>
clause deletes all rows. A faster way to do this, when you do not need to know
the number of deleted rows, is to use <code class="highlight fixed" style="white-space:pre-wrap">TRUNCATE TABLE</code>. However,
within a transaction or if you have a lock on the table, 
<code class="fixed" style="white-space:pre-wrap">TRUNCATE TABLE</code> cannot be used whereas <code class="fixed" style="white-space:pre-wrap">DELETE</code>
can. See <code class="highlight fixed" style="white-space:pre-wrap">[TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table)</code>, and
<code class="highlight fixed" style="white-space:pre-wrap">[LOCK](/kb/en/lock/)</code>.

### RETURNING

It is possible to return a resultset of the deleted rows for a single table to the client by using the syntax `DELETE ... RETURNING select_expr [, select_expr2 ...]]`

Any of SQL expression that can be calculated from a single row fields is allowed. Subqueries are allowed. The AS keyword is allowed, so it is possible to use aliases.

The use of aggregate functions is not allowed. RETURNING cannot be used in multi-table DELETEs.

##### MariaDB starting with [10.3.1](/kb/en/mariadb-1031-release-notes/)

### Same Source and Target Table

Until [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), deleting from a table with the same source and target was not possible. From [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), this is now possible. For example:

```sql
DELETE FROM t1 WHERE c1 IN (SELECT b.c1 FROM t1 b WHERE b.c2=0);
```

##### MariaDB starting with [10.3.4](/kb/en/mariadb-1034-release-notes/)

One can use `DELETE HISTORY` to delete historical information from [System-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables).

## Examples

How to use the [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by) and [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit) clauses:

```sql
DELETE FROM page_hit ORDER BY timestamp LIMIT 1000000;
```

How to use the RETURNING clause:

```sql
DELETE FROM t RETURNING f1;
+------+
| f1   |
+------+
|    5 |
|   50 |
|  500 |
+------+ 
```

The following statement joins two tables: one is only used to satisfy a WHERE condition, but no row is deleted from it; rows from the other table are deleted, instead.

```sql
DELETE post FROM blog INNER JOIN post WHERE blog.id = post.blog_id;
```

### Deleting from the Same Source and Target

```sql
CREATE TABLE t1 (c1 INT, c2 INT);
DELETE FROM t1 WHERE c1 IN (SELECT b.c1 FROM t1 b WHERE b.c2=0);
```

Until [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), this returned:

```sql
ERROR 1093 (HY000): Table 't1' is specified twice, both as a target for 'DELETE' 
  and as a separate source for
```

From [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/):

```sql
Query OK, 0 rows affected (0.00 sec)
```

## See Also

- [How IGNORE works](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/ignore)
- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select)
- [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by)
- [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit)
- [REPLACE ... RETURNING](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replacereturning)
- [INSERT ... RETURNING](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insertreturning)
- [Returning clause](https://www.youtube.com/watch?v=n-LTdEBeAT4) (video)