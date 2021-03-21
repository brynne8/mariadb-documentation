# EXPLAIN

## Syntax

```sql
EXPLAIN tbl_name
```

Or

```sql
EXPLAIN [EXTENDED | PARTITIONS] 
  {SELECT select_options | UPDATE update_options | DELETE delete_options}
```

## Description

The <code class="highlight fixed" style="white-space:pre-wrap">EXPLAIN</code> statement can be used either as a synonym for
<code class="highlight fixed" style="white-space:pre-wrap">DESCRIBE</code> or as a way to obtain information about how MariaDB
executes a <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code> (as well as `UPDATE` and `DELETE` since [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)) statement:

- <code class="highlight fixed" style="white-space:pre-wrap">'EXPLAIN tbl_name'</code> is synonymous with 
  <code class="highlight fixed" style="white-space:pre-wrap">'[DESCRIBE](/kb/en/describe-command/) tbl_name'</code> or 
  <code class="highlight fixed" style="white-space:pre-wrap">'[SHOW COLUMNS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-columns/) FROM tbl_name'</code>.
- When you precede a <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code> statement (or, since [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/), an `UPDATE` or a `DELETE` as well) with the keyword 
  <code class="highlight fixed" style="white-space:pre-wrap">EXPLAIN</code>, MariaDB displays information from the optimizer
  about the query execution plan. That is, MariaDB explains how it would
  process the <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code>, `UPDATE` or `DELETE`, including information about how tables
  are joined and in which order. <code class="highlight fixed" style="white-space:pre-wrap">EXPLAIN EXTENDED</code> can be
  used to provide additional information.<br>
- <code class="highlight fixed" style="white-space:pre-wrap">EXPLAIN PARTITIONS</code> has been available since MySQL 5.1.5. It is useful only when examining queries involving partitioned tables.<br> For details, see [Partition pruning and selection](/mariadb-administration/partitioning-tables/partition-pruning-and-selection/).
- [ANALYZE statement](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/analyze-statement/), which performs the query as well as producing EXPLAIN output, and provides actual as well as estimated statistics, has been available from [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/).
- Since [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/), it has been possible to have `EXPLAIN` output printed in the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/). See [EXPLAIN in the Slow Query Log](/mariadb-administration/server-monitoring-logs/slow-query-log/explain-in-the-slow-query-log/) for details.

Since [MariaDB 10.0](/kb/en/what-is-mariadb-100/), [SHOW EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-explain/) shows the output of a running statement. In some cases, its output can be closer to reality than `EXPLAIN`.

Since [MariaDB 10.1](/kb/en/what-is-mariadb-101/), the [ANALYZE statement](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/analyze-statement/) runs a statement and returns information about its execution plan. It also shows additional columns, to check how much the optimizer's estimation about filtering and found rows are close to reality.

There is an online [EXPLAIN Analyzer](/clients-utilities/explain-analyzer/) that you can use to share `EXPLAIN` and `EXPLAIN EXTENDED` output with others.

`EXPLAIN` can acquire metadata locks in the same way that `SELECT` does, as it needs to know table metadata and, sometimes, data as well.

### Columns in EXPLAIN ... SELECT

<table><tbody><tr><th>Column name</th><th>Description</th></tr>
<tr><td><code>id</code></td><td>Sequence number that shows in which order tables are joined.</td></tr>
<tr><td><code>select_type</code></td><td>What kind of <code>SELECT</code> the table comes from.</td></tr>
<tr><td><code>table</code></td><td>Alias name of table. Materialized temporary tables for sub queries are named &lt;subquery#&gt;</td></tr>
<tr><td><code>type</code></td><td>How rows are found from the table (join type).</td></tr>
<tr><td><code>possible_keys</code></td><td>keys in table that could be used to find rows in the table</td></tr>
<tr><td><code>key</code></td><td>The name of the key that is used to retrieve rows. <code>NULL</code> is no key was used.</td></tr>
<tr><td><code>key_len</code></td><td>How many bytes of the key that was used (shows if we are using only parts of the multi-column key).</td></tr>
<tr><td><code>ref</code></td><td>The reference that is used as the key value.</td></tr>
<tr><td><code>rows</code></td><td>An estimate of how many rows we will find in the table for each key lookup.</td></tr>
<tr><td><code>Extra</code></td><td>Extra information about this join.</td></tr>
</tbody></table>

Here are descriptions of the values for some of the more complex columns in `EXPLAIN ... SELECT`:

#### "Select_type" Column

The `select_type` column can have the following values:

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td><code>DEPENDENT SUBQUERY</code></td><td>The <code>SUBQUERY</code> is <code>DEPENDENT</code>.</td></tr>
<tr><td><code>DEPENDENT UNION</code></td><td>The <code>UNION</code> is <code>DEPENDENT</code>.</td></tr>
<tr><td><code>DERIVED</code></td><td>The <code>SELECT</code> is <code>DERIVED</code> from the <code>PRIMARY</code>.</td></tr>
<tr><td><code>MATERIALIZED</code></td><td>The <code>SUBQUERY</code> is <code><a href="/kb/en/semi-join-materialization-strategy/">MATERIALIZED</a></code>.</td></tr>
<tr><td><code>PRIMARY</code></td><td>The <code>SELECT</code> is a <code>PRIMARY</code> one.</td></tr>
<tr><td><code>SIMPLE</code></td><td>The <code>SELECT</code> is a <code>SIMPLE</code> one.</td></tr>
<tr><td><code>SUBQUERY</code></td><td>The <code>SELECT</code> is a <code>SUBQUERY</code> of the <code>PRIMARY</code>.</td></tr>
<tr><td><code>UNCACHEABLE SUBQUERY</code></td><td>The <code>SUBQUERY</code> is <code>UNCACHEABLE</code>.</td></tr>
<tr><td><code>UNCACHEABLE UNION</code></td><td>The <code>UNION</code> is <code>UNCACHEABLE</code>.</td></tr>
<tr><td><code>UNION</code></td><td>The <code>SELECT</code> is a <code>UNION</code> of the <code>PRIMARY</code>.</td></tr>
<tr><td><code>UNION RESULT</code></td><td>The result of the <code>UNION</code>.</td></tr>
<tr><td><code>LATERAL DERIVED</code></td><td>The <code>SELECT</code> uses a <a href="/kb/en/lateral-derived-optimization/">Lateral Derived optimization</a></td></tr>
</tbody></table>

#### "Type" Column

This column contains information on how the table is accessed.

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td><code>ALL</code></td><td>A full table scan is done for the table (all rows are read). This is bad if the table is large and the table is joined against a previous table!  This happens when the optimizer could not find any usable index to access rows.</td></tr>
<tr><td><code>const</code></td><td>There is only one possibly matching row in the table. The row is read before the optimization phase and all columns in the table are treated as constants.</td></tr>
<tr><td><code>eq_ref</code></td><td>A unique index is used to find the rows. This is the best possible plan to find the row.</td></tr>
<tr><td><code>fulltext</code></td><td>A fulltext index is used to access the rows.</td></tr>
<tr><td><code>index_merge</code></td><td>A 'range' access is done for for several index and the found rows are merged. The key column shows which keys are used.</td></tr>
<tr><td><code>index_subquery</code></td><td>This is similar as ref, but used for sub queries that are transformed to key lookups.</td></tr>
<tr><td><code>index</code></td><td>A full scan over the used index.  Better than ALL but still bad if index is large and the table is joined against a previous table.</td></tr>
<tr><td><code>range</code></td><td>The table will be accessed with a key over one or more value ranges.</td></tr>
<tr><td><code>ref_or_null</code></td><td>Like 'ref' but in addition another search for the 'null' value is done if the first value was not found. This happens usually with sub queries.</td></tr>
<tr><td><code>ref</code></td><td>A non unique index or prefix of an unique index is used to find the rows. Good if the prefix doesn't match many rows.</td></tr>
<tr><td><code>system</code></td><td>The table has 0 or 1 rows.</td></tr>
<tr><td><code>unique_subquery</code></td><td>This is similar as eq_ref, but used for sub queries that are transformed to key lookups</td></tr>
</tbody></table>

#### "Extra" Column

This column consists of one or more of the following values, separated by ';'

<em>Note that some of these values are detected after the optimization phase.</em>

The optimization phase can do the following changes to the `WHERE` clause:

- Add the expressions from the `ON` and `USING` clauses to the `WHERE`
  clause.
- Constant propagation:  If there is `column=constant`, replace all column
  instances with this constant.
- Replace all columns from '`const`' tables with their values.
- Remove the used key columns from the `WHERE` (as this will be tested as
  part of the key lookup).
- Remove impossible constant sub expressions.
  For example `WHERE '(a=1 and a=2) OR b=1'` becomes `'b=1'`.
- Replace columns with other columns that has identical values:
  Example:  `WHERE` `a=b` and `a=c` may be treated
  as `'WHERE a=b and a=c and b=c'`.
- Add extra conditions to detect impossible row conditions earlier. This
  happens mainly with `OUTER JOIN` where we in some cases add detection
  of `NULL` values in the `WHERE` (Part of '`Not exists`' optimization).
  This can cause an unexpected '`Using where`' in the Extra column.
- For each table level we remove expressions that have already been tested when
  we read the previous row. Example: When joining tables `t1` with `t2`
  using the following `WHERE 't1.a=1 and t1.a=t2.b'`, we don't have to
  test `'t1.a=1'` when checking rows in `t2` as we already know that this
  expression is true.

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td><code>const row not found</code></td><td>The table was a system table (a table with should exactly one row), but no row was found.</td></tr>
<tr><td><code>Distinct</code></td><td>If distinct optimization (remove duplicates) was used. This is marked only for the last table in the <code>SELECT</code>.</td></tr>
<tr><td><code>Full scan on NULL key</code></td><td>The table is a part of the sub query and if the value that is used to match the sub query will be <code>NULL</code>, we will do a full table scan.</td></tr>
<tr><td><code>Impossible HAVING</code></td><td>The used <code>HAVING</code> clause is always false so the <code>SELECT</code> will return no rows.</td></tr>
<tr><td><code>Impossible WHERE</code> <code>noticed</code> <code>after</code> <code>reading</code> <code>const</code> <code>tables.</code></td><td>The used <code>WHERE</code> clause is always false so the <code>SELECT</code> will return no rows. This case was detected after we had read all 'const' tables and used the column values as constant in the <code>WHERE</code> clause. For example: <code>WHERE const_column=5</code> and <code>const_column</code> had a value of 4.</td></tr>
<tr><td><code>Impossible WHERE</code></td><td>The used <code>WHERE</code> clause is always false so the <code>SELECT</code> will return no rows. For example: <code>WHERE 1=2</code></td></tr>
<tr><td><code>No matching min/max row</code></td><td>During early optimization of <code>MIN()</code>/<code>MAX()</code> values it was detected that no row could match the <code>WHERE</code> clause. The <code>MIN()</code>/<code>MAX()</code> function will return <code>NULL</code>.</td></tr>
<tr><td><code>no matching row</code> <code>in</code> <code>const</code> <code>table</code></td><td>The table was a const table (a table with only one possible matching row), but no row was found.</td></tr>
<tr><td><code>No tables used</code></td><td>The <code>SELECT</code> was a sub query that did not use any tables. For example a there was no <code>FROM</code> clause or a <code>FROM DUAL</code> clause.</td></tr>
<tr><td><code>Not exists</code></td><td>Stop searching after more row if we find one single matching row. This optimization is used with <code>LEFT JOIN</code> where one is explicitly searching for rows that doesn't exists in the <code>LEFT JOIN TABLE</code>. Example: <code>SELECT * FROM t1 LEFT JOIN t2 on (...) WHERE t2.not_null_column IS NULL</code>.  As <code>t2.not_null_column</code> can only be <code>NULL</code> if there was no matching row for on condition, we can stop searching if we find a single matching row.</td></tr>
<tr><td><code>Open_frm_only</code></td><td>For <code>information_schema</code> tables.  Only the <code>frm</code> (table definition file was opened) was opened for each matching row.</td></tr>
<tr><td><code>Open_full_table</code></td><td>For <code>information_schema</code> tables. A full table open for each matching row is done to retrieve the requested information. (Slow)</td></tr>
<tr><td><code>Open_trigger_only</code></td><td>For <code>information_schema</code> tables. Only the trigger file definition was opened for each matching row.</td></tr>
<tr><td><code>Range checked</code> <code>for</code> <code>each</code> <code>record</code> <code>(index map: ...)</code></td><td>This only happens when there was no good default index to use but there may some index that could be used when we can treat all columns from previous table as constants.  For each row combination the optimizer will decide which index to use (if any) to fetch a row from this table. This is not fast, but faster than a full table scan that is the only other choice. The index map is a bitmask that shows which index are considered for each row condition.</td></tr>
<tr><td><code>Scanned 0/1/all databases</code></td><td>For <code>information_schema</code> tables. Shows how many times we had to do a directory scan.</td></tr>
<tr><td><code>Select tables</code> <code>optimized</code> <code>away</code></td><td>All tables in the join was optimized away. This happens when we are only using <code>COUNT(*)</code>, <code>MIN()</code> and <code>MAX()</code> functions in the <code>SELECT</code> and we where able to replace all of these with constants.</td></tr>
<tr><td><code>Skip_open_table</code></td><td>For <code>information_schema</code> tables. The queried table didn't need to be opened.</td></tr>
<tr><td><code>unique row not found</code></td><td>The table was detected to be a const table (a table with only one possible matching row) during the early optimization phase, but no row was found.</td></tr>
<tr><td><code>Using filesort</code></td><td>Filesort is needed to resolve the query. This means an extra phase where we first collect all columns to sort, sort them with a disk based merge sort and then use the sorted set to retrieve the rows in sorted order. If the column set is small, we store all the columns in the sort file to not have to go to the database to retrieve them again.</td></tr>
<tr><td><code>Using index</code></td><td>Only the index is used to retrieve the needed information from the table. There is no need to perform an extra seek to retrieve the actual record.</td></tr>
<tr><td><code>Using index condition</code></td><td>Like '<code>Using where</code>' but the where condition is pushed down to the table engine for internal optimization at the index level.</td></tr>
<tr><td><code>Using index condition(BKA)</code></td><td>Like '<code>Using index condition</code>' but in addition we use batch key access to retrieve rows.</td></tr>
<tr><td><code>Using index for group-by</code></td><td>The index is being used to resolve a <code>GROUP BY</code> or <code>DISTINCT</code> query. The rows are not read.  This is very efficient if the table has a lot of identical index entries as duplicates are quickly jumped over.</td></tr>
<tr><td><code>Using intersect(...)</code></td><td>For <code>index_merge</code> joins. Shows which index are part of the intersect.</td></tr>
<tr><td><code>Using join buffer</code></td><td>We store previous row combinations in a row buffer to be able to match each row against all of the rows combinations in the join buffer at one go.</td></tr>
<tr><td><code>Using sort_union(...)</code></td><td>For <code>index_merge</code> joins. Shows which index are part of the union.</td></tr>
<tr><td><code>Using temporary</code></td><td>A temporary table is created to hold the result. This typically happens if you are using <code>GROUP BY</code>, <code>DISTINCT</code> or <code>ORDER BY</code>.</td></tr>
<tr><td><code>Using where</code></td><td>A <code>WHERE</code> expression (in additional to the possible key lookup) is used to check if the row should be accepted. If you don't have 'Using where' together with a join type of <code>ALL</code>, you are probably doing something wrong!</td></tr>
<tr><td><code>Using where</code> <code>with</code> <code>pushed</code> <code>condition</code></td><td>Like '<code>Using where</code>' but the where condition is pushed down to the table engine for internal optimization at the row level.</td></tr>
<tr><td><code>Using buffer</code></td><td>The <code>UPDATE</code> statement will first buffer the rows, and then run the updates, rather than do updates on the fly. See <a href="/kb/en/using-buffer-update-algorithm/">Using Buffer UPDATE Algorithm</a> for a detailed explanation.</td></tr>
</tbody></table>

### EXPLAIN EXTENDED

The `EXTENDED` keyword adds another column, <em>filtered</em>, to the output. This is a percentage estimate of the table rows that will be filtered by the condition.

An `EXPLAIN EXTENDED` will always throw a warning, as it adds extra <em>Message</em> information to a subsequent [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/) statement. This includes what the `SELECT` query would look like after optimizing and rewriting rules are applied and how the optimizer qualifies columns and tables.

## Examples

As synonym for `DESCRIBE` or `SHOW COLUMNS FROM`:

```sql
DESCRIBE city;
+------------+----------+------+-----+---------+----------------+
| Field      | Type     | Null | Key | Default | Extra          |
+------------+----------+------+-----+---------+----------------+
| Id         | int(11)  | NO   | PRI | NULL    | auto_increment |
| Name       | char(35) | YES  |     | NULL    |                |
| Country    | char(3)  | NO   | UNI |         |                |
| District   | char(20) | YES  | MUL |         |                |
| Population | int(11)  | YES  |     | NULL    |                |
+------------+----------+------+-----+---------+----------------+
```

A simple set of examples to see how `EXPLAIN` can identify poor index usage:

```sql
CREATE TABLE IF NOT EXISTS `employees_example` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `first_name` varchar(30) NOT NULL,
  `last_name` varchar(40) NOT NULL,
  `position` varchar(25) NOT NULL,
  `home_address` varchar(50) NOT NULL,
  `home_phone` varchar(12) NOT NULL,
  `employee_code` varchar(25) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `employee_code` (`employee_code`),
  KEY `first_name` (`first_name`,`last_name`)
) ENGINE=Aria;

INSERT INTO `employees_example` (`first_name`, `last_name`, `position`, `home_address`, `home_phone`, `employee_code`)
  VALUES
  ('Mustapha', 'Mond', 'Chief Executive Officer', '692 Promiscuous Plaza', '326-555-3492', 'MM1'),
  ('Henry', 'Foster', 'Store Manager', '314 Savage Circle', '326-555-3847', 'HF1'),
  ('Bernard', 'Marx', 'Cashier', '1240 Ambient Avenue', '326-555-8456', 'BM1'),
  ('Lenina', 'Crowne', 'Cashier', '281 Bumblepuppy Boulevard', '328-555-2349', 'LC1'),
  ('Fanny', 'Crowne', 'Restocker', '1023 Bokanovsky Lane', '326-555-6329', 'FC1'),
  ('Helmholtz', 'Watson', 'Janitor', '944 Soma Court', '329-555-2478', 'HW1');

SHOW INDEXES FROM employees_example;
+-------------------+------------+---------------+--------------+---------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table             | Non_unique | Key_name      | Seq_in_index | Column_name   | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------------------+------------+---------------+--------------+---------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| employees_example |          0 | PRIMARY       |            1 | id            | A         |           7 |     NULL | NULL   |      | BTREE      |         |               |
| employees_example |          0 | employee_code |            1 | employee_code | A         |           7 |     NULL | NULL   |      | BTREE      |         |               |
| employees_example |          1 | first_name    |            1 | first_name    | A         |        NULL |     NULL | NULL   |      | BTREE      |         |               |
| employees_example |          1 | first_name    |            2 | last_name     | A         |        NULL |     NULL | NULL   |      | BTREE      |         |               |
+-------------------+------------+---------------+--------------+---------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
```

`SELECT` on a primary key:

```sql
EXPLAIN SELECT * FROM employees_example WHERE id=1;
+------+-------------+-------------------+-------+---------------+---------+---------+-------+------+-------+
| id   | select_type | table             | type  | possible_keys | key     | key_len | ref   | rows | Extra |
+------+-------------+-------------------+-------+---------------+---------+---------+-------+------+-------+
|    1 | SIMPLE      | employees_example | const | PRIMARY       | PRIMARY | 4       | const |    1 |       |
+------+-------------+-------------------+-------+---------------+---------+---------+-------+------+-------+
```

The type is <em>const</em>, which means that only one possible result could be returned. 
Now, returning the same record but searching by their phone number:

```sql
EXPLAIN SELECT * FROM employees_example WHERE home_phone='326-555-3492';
+------+-------------+-------------------+------+---------------+------+---------+------+------+-------------+
| id   | select_type | table             | type | possible_keys | key  | key_len | ref  | rows | Extra       |
+------+-------------+-------------------+------+---------------+------+---------+------+------+-------------+
|    1 | SIMPLE      | employees_example | ALL  | NULL          | NULL | NULL    | NULL |    6 | Using where |
+------+-------------+-------------------+------+---------------+------+---------+------+------+-------------+
```

Here, the type is <em>All</em>, which means no index could be used. Looking at the rows count, a full table scan (all six rows) had to be performed in order to retrieve the record. If it's a requirement to search by phone number, an index will have to be created.

[SHOW EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-explain/) example:

```sql
SHOW EXPLAIN FOR 1;
+------+-------------+-------+-------+---------------+------+---------+------+---------+-------------+
| id   | select_type | table | type  | possible_keys | key  | key_len | ref  | rows    | Extra       |
+------+-------------+-------+-------+---------------+------+---------+------+---------+-------------+
|    1 | SIMPLE      | tbl   | index | NULL          | a    | 5       | NULL | 1000107 | Using index |
+------+-------------+-------+-------+---------------+------+---------+------+---------+-------------+
1 row in set, 1 warning (0.00 sec)
```

### Example of <code class="highlight fixed" style="white-space:pre-wrap">ref_or_null</code> Optimization

```sql
SELECT * FROM table_name
  WHERE key_column=expr OR key_column IS NULL;
```

<code class="highlight fixed" style="white-space:pre-wrap">ref_or_null</code> is something that often happens when you use subqueries with <code class="highlight fixed" style="white-space:pre-wrap">NOT IN</code> as then one has to do an extra check for `NULL` values if the first value didn't have a matching row.

## See Also

- [SHOW EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-explain/)