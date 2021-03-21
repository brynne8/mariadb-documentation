# Histogram-Based Statistics

##### MariaDB starting with [10.0.2](/kb/en/mariadb-1002-release-notes/)

Histograms were introduced in [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/), and are collected by default from [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/).

Histogram-based statistics were introduced in [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/) as a mechanism to improve the query plan chosen by the optimizer in certain situations. Until then, all conditions on non-indexed columns were ignored when searching for the best execution plan. Histograms can be collected for both indexed and non-indexed columns, and are made available to the optimizer.

Histogram statistics are stored in the [mysql.column_stats](/kb/en/mysqlcolumn_stats-table/) table, which stores data for [engine-independent table statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics), and so are essentially a subset of engine-independent table statistics.

Consider this example, using the following query:

```sql
SELECT * FROM t1,t2 WHERE t1.a=t2.a and t2.b BETWEEN 1 AND 3;
```

Let's assume that

- table t1 contains 100 records
- table t2 contains 1000 records
- there is a primary index on t1(a)
- there is a secondary index on t2(a)
- there is no index defined on column t2.b
- the selectivity of the condition t2.b BETWEEN (1,3) is high (~ 1%)

Before histograms were introduced, the optimizer would choose the plan that:

- accesses t1 using a table scan
- accesses t2 using index t2(a)
- checks the condition t2.b BETWEEN 1 AND 3

This plan examines all rows of both tables and performs 100 index look-ups.

With histograms available, the optimizer can choose the following, more efficient plan:

- accesses table t2 in a table scan
- checks the condition t2.b BETWEEN 1 AND 3
- accesses t1 using index t1(a)

This plan also examine all rows from t2, but it performs only 10 look-ups to access 10 rows of table t1.

## System Variables

There are a number of system variables that affect histograms.

### histogram_size

The [histogram_size](/kb/en/server-system-variables/#histogram_size) variable determines the size, in bytes, from 0 to 255, used for a histogram. This is effectively the number of bins for `histogram_type=SINGLE_PREC_HB` or number of bins/2 for `histogram_type=DOUBLE_PREC_HB`. If it is set to 0 (the default for [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/) and below), no histograms are created when running an [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table).

### histogram_type

The [histogram_type](/kb/en/server-system-variables/#histogram_type) variable determines whether single precision (`SINGLE_PREC_HB`) or double precision (`DOUBLE_PREC_HB`) height-balanced histograms are created. From [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/), double precision is the default. For [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/) and below, single precision is the default .

### optimizer_use_condition_selectivity

The [optimizer_use_condition_selectivity](/kb/en/server-system-variables/#optimizer_use_condition_selectivity) controls which statistics can be used by the optimizer when looking for the best query execution plan.

- `1` Use selectivity of predicates as in [MariaDB 5.5](/kb/en/what-is-mariadb-55/).
- `2` Use selectivity of all range predicates supported by indexes.
- `3` Use selectivity of all range predicates estimated without histogram.
- `4` Use selectivity of all range predicates estimated with histogram.
- `5` Additionally use selectivity of certain non-range predicates calculated on record sample.

From [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/), the default is `4`. Until [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/), the default is `1`.

## Example

Here is an example of the dramatic impact histogram-based statistics can make. The query is based on [DBT3 Benchmark Q20](/kb/en/dbt3-benchmark-queries/#q20) with 60 millions records in the `lineitem` table.

```sql
select sql_calc_found_rows s_name, s_address from 
supplier, nation where 
  s_suppkey in
    (select ps_suppkey from partsupp where
      ps_partkey in (select p_partkey from part where 
         p_name like 'forest%') and 
    ps_availqty > 
      (select 0.5 * sum(l_quantity) from lineitem where
        l_partkey = ps_partkey and l_suppkey = ps_suppkey and
        l_shipdate >= date('1994-01-01') and
        l_shipdate < date('1994-01-01') + interval '1' year ))
  and s_nationkey = n_nationkey
  and n_name = 'CANADA'
  order by s_name
  limit 10;
```

First,

```sql
set optimizer_switch='materialization=off,semijoin=off';
```

```sql
+---+-------- +----------+-------+...+------+----------+------------
| id| sel_type| table    | type  |...| rows | filt | Extra
+---+-------- +----------+-------+...+------+----------+------------
| 1 | PRIMARY | nation   | ALL   |...| 25   |100.00 | Using where;...
| 1 | PRIMARY | supplier | ref   |...| 1447 |100.00 | Using where; Subq
| 2 | DEP SUBQ| partsupp | idxsq |...| 38   |100.00 | Using where
| 4 | DEP SUBQ| lineitem | ref   |...| 3    |100.00 | Using where
| 3 | DEP SUBQ| part     | unqsb |...| 1    |100.00 | Using where
+---+-------- +----------+-------+...+------+----------+------------

10 rows in set
(51.78 sec)
```

Next, a really bad plan, yet one sometimes chosen:

```sql
+---+-------- +----------+-------+...+------+----------+------------
| id| sel_type| table    | type  |...| rows | filt | Extra
+---+-------- +----------+-------+...+------+----------+------------
| 1 | PRIMARY | supplier | ALL   |...|100381|100.00 | Using where; Subq
| 1 | PRIMARY | nation   | ref   |...| 1    |100.00 | Using where
| 2 | DEP SUBQ| partsupp | idxsq |...| 38   |100.00 | Using where
| 4 | DEP SUBQ| lineitem | ref   |...| 3    |100.00 | Using where
| 3 | DEP SUBQ| part     | unqsb |...| 1    |100.00 | Using where
+---+-------- +----------+-------+...+------+----------+------------

10 rows in set
(7 min 33.42 sec)
```

[Persistent statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics) don't improve matters:

```sql
set use_stat_tables='preferably';
+---+-------- +----------+-------+...+------+----------+------------
| id| sel_type| table    | type  |...| rows | filt | Extra
+---+-------- +----------+-------+...+------+----------+------------
| 1 | PRIMARY | supplier | ALL   |...|10000 |100.00 | Using where;
| 1 | PRIMARY | nation   | ref   |...| 1    |100.00 | Using where
| 2 | DEP SUBQ| partsupp | idxsq |...| 80   |100.00 | Using where
| 4 | DEP SUBQ| lineitem | ref   |...| 7    |100.00 | Using where
| 3 | DEP SUBQ| part     | unqsb |...| 1    |100.00 | Using where
+---+-------- +----------+-------+...+------+----------+------------

10 rows in set
(7 min 40.44 sec)
```

The default flags for [optimizer_switch](/kb/en/server-system-variables/#optimizer_switch) do not help much:

```sql
set optimizer_switch='materialization=default,semijoin=default';
+---+-------- +----------+-------+...+------+----------+------------
| id| sel_type| table    | type  |...| rows  | filt  | Extra
+---+-------- +----------+-------+...+------+----------+------------
| 1 | PRIMARY | supplier | ALL   |...|10000  |100.00 | Using where;
| 1 | PRIMARY | nation   | ref   |...| 1     |100.00 | Using where
| 1 | PRIMARY | <subq2>  | eq_ref|...| 1     |100.00 |
| 2 | MATER   | part     | ALL   |.. |2000000|100.00 | Using where
| 2 | MATER   | partsupp | ref   |...| 4     |100.00 | Using where; Subq
| 4 | DEP SUBQ| lineitem | ref   |...| 7     |100.00 | Using where
+---+-------- +----------+-------+...+------+----------+------------

10 rows in set
(5 min 21.44 sec)
```

Using statistics doesn't help either:

```sql
set optimizer_switch='materialization=default,semijoin=default';
set optimizer_use_condition_selectivity=4;

+---+-------- +----------+-------+...+------+----------+------------
| id| sel_type| table    | type  |...| rows  | filt  | Extra
+---+-------- +----------+-------+...+------+----------+------------
| 1 | PRIMARY | nation   | ALL   |...| 25    |4.00   | Using where
| 1 | PRIMARY | supplier | ref   |...| 4000  |100.00 | Using where;
| 1 | PRIMARY | <subq2>  | eq_ref|...| 1     |100.00 |
| 2 | MATER   | part     | ALL   |.. |2000000|1.56   | Using where
| 2 | MATER   | partsupp | ref   |...| 4     |100.00 | Using where; Subq
| 4 | DEP SUBQ| lineitem | ref   |...| 7     | 30.72 | Using where
+---+-------- +----------+-------+...+------+----------+------------

10 rows in set
(5 min 22.41 sec)
```

Now, taking into account the cost of the dependent subquery:

```sql
set optimizer_switch='materialization=default,semijoin=default';
set optimizer_use_condition_selectivity=4;
set optimizer_switch='expensive_pred_static_pushdown=on';
+---+-------- +----------+-------+...+------+----------+------------
| id| sel_type| table    | type  |...| rows | filt  | Extra
+---+-------- +----------+-------+...+------+----------+------------
| 1 | PRIMARY | nation   | ALL   |...| 25   | 4.00  | Using where
| 1 | PRIMARY | supplier | ref   |...| 4000 |100.00 | Using where;
| 2 | PRIMARY | partsupp | ref   |...| 80   |100.00 |
| 2 | PRIMARY | part     | eq_ref|...| 1    | 1.56  | where; Subq; FM
| 4 | DEP SUBQ| lineitem | ref   |...| 7    | 30.72 | Using where
+---+-------- +----------+-------+...+------+----------+------------

10 rows in set
(49.89 sec)
```

Finally, using [join_buffer](/kb/en/server-system-variables/#join_buffer_size) as well:

```sql
set optimizer_switch= 'materialization=default,semijoin=default';
set optimizer_use_condition_selectivity=4;
set optimizer_switch='expensive_pred_static_pushdown=on';
set join_cache_level=6;
set optimizer_switch='mrr=on';
set optimizer_switch='mrr_sort_keys=on';
set join_buffer_size=1024*1024*16;
set join_buffer_space_limit=1024*1024*32;
+---+-------- +----------+-------+...+------+----------+------------
| id| sel_type| table    | type  |...| rows | filt |  Extra
+---+-------- +----------+-------+...+------+----------+------------
| 1 | PRIMARY | nation   | AL  L |...| 25   | 4.00  | Using where
| 1 | PRIMARY | supplier | ref   |...| 4000 |100.00 | where; BKA
| 2 | PRIMARY | partsupp | ref   |...| 80   |100.00 | BKA
| 2 | PRIMARY | part     | eq_ref|...| 1    | 1.56  | where Sq; FM; BKA
| 4 | DEP SUBQ| lineitem | ref   |...| 7    | 30.72 | Using where
+---+-------- +----------+-------+...+------+----------+------------

10 rows in set
(35.71 sec)
```

## See Also

- [DECODE_HISTOGRAM()](/built-in-functions/secondary-functions/information-functions/decode_histogram)
- [Index Statistics](/replication/optimization-and-tuning/optimization-and-indexes/index-statistics)
- [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics)
- [Engine-independent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics)