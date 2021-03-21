# Table Pullout Optimization

Table pullout is an optimization for [Semi-join subqueries](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/semi-join-subquery-optimizations).

## The idea of Table Pullout

Sometimes, a subquery can be re-written as a join. For example:

```sql
select *
from City 
where City.Country in (select Country.Code
                       from Country 
                       where Country.Population < 100*1000);
```

If we know that there can be, at most, one country with with a given value of `Country.Code` (we can tell that if we see that table Country has a primary key or unique index over that column), we can re-write this query as:

```sql
select City.* 
from 
  City, Country 
where
 City.Country=Country.Code AND Country.Population < 100*1000;
```

## Table pullout in action

If one runs [EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain) for the above query in MySQL 5.1-5.6 or [MariaDB 5.1](/kb/en/what-is-mariadb-51/)-5.2, they'll get this plan:

```sql
MySQL [world]> explain select * from City where City.Country in (select Country.Code from Country where Country.Population < 100*1000);
+----+--------------------+---------+-----------------+--------------------+---------+---------+------+------+-------------+
| id | select_type        | table   | type            | possible_keys      | key     | key_len | ref  | rows | Extra       |
+----+--------------------+---------+-----------------+--------------------+---------+---------+------+------+-------------+
|  1 | PRIMARY            | City    | ALL             | NULL               | NULL    | NULL    | NULL | 4079 | Using where |
|  2 | DEPENDENT SUBQUERY | Country | unique_subquery | PRIMARY,Population | PRIMARY | 3       | func |    1 | Using where |
+----+--------------------+---------+-----------------+--------------------+---------+---------+------+------+-------------+
2 rows in set (0.00 sec)
```

It shows that the optimizer is going to do a full scan on table `City`, and for each city it will do a lookup in table `Country`.

If one runs the same query in [MariaDB 5.3](/kb/en/what-is-mariadb-53/), they will get this plan:

```sql
MariaDB [world]> explain select * from City where City.Country in (select Country.Code from Country where Country.Population < 100*1000);
+----+-------------+---------+-------+--------------------+------------+---------+--------------------+------+-----------------------+
| id | select_type | table   | type  | possible_keys      | key        | key_len | ref                | rows | Extra                 |
+----+-------------+---------+-------+--------------------+------------+---------+--------------------+------+-----------------------+
|  1 | PRIMARY     | Country | range | PRIMARY,Population | Population | 4       | NULL               |   37 | Using index condition |
|  1 | PRIMARY     | City    | ref   | Country            | Country    | 3       | world.Country.Code |   18 |                       |
+----+-------------+---------+-------+--------------------+------------+---------+--------------------+------+-----------------------+
2 rows in set (0.00 sec)
```

The interesting parts are:

- Both tables have <code class="fixed" style="white-space:pre-wrap">select_type=PRIMARY</code>, and <code class="fixed" style="white-space:pre-wrap">id=1</code> as if they were in one join.
- The `Country` table is first, followed by the `City` table.

Indeed, if one runs EXPLAIN EXTENDED; SHOW WARNINGS, they will see that the subquery is gone and it was replaced with a join:

```sql
MariaDB [world]> show warnings\G
*************************** 1. row ***************************
  Level: Note
   Code: 1003
Message: select `world`.`City`.`ID` AS `ID`,`world`.`City`.`Name` AS 
`Name`,`world`.`City`.`Country` AS `Country`,`world`.`City`.`Population` AS 
`Population` 

  
   from `world`.`City` join `world`.`Country` where 


((`world`.`City`.`Country` = `world`.`Country`.`Code`) and (`world`.`Country`.
`Population` < (100 * 1000)))
1 row in set (0.00 sec)
```

Changing the subquery into a join allows feeding the join to the join optimizer, which can make a choice between two possible join orders:

1 City -&gt; Country
2 Country -&gt; City

as opposed to the single choice of

1 City-&gt;Country

which we had before the optimization.

In the above example, the choice produces a better query plan. Without pullout, the query plan with a subquery would read `(4079 + 1*4079)=8158` table records.  With table pullout, the join plan would read `(37 + 37 * 18) = 703` rows.  Not all row reads are equal, but generally, reading `10` times fewer table records is faster.

## Table pullout fact sheet

- Table pullout is possible only in semi-join subqueries.
- Table pullout is based on `UNIQUE`/`PRIMARY` key definitions.
- Doing table pullout does not cut off any possible query plans, so MariaDB will always try to pull out as much as possible.
- Table pullout is able to pull individual tables out of subqueries to their parent selects. If all tables in a subquery have been pulled out, the subquery (i.e. its semi-join) is removed completely.
- One common bit of advice for optimizing MySQL has been "If possible, rewrite your subqueries as joins". Table pullout does exactly that, so manual rewrites are no longer necessary.

## Controlling table pullout

There is no separate @@optimizer_switch flag for table pullout. Table pullout can be disabled by switching off all semi-join optimizations with 
<code class="fixed" style="white-space:pre-wrap">SET @@optimizer_switch='semijoin=off'</code> command.