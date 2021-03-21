# FirstMatch Strategy

`FirstMatch` is an execution strategy for [Semi-join subqueries](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/semi-join-subquery-optimizations).

## The idea

It is very similar to how `IN/EXISTS` subqueries were executed in MySQL 5.x.

Let's take the usual example of a search for countries with big cities:

```sql
select * from Country 
where Country.code IN (select City.Country 
                       from City 
                       where City.Population > 1*1000*1000)
      and Country.continent='Europe'
```

Suppose, our execution plan is to find countries in Europe, and then, for each found country, check if it has any big cities. Regular inner join execution will look as follows:

<img src="/kb/en/firstmatch-strategy/+image/firstmatch-inner-join" alt="firstmatch-inner-join" title="firstmatch-inner-join">

Since Germany has two big cities (in this diagram), it will be put into the query output twice. This is not correct, <code class="fixed" style="white-space:pre-wrap">SELECT ... FROM Country</code> should not produce the same country record twice. The `FirstMatch` strategy avoids the production of duplicates by short-cutting execution as soon as the first genuine match is found:

<img src="/kb/en/firstmatch-strategy/+image/firstmatch-firstmatch" alt="firstmatch-firstmatch" title="firstmatch-firstmatch">

Note that the short-cutting has to take place after "Using where" has been applied. It would have been wrong to short-cut after we found Trier.

## FirstMatch in action

The `EXPLAIN` for the above query will look as follows:

```sql
MariaDB [world]> explain select * from Country  where Country.code IN (select City.Country from City where City.Population > 1*1000*1000) and Country.continent='Europe';
+----+-------------+---------+------+--------------------+-----------+---------+--------------------+------+----------------------------------+
| id | select_type | table   | type | possible_keys      | key       | key_len | ref                | rows | Extra                            |
+----+-------------+---------+------+--------------------+-----------+---------+--------------------+------+----------------------------------+
|  1 | PRIMARY     | Country | ref  | PRIMARY,continent  | continent | 17      | const              |   60 | Using index condition            |
|  1 | PRIMARY     | City    | ref  | Population,Country | Country   | 3       | world.Country.Code |   18 | Using where; FirstMatch(Country) |
+----+-------------+---------+------+--------------------+-----------+---------+--------------------+------+----------------------------------+
2 rows in set (0.00 sec)

```

<code class="fixed" style="white-space:pre-wrap">FirstMatch(Country)</code> in the Extra column means that <em>as soon as we have produced one matching record combination, short-cut the execution and jump back to the Country</em> table.

`FirstMatch`'s query plan is very similar to one you would get in MySQL:

```sql
MySQL [world]> explain select * from Country  where Country.code IN (select City.Country from City where City.Population > 1*1000*1000) and Country.continent='Europe';
+----+--------------------+---------+----------------+--------------------+-----------+---------+-------+------+------------------------------------+
| id | select_type        | table   | type           | possible_keys      | key       | key_len | ref   | rows | Extra                              |
+----+--------------------+---------+----------------+--------------------+-----------+---------+-------+------+------------------------------------+
|  1 | PRIMARY            | Country | ref            | continent          | continent | 17      | const |   60 | Using index condition; Using where |
|  2 | DEPENDENT SUBQUERY | City    | index_subquery | Population,Country | Country   | 3       | func  |   18 | Using where                        |
+----+--------------------+---------+----------------+--------------------+-----------+---------+-------+------+------------------------------------+
2 rows in set (0.01 sec)
```

and these two particular query plans will execute in the same time.

## Difference between FirstMatch and IN-&gt;EXISTS

The general idea behind the `FirstMatch` strategy is the same as the one behind the `IN-&gt;EXISTS` transformation, however, `FirstMatch` has several advantages:

- Equality propagation works across semi-join bounds, but not subquery bounds. Therefore, converting a subquery to semi-join and using `FirstMatch` can still give a better execution plan. (TODO example)

- There is only one way to apply the `IN-&gt;EXISTS` strategy and MySQL will do it unconditionally. With `FirstMatch`, the optimizer can make a choice between whether it should run the `FirstMatch` strategy as soon as all tables used in the subquery are in the join prefix, or at some later point in time. (TODO: example)

## FirstMatch factsheet

- The `FirstMatch` strategy works by executing the subquery and short-cutting its execution as soon as the first match is found.
- This means, subquery tables must be after all of the parent select's tables that are referred from the subquery predicate.
- `EXPLAIN` shows `FirstMatch` as "`FirstMatch(tableN)`".
- The strategy can handle correlated subqueries.
- But it cannot be applied if the subquery has meaningful `GROUP BY` and/or aggregate functions.
- Use of the `FirstMatch` strategy is controlled with the <code class="fixed" style="white-space:pre-wrap">firstmatch=on|off</code> flag in the [optimizer_switch](/kb/en/server-system-variables/#optimizer_switch) variable.

## See also

- [Semi-join subquery optimizations](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/semi-join-subquery-optimizations)

In-depth material:

- [WL#3750: initial specification for FirstMatch](http://forge.mysql.com/worklog/task.php?id=3750)