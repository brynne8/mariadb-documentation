# ANALYZE Statement

##### MariaDB starting with [10.1.0](/kb/en/mariadb-1010-release-notes/)

The `ANALYZE statement` was introduced in [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/).

## Description

The `ANALYZE statement` is similar to the `EXPLAIN statement`. `ANALYZE statement` will invoke the optimizer, execute the statement, and then produce `EXPLAIN` output instead of the result set. The `EXPLAIN` output will be annotated with statistics from statement execution.

This lets one check how close the optimizer's estimates about the query plan are to the reality.  `ANALYZE` produces an overview, while the
<a undefined>ANALYZE FORMAT=JSON</a> command provides a more detailed view of the query plan and the query execution.

The syntax is

```sql
ANALYZE explainable_statement;
```

where the statement is any statement for which one can run [EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain/).

## Command Output

Consider an example:

```sql
ANALYZE SELECT * FROM tbl1 
WHERE key1 
  BETWEEN 10 AND 200 AND 
  col1 LIKE 'foo%'\G
```

```sql
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tbl1
         type: range
possible_keys: key1
          key: key1
      key_len: 5
          ref: NULL
         rows: 181
       r_rows: 181
     filtered: 100.00
   r_filtered: 10.50
        Extra: Using index condition; Using where
```

Compared to `EXPLAIN`, `ANALYZE`  produces two extra columns:

- <strong>`r_rows`</strong> is an observation-based counterpart of the <strong>rows</strong> column. It shows how many rows were actually read from the table.
- <strong>`r_filtered`</strong> is an observation-based counterpart of the <strong>filtered</strong> column. It shows which fraction of rows was left after applying the WHERE condition.

## Interpreting the Output

### Joins

Let's consider a more complicated example.

```sql
ANALYZE SELECT *
FROM orders, customer 
WHERE
  customer.c_custkey=orders.o_custkey AND
  customer.c_acctbal < 0 AND
  orders.o_totalprice > 200*1000
```

```sql
+----+-------------+----------+------+---------------+-------------+---------+--------------------+--------+--------+----------+------------+-------------+
| id | select_type | table    | type | possible_keys | key         | key_len | ref                | rows   | r_rows | filtered | r_filtered | Extra       |
+----+-------------+----------+------+---------------+-------------+---------+--------------------+--------+--------+----------+------------+-------------+
|  1 | SIMPLE      | customer | ALL  | PRIMARY,...   | NULL        | NULL    | NULL               | 149095 | 150000 |    18.08 |       9.13 | Using where |
|  1 | SIMPLE      | orders   | ref  | i_o_custkey   | i_o_custkey | 5       | customer.c_custkey |      7 |     10 |   100.00 |      30.03 | Using where |
+----+-------------+----------+------+---------------+-------------+---------+--------------------+--------+--------+----------+------------+-------------+
```

Here, one can see that

- For table customer, <strong>customer.rows=149095,  customer.r_rows=150000</strong>. The estimate for number of rows we will read was fairly precise
- <strong>customer.filtered=18.08, customer.r_filtered=9.13</strong>.  The optimizer somewhat overestimated the number of records that will match selectivity of condition attached to `customer` table (in general, when you have a full scan and r_filtered is less than 15%, it's time to consider adding an appropriate index).
- For table orders,  <strong>orders.rows=7, orders.r_rows=10</strong>.  This means that on average, there are 7 orders for a given c_custkey, but in our case there were 10, which is close to the expectation (when this number is consistently far from the expectation, it may be time to run ANALYZE TABLE, or even edit the table statistics manually to get better query plans).
- <strong>orders.filtered=100, orders.r_filtered=30.03</strong>. The optimizer didn't have any way to estimate which fraction of records will be left after it checks the condition that is attached to table orders (it's orders.o_totalprice &gt; 200*1000). So, it used 100%. In reality, it is 30%. 30% is typically not selective enough to warrant adding new indexes. For joins with many tables, it might be worth to collect and use [column statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics/) for columns in question, this may help the optimizer to pick a better query plan.

### Meaning of NULL in r_rows and r_filtered

Let's modify the previous example slightly

```sql
ANALYZE SELECT * 
FROM orders, customer 
WHERE
  customer.c_custkey=orders.o_custkey AND
  customer.c_acctbal < -0 AND 
  customer.c_comment LIKE '%foo%' AND
  orders.o_totalprice > 200*1000;
```

```sql
+----+-------------+----------+------+---------------+-------------+---------+--------------------+--------+--------+----------+------------+-------------+
| id | select_type | table    | type | possible_keys | key         | key_len | ref                | rows   | r_rows | filtered | r_filtered | Extra       |
+----+-------------+----------+------+---------------+-------------+---------+--------------------+--------+--------+----------+------------+-------------+
|  1 | SIMPLE      | customer | ALL  | PRIMARY,...   | NULL        | NULL    | NULL               | 149095 | 150000 |    18.08 |       0.00 | Using where |
|  1 | SIMPLE      | orders   | ref  | i_o_custkey   | i_o_custkey | 5       | customer.c_custkey |      7 |   NULL |   100.00 |       NULL | Using where |
+----+-------------+----------+------+---------------+-------------+---------+--------------------+--------+--------+----------+------------+-------------+
```

Here, one can see that <strong>orders.r_rows=NULL</strong> and <strong>orders.r_filtered=NULL</strong>. This means that table orders was not scanned even once.  
Indeed, we can also see customer.r_filtered=0.00. This shows that a part of WHERE attached to table `customer` was never satisfied (or, satisfied in less than 0.01% of cases).

## ANALYZE FORMAT=JSON

[ANALYZE FORMAT=JSON](/kb/en/analyze-formatjson/) produces JSON output.  It produces much more information than tabular `ANALYZE`.

## Notes

- `ANALYZE UPDATE` or `ANALYZE DELETE` will actually make updates/deletes (`ANALYZE SELECT` will perform the select operation and then discard the resultset).
- PostgreSQL has a similar command, `EXPLAIN ANALYZE`.
- The [EXPLAIN in the slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/explain-in-the-slow-query-log/) feature allows MariaDB to have `ANALYZE` output of slow queries printed into the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) (see [MDEV-6388](https://jira.mariadb.org/browse/MDEV-6388)).

## See Also

- [ANALYZE FORMAT=JSON](/kb/en/analyze-formatjson/)
- [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/)
- JIRA task for ANALYZE statement, [MDEV-406](https://jira.mariadb.org/browse/MDEV-406)