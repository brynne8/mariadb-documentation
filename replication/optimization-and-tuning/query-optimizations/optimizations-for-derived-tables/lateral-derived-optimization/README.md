# Lateral Derived Optimization

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

Starting from [MariaDB 10.3](/kb/en/what-is-mariadb-103/), MariaDB has Lateral Derived optimization, also referred to as "Split Grouping Optimization" in some sources.

## Description

The optimization's use case is

- The query uses a derived table (or a VIEW, or a non-recursive CTE)
- The derived table/View/CTE has a GROUP BY operation as its top-level operation
- The query only needs data from a few GROUP BY groups

An example of this: consider a VIEW that computes totals for each customer in October:

```sql
create view OCT_TOTALS as
select
  customer_id,
  SUM(amount) as TOTAL_AMT
from orders
where
  order_date BETWEEN '2017-10-01' and '2017-10-31'
group by
  customer_id;
```

And a query that does a join with the customer table to get October totals for "Customer#1" and Customer#2:

```sql
select *
from
  customer, OCT_TOTALS
where
  customer.customer_id=OCT_TOTALS.customer_id and
  customer.customer_name IN ('Customer#1', 'Customer#2')
```

Before Lateral Derived optimization, MariaDB would execute the query as follows:

1 Materialize the view OCT_TOTALS. This essentially computes OCT_TOTALS for all customers.
2 Join it with table customer.

The EXPLAIN would look like so:

```sql
+------+-------------+------------+-------+---------------+-----------+---------+---------------------------+-------+--------------------------+
| id   | select_type | table      | type  | possible_keys | key       | key_len | ref                       | rows  | Extra                    |
+------+-------------+------------+-------+---------------+-----------+---------+---------------------------+-------+--------------------------+
|    1 | PRIMARY     | customer   | range | PRIMARY,name  | name      | 103     | NULL                      | 2     | Using where; Using index |
|    1 | PRIMARY     | <derived2> | ref   | key0          | key0      | 4       | test.customer.customer_id | 36    |                          |
|    2 | DERIVED     | orders     | index | NULL          | o_cust_id | 4       | NULL                      | 36738 | Using where              |
+------+-------------+------------+-------+---------------+-----------+---------+---------------------------+-------+--------------------------+
```

It is obvious that Step #1 is very inefficient: we compute totals for all customers in the database, while we will only need them for two customers. (If there are 1000 customers, we are doing 500x more work than needed here)

Lateral Derived optimization addresses this case. It turns the computation of OCT_TOTALS into what SQL Standard refers to as "LATERAL subquery": a subquery that may have dependencies on the outside tables.
This allows pushing the equality `customer.customer_id=OCT_TOTALS.customer_id` down into the derived table/view, where it can be used to limit the computation to compute totals only for the customer of interest.

The query plan will look as follows:

1 Scan table `customer` and find `customer_id` for Customer#1 and Customer#2.
2 For each customer_id, compute the October totals, for this specific customer.

The EXPLAIN output will look like so:

```sql
+------+-----------------+------------+-------+---------------+-----------+---------+---------------------------+------+--------------------------+
| id   | select_type     | table      | type  | possible_keys | key       | key_len | ref                       | rows | Extra                    |
+------+-----------------+------------+-------+---------------+-----------+---------+---------------------------+------+--------------------------+
|    1 | PRIMARY         | customer   | range | PRIMARY,name  | name      | 103     | NULL                      | 2    | Using where; Using index |
|    1 | PRIMARY         | <derived2> | ref   | key0          | key0      | 4       | test.customer.customer_id | 2    |                          |
|    2 | LATERAL DERIVED | orders     | ref   | o_cust_id     | o_cust_id | 4       | test.customer.customer_id | 1    | Using where              |
+------+-----------------+------------+-------+---------------+-----------+---------+---------------------------+------+--------------------------+
```

Note the line with `id=2`: select_type is `LATERAL DERIVED`.  And table customer uses ref access referring to `customer.customer_id`, which is normally not allowed for derived tables.

## Controlling the Optimization

Lateral Derived is enabled by default, the optimizer will make a cost-based decision whether the optimization should be used.

If you need to disable the optimization, it has an [optimizer_switch](/replication/optimization-and-tuning/query-optimizations/optimizer-switch) flag. It can be disabled like so:

```sql
set optimizer_switch='split_materialized=off'
```

## References

- Jira task: [https://jira.mariadb.org/browse/MDEV-13369](https://jira.mariadb.org/browse/MDEV-13369)
- Commit: [https://github.com/MariaDB/server/commit/b14e2b044b](https://github.com/MariaDB/server/commit/b14e2b044b)