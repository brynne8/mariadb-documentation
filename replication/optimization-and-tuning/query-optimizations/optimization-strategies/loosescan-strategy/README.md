# LooseScan Strategy

LooseScan is an execution strategy for [Semi-join subqueries](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/semi-join-subquery-optimizations).



## The idea

We will demonstrate the `LooseScan` strategy by example. Suppose, we're looking for countries that have satellites. We can get them using the following query (for the sake of simplicity we ignore satellites that are owned by consortiums of multiple countries):

```sql
select * from Country  
where 
  Country.code in (select country_code from Satellite)
```

Suppose, there is an index on <code class="fixed" style="white-space:pre-wrap">Satellite.country_code</code>. If we use that index, we will get satellites in the order of their owner country:

<img src="/kb/en/loosescan-strategy/+image/loosescan-satellites-ordered-r2" alt="loosescan-satellites-ordered-r2" title="loosescan-satellites-ordered-r2">

The `LooseScan` strategy doesn't really need ordering, what it needs is grouping. In the above figure, satellites are grouped by country. For instance, all satellites owned by Australia come together, without being mixed with satellites of other countries. This makes it easy to select just one satellite from each group, which you can join with its country and get a list of countries without duplicates:

<img src="/kb/en/loosescan-strategy/+image/loosescan-diagram-no-where" alt="loosescan-diagram-no-where" title="loosescan-diagram-no-where">

## LooseScan in action

The `EXPLAIN` output for the above query looks as follows:

```sql
MariaDB [world]> explain select * from Country where Country.code in (select country_code from Satellite);
+----+-------------+-----------+--------+---------------+--------------+---------+------------------------------+------+-------------------------------------+
| id | select_type | table     | type   | possible_keys | key          | key_len | ref                          | rows | Extra                               |
+----+-------------+-----------+--------+---------------+--------------+---------+------------------------------+------+-------------------------------------+
|  1 | PRIMARY     | Satellite | index  | country_code  | country_code | 9       | NULL                         |  932 | Using where; Using index; LooseScan |
|  1 | PRIMARY     | Country   | eq_ref | PRIMARY       | PRIMARY      | 3       | world.Satellite.country_code |    1 | Using index condition               |
+----+-------------+-----------+--------+---------------+--------------+---------+------------------------------+------+-------------------------------------+
```

## Factsheet

- LooseScan avoids the production of duplicate record combinations by putting the subquery table first and using its index to select one record from multiple duplicates
- Hence, in order for LooseScan to be applicable, the subquery should look like:

```sql
expr IN (SELECT tbl.keypart1 FROM tbl ...)
```

or

```sql
expr IN (SELECT tbl.keypart2 FROM tbl WHERE tbl.keypart1=const AND ...)
```

- LooseScan can handle correlated subqueries
- LooseScan can be switched off by setting the <code class="fixed" style="white-space:pre-wrap">loosescan=off</code> flag in the [optimizer_switch](/kb/en/server-system-variables/#optimizer_switch) variable.