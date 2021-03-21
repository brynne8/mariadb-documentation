# Subquery Cache

The goal of the subquery cache is to optimize the evaluation of correlated
subqueries by storing results together with correlation parameters in a cache
and avoiding re-execution of the subquery in cases where the result is already
in the cache.

## Administration

The cache is on by default. One can switch it off using the [optimizer_switch](/kb/en/server-system-variables/#optimizer_switch) `subquery_cache` setting, like so:

```sql
SET optimizer_switch='subquery_cache=off';
```

The efficiency of the subquery cache is visible in 2 statistical variables:

- [Subquery_cache_hit](/kb/en/server-status-variables/#subquery_cache_hit) - Global counter for all subquery cache hits.
- [Subquery_cache_miss](/kb/en/server-status-variables/#subquery_cache_miss) - Global counter for all subquery cache misses.

The session variables [tmp_table_size](/kb/en/server-system-variables/#tmp_table_size) and [max_heap_table_size](/kb/en/server-system-variables/#max_heap_table_size)
influence the size of in-memory temporary tables in the table used
for caching. It cannot grow more than the minimum of the above variables values
(see the [Implementation](#implementation) section for details).

## Visibility

Your usage of the cache is visible in `EXTENDED EXPLAIN` output (warnings) as
<code class="fixed" style="white-space:pre-wrap">"&lt;expr_cache&gt;&lt;//list of parameters//&gt;(//cached expression//)"</code>.
For example:

```sql
EXPLAIN EXTENDED SELECT * FROM t1 WHERE a IN (SELECT b FROM t2);
+----+--------------------+-------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type        | table | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+--------------------+-------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | PRIMARY            | t1    | ALL  | NULL          | NULL | NULL    | NULL |    2 |   100.00 | Using where |
|  2 | DEPENDENT SUBQUERY | t2    | ALL  | NULL          | NULL | NULL    | NULL |    2 |   100.00 | Using where |
+----+--------------------+-------+------+---------------+------+---------+------+------+----------+-------------+
2 rows in set, 1 warning (0.00 sec)

SHOW WARNINGS;
+-------+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Level | Code | Message                                                                                                                                                                                                    |
+-------+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Note  | 1003 | SELECT `test`.`t1`.`a` AS `a` from `test`.`t1` WHERE <expr_cache><`test`.`t1`.`a`>(<in_optimizer>(`test`.`t1`.`a`,<exists>(SELECT 1 FROM `test`.`t2` WHERE (<cache>(`test`.`t1`.`a`) = `test`.`t2`.`b`)))) |
+-------+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

In the example above the presence of
<code class="fixed" style="white-space:pre-wrap">"&lt;expr_cache&gt;&lt;`test`.`t1`.`a`&gt;(...)"</code> is how you know you are
using the subquery cache.

## Implementation

Every subquery cache creates a temporary table where the results and all
parameters are stored. It has a unique index over all parameters. First the
cache is created in a [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) table (if doing this is impossible the cache becomes
disabled for that expression). When the table grows up to the minimum of
`tmp_table_size` and `max_heap_table_size`, the hit rate will be checked:

- if the hit rate is really small (&lt;0.2) the cache will be disabled.
- if the hit rate is moderate (&lt;0.7) the table will be cleaned (all records
  deleted) to keep the table in memory
- if the hit rate is high the table will be converted to a disk table
  (for 5.3.0 it can only be converted to a disk table).

```sql
hit rate = hit / (hit + miss)
```

## Performance Impact

Here are some examples that show the performance impact of the subquery cache
(these tests were made on a 2.53 GHz Intel Core 2 Duo MacBook Pro with dbt-3
scale 1 data set).

<table><tbody><tr><th>example</th><th>cache on</th><th>cache off</th><th>gain</th><th>hit</th><th>miss</th><th>hit rate</th></tr>
<tr><th>1</th><td>1.01sec</td><td>1 hour 31 min 43.33sec</td><td>5445x</td><td>149975</td><td>25</td><td>99.98%</td></tr>
<tr><th>2</th><td>0.21sec</td><td>1.41sec</td><td>6.71x</td><td>6285</td><td>220</td><td>96.6%</td></tr>
<tr><th>3</th><td>2.54sec</td><td>2.55sec</td><td>1.00044x</td><td>151</td><td>461</td><td>24.67%</td></tr>
<tr><th>4</th><td>1.87sec</td><td>1.95sec</td><td>0.96x</td><td>0</td><td>23026</td><td>0%</td></tr>
</tbody></table>

### Example 1

Dataset from DBT-3 benchmark, a query to find customers with balance near top in their nation:

```sql
select count(*) from customer 
where 
   c_acctbal > 0.8 * (select max(c_acctbal) 
                      from customer C 
                      where C.c_nationkey=customer.c_nationkey
                      group by c_nationkey);
```

### Example 2

DBT-3 benchmark, Query #17

```sql
select sum(l_extendedprice) / 7.0 as avg_yearly 
from lineitem, part 
where 
  p_partkey = l_partkey and 
  p_brand = 'Brand#42' and p_container = 'JUMBO BAG' and 
  l_quantity < (select 0.2 * avg(l_quantity) from lineitem 
                where l_partkey = p_partkey);
```

### Example 3

DBT-3 benchmark, Query #2

```sql
select
        s_acctbal, s_name, n_name, p_partkey, p_mfgr, s_address, s_phone, s_comment
from
        part, supplier, partsupp, nation, region
where
        p_partkey = ps_partkey and s_suppkey = ps_suppkey and p_size = 33
        and p_type like '%STEEL' and s_nationkey = n_nationkey
        and n_regionkey = r_regionkey and r_name = 'MIDDLE EAST'
        and ps_supplycost = (
                select
                        min(ps_supplycost)
                from
                        partsupp, supplier, nation, region
                where
                        p_partkey = ps_partkey and s_suppkey = ps_suppkey
                        and s_nationkey = n_nationkey and n_regionkey = r_regionkey
                        and r_name = 'MIDDLE EAST'
        )
order by
        s_acctbal desc, n_name, s_name, p_partkey;
```

### Example 4

DBT-3 benchmark, Query #20

```sql
select
        s_name, s_address
from
        supplier, nation
where
        s_suppkey in (
                select
                        distinct (ps_suppkey)
                from
                        partsupp, part
                where
                        ps_partkey=p_partkey
                        and p_name like 'indian%'
                        and ps_availqty > (
                                select
                                        0.5 * sum(l_quantity)
                                from
                                        lineitem
                                where
                                        l_partkey = ps_partkey
                                        and l_suppkey = ps_suppkey
                                        and l_shipdate >= '1995-01-01'
                                        and l_shipdate < date_ADD('1995-01-01',interval 1 year)
                                )
        )
        and s_nationkey = n_nationkey and n_name = 'JAPAN'
order by
        s_name;
```

## See Also

- [Query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache/)
- [http://mysqlmaniac.com/2012/what-about-the-subqueries/](http://mysqlmaniac.com/2012/what-about-the-subqueries/) blog post describing impact of  subquery cache optimization on queries used by DynamicPageList MediaWiki extension
- [http://varokism.blogspot.ru/2013/06/mariadb-subquery-cache-in-real-use-case.html](http://varokism.blogspot.ru/2013/06/mariadb-subquery-cache-in-real-use-case.html) Another use case from the real world
- [What is MariaDB 5.3](/kb/en/what-is-mariadb-53/)