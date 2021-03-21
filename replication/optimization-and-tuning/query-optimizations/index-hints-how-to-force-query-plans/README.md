# Index Hints: How to Force Query Plans

The optimizer is largely cost-based and will try to choose the optimal plan for
any query. However in some cases it does not have enough information to choose
a perfect plan and in these cases you may have to provide hints to force the
optimizer to use another plan.

You can examine the query plan for a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) by writing
[EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain) before the statement. As of [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/), [SHOW EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-explain) shows the output of a running query. In some cases, its output can be closer to reality than `EXPLAIN`.

For the following queries, we will use the world database for
the examples.

## Setting up the World Example Database

Download it from [ftp://ftp.askmonty.org/public/world.sql.gz](ftp://ftp.askmonty.org/public/world.sql.gz)

Install it with:

```sql
mysqladmin create world
zcat world.sql.gz | ../client/mysql world
```

or

```sql
mysqladmin create world
gunzip world.sql.gz
../client/mysql world < world.sql
```

## Forcing Join Order

You can force the join order by using <a undefined>STRAIGHT_JOIN</a> either in
the [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) or <a undefined>JOIN</a> part.

The simplest way to force the join order is to put the tables in the correct
order in the `FROM` clause and use `SELECT STRAIGHT_JOIN` like so:

```sql
SELECT STRAIGHT_JOIN SUM(City.Population) FROM Country,City WHERE
City.CountryCode=Country.Code AND Country.HeadOfState="Vladimir Putin";
```

If you only want to force the join order for a few tables, use
`STRAIGHT_JOIN` in the `FROM` clause. When this is done, only tables
connected with `STRAIGHT_JOIN` will have their order forced. For example:

```sql
SELECT SUM(City.Population) FROM Country STRAIGHT_JOIN City WHERE
City.CountryCode=Country.Code AND Country.HeadOfState="Vladimir Putin";
```

In both of the above cases `Country` will be scanned first and for each
matching country (one in this case) all rows in `City` will be checked for a
match. As there is only one matching country this will be faster than the
original query.

The output of [EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain) for the above cases is:

<table><tbody><tr><th>id</th><th>select_type</th><th>table</th><th>type</th><th>possible_keys</th><th>key</th><th>key_len</th><th>ref</th><th>rows</th><th>Extra</th></tr>
<tr><td>1</td><td>SIMPLE</td><td>Country</td><td>ALL</td><td>PRIMARY</td><td>NULL</td><td>NULL</td><td>NULL</td><td>239</td><td>Using where</td></tr>
<tr><td>1</td><td>SIMPLE</td><td>City</td><td>ALL</td><td>NULL</td><td>NULL</td><td>NULL</td><td>NULL</td><td>4079</td><td>Using where; Using join buffer (flat, BNL join)</td></tr>
</tbody></table>

This is one of the few cases where `ALL` is ok, as the scan of the
`Country` table will only find one matching row.

## Forcing Usage of a Specific Index for the WHERE Clause

In some cases the optimizer may choose a non-optimal index or it may choose to
not use an index at all, even if some index could theoretically be used.

In these cases you have the option to either tell the optimizer to only use a
limited set of indexes, ignore one or more indexes, or force the usage of some
particular index.

### USE INDEX: Use a Limited Set of Indexes

You can limit which indexes are considered with the [USE INDEX](/replication/optimization-and-tuning/query-optimizations/use-index)
option.

```sql
USE INDEX [{FOR {JOIN|ORDER BY|GROUP BY}] ([index_list])
```

The default is '`FOR JOIN`', which means that the hint only affects how the
`WHERE` clause is optimized.

`USE INDEX` is used after the table name in the `FROM` clause.

Example:

```sql
CREATE INDEX Name ON City (Name);
CREATE INDEX CountryCode ON City (Countrycode);
EXPLAIN SELECT Name FROM City USE INDEX (CountryCode)
WHERE name="Helsingborg" AND countrycode="SWE";
```

This produces:

<table><tbody><tr><th>id</th><th>select_type</th><th>table</th><th>type</th><th>possible_keys</th><th>key</th><th>key_len</th><th>ref</th><th>rows</th><th>Extra</th></tr>
<tr><td>1</td><td>SIMPLE</td><td>City</td><td>ref</td><td>CountryCode</td><td>CountryCode</td><td>3</td><td>const</td><td>14</td><td>Using where</td></tr>
</tbody></table>

If we had not used [USE INDEX](/replication/optimization-and-tuning/query-optimizations/use-index), the `Name` index would have been in
`possible keys`.

### IGNORE INDEX: Don't Use a Particular Index

You can tell the optimizer to not consider some particular index with the
[IGNORE INDEX](/replication/optimization-and-tuning/query-optimizations/ignore-index) option.

```sql
IGNORE INDEX [{FOR {JOIN|ORDER BY|GROUP BY}] ([index_list])
```

This is used after the table name in the `FROM` clause:

```sql
CREATE INDEX Name ON City (Name);
CREATE INDEX CountryCode ON City (Countrycode);
EXPLAIN SELECT Name FROM City IGNORE INDEX (Name)
WHERE name="Helsingborg" AND countrycode="SWE";
```

This produces:

<table><tbody><tr><th>id</th><th>select_type</th><th>table</th><th>type</th><th>possible_keys</th><th>key</th><th>key_len</th><th>ref</th><th>rows</th><th>Extra</th></tr>
<tr><td>1</td><td>SIMPLE</td><td>City</td><td>ref</td><td>CountryCode</td><td>CountryCode</td><td>3</td><td>const</td><td>14</td><td>Using where</td></tr>
</tbody></table>

The benefit of using `IGNORE_INDEX` instead of `USE_INDEX` is that it will
not disable a new index which you may add later.

### FORCE INDEX: Forcing an Index

[Forcing an index](/replication/optimization-and-tuning/query-optimizations/force-index) to be used is mostly useful when the optimizer
decides to do a table scan even if you know that using an index would
be better. (The optimizer could decide to do a table scan even if there is
an available index when it believes that most or all rows will match and
it can avoid the overhead of using the index).

```sql
CREATE INDEX Name ON City (Name);
EXPLAIN SELECT Name,CountryCode FROM City FORCE INDEX (Name)
WHERE name>="A" and CountryCode >="A";
```

This produces:

<table><tbody><tr><th>id</th><th>select_type</th><th>table</th><th>type</th><th>possible_keys</th><th>key</th><th>key_len</th><th>ref</th><th>rows</th><th>Extra</th></tr>
<tr><td>1</td><td>SIMPLE</td><td>City</td><td>range</td><td>Name</td><td>Name</td><td>35</td><td>NULL</td><td>4079</td><td>Using where</td></tr>
</tbody></table>

`FORCE_INDEX` works by only considering the given indexes (like with
`USE_INDEX`) but in addition it tells the optimizer to regard a table scan as
something very expensive. However if none of the 'forced' indexes can be used,
then a table scan will be used anyway.

### Index Prefixes

When using index hints (USE, FORCE or [IGNORE INDEX](/replication/optimization-and-tuning/query-optimizations/ignore-index)), the index name value can also be an unambiguous prefix of an index name.

## Forcing an Index to be Used for ORDER BY or GROUP BY

The optimizer will try to use indexes to resolve [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by) and [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by).

You can use [USE INDEX](/replication/optimization-and-tuning/query-optimizations/use-index), [IGNORE INDEX](/replication/optimization-and-tuning/query-optimizations/ignore-index) and
[FORCE INDEX](/replication/optimization-and-tuning/query-optimizations/force-index) as in the <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> clause above
to ensure that some specific index used:

```sql
USE INDEX [{FOR {JOIN|ORDER BY|GROUP BY}] ([index_list])
```

This is used after the table name in the <code class="highlight fixed" style="white-space:pre-wrap">FROM</code> clause.

Example:

```sql
CREATE INDEX Name ON City (Name);
EXPLAIN SELECT Name,Count(*) FROM City
FORCE INDEX FOR GROUP BY (Name)
WHERE population >= 10000000 GROUP BY Name;
```

This produces:

<table><tbody><tr><th>id</th><th>select_type</th><th>table</th><th>type</th><th>possible_keys</th><th>key</th><th>key_len</th><th>ref</th><th>rows</th><th>Extra</th></tr>
<tr><td>1</td><td>SIMPLE</td><td>City</td><td>index</td><td>NULL</td><td>Name</td><td>35</td><td>NULL</td><td>4079</td><td>Using where</td></tr>
</tbody></table>

Without the [FORCE INDEX](/replication/optimization-and-tuning/query-optimizations/force-index) option we would have
'<code class="highlight fixed" style="white-space:pre-wrap">Using where; Using temporary; Using filesort</code>' in the
'Extra' column, which means that the optimizer would created a temporary
table and sort it.

### Help the Optimizer Optimize GROUP BY and ORDER BY

The optimizer uses several strategies to optimize [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by)
and [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by):

- Resolve with an index:
<ul start="1"><li>Scan the table in index order and output data as we go. (This only works if
   the [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by) / [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by) can be
   resolved by an index after constant propagation is done).
</li></ul>

- Filesort:
<ul start="1"><li>Scan the table to be sorted and collect the sort keys in a temporary file.
</li><li>Sort the keys + reference to row (with filesort)
</li><li>Scan the table in sorted order
</li></ul>

- Use a temporary table for [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by):
<ul start="1"><li>Create a temporary (in memory) table for the 'to-be-sorted' data. (If this
   gets bigger than <code class="highlight fixed" style="white-space:pre-wrap">max_heap_table_size</code> or contains blobs
   then an [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) or [MyISAM](/kb/en/myisam/) disk based table will be used)
</li><li>Sort the keys + reference to row (with filesort)
</li><li>Scan the table in sorted order
</li></ul>

A temporary table will always be used if the fields which will be sorted are
not from the first table in the [JOIN](/kb/en/join/) order.

- Use a temporary table for [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by):
<ul start="1"><li>Create a temporary table to hold the [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by) result with
   an index that matches the [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by) fields.
</li><li>Produce a result row
</li><li>If a row with the [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by) key exists in the temporary
   table, add the new result row to it. If not, create a new row.
</li><li>Before sending the results to the user, sort the rows with filesort to get
   the results in the [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by) order.
</li></ul>

### Forcing/Disallowing TemporaryTables to be Used for GROUP BY:

Using an in-memory table (as described above) is usually the fastest option for
[GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by) if the result set is small. It is not optimal if
the result set is very big. You can tell the optimizer this by using
<code class="highlight fixed" style="white-space:pre-wrap">SELECT SQL_SMALL_RESULT</code>
or <code class="highlight fixed" style="white-space:pre-wrap">SELECT SQL_BIG_RESULT</code>.

For example:

```sql
EXPLAIN SELECT SQL_SMALL_RESULT Name,Count(*) AS Cities FROM City GROUP BY Name HAVING Cities > 2;
```

produces:

<table><tbody><tr><th>id</th><th>select_type</th><th>table</th><th>type</th><th>possible_keys</th><th>key</th><th>key_len</th><th>ref</th><th>rows</th><th>Extra</th></tr>
<tr><td>1</td><td>SIMPLE</td><td>City</td><td>ALL</td><td>NULL</td><td>NULL</td><td>NULL</td><td>NULL</td><td>4079</td><td>Using temporary; Using filesort</td></tr>
</tbody></table>

while:

```sql
EXPLAIN SELECT SQL_BIG_RESULT Name,Count(*) AS Cities FROM City
GROUP BY Name HAVING Cities > 2;
```

produces:

<table><tbody><tr><th>id</th><th>select_type</th><th>table</th><th>type</th><th>possible_keys</th><th>key</th><th>key_len</th><th>ref</th><th>rows</th><th>Extra</th></tr>
<tr><td>1</td><td>SIMPLE</td><td>City</td><td>ALL</td><td>NULL</td><td>NULL</td><td>NULL</td><td>NULL</td><td>4079</td><td>Using filesort</td></tr>
</tbody></table>

The difference is that with <code class="highlight fixed" style="white-space:pre-wrap">SQL_SMALL_RESULT</code> a
temporary table is used.

## Forcing Usage of Temporary Tables

In some cases you may want to force the use of a temporary table for the result
to free up the table/row locks for the used tables as quickly as possible.

You can do this with the <code class="highlight fixed" style="white-space:pre-wrap">SQL_BUFFER_RESULT</code> option:

```sql
CREATE INDEX Name ON City (Name);
EXPLAIN SELECT SQL_BUFFER_RESULT Name,Count(*) AS Cities FROM City
GROUP BY Name HAVING Cities > 2;
```

This produces:

<table><tbody><tr><th>id</th><th>select_type</th><th>table</th><th>type</th><th>possible_keys</th><th>key</th><th>key_len</th><th>ref</th><th>rows</th><th>Extra</th></tr>
<tr><td>1</td><td>SIMPLE</td><td>City</td><td>index</td><td>NULL</td><td>Name</td><td>35</td><td>NULL</td><td>4079</td><td>Using index; Using temporary</td></tr>
</tbody></table>

Without <code class="highlight fixed" style="white-space:pre-wrap">SQL_BUFFER_RESULT</code>, the above query would not use a
temporary table for the result set.

## Optimizer Switch

In [MariaDB 5.3](/kb/en/what-is-mariadb-53/) we added an [optimizer switch](/kb/en/server-system-variables/#optimizer_switch)
which allows you to specify which algorithms will be considered when optimizing
a query.

See the [optimizer](/kb/en/optimizer/) section for more information about the different
algorithms which are used.

## See Also

- [FORCE INDEX](/replication/optimization-and-tuning/query-optimizations/force-index)
- [USE INDEX](/replication/optimization-and-tuning/query-optimizations/use-index)
- [IGNORE INDEX](/replication/optimization-and-tuning/query-optimizations/ignore-index)
- [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by)