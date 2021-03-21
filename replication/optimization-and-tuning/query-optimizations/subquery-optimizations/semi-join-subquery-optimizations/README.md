# Semi-join Subquery Optimizations

MariaDB has a set of optimizations specifically targeted at <em>semi-join subqueries</em>.

## What is a semi-join subquery

A semi-join subquery has a form of

```sql
SELECT ... FROM outer_tables WHERE expr IN (SELECT ... FROM inner_tables ...) AND ...
```

that is, the subquery is an IN-subquery and it is located in the WHERE clause. The most important part here is

<em>with semi-join subquery, we're only interested in records of outer_tables that have matches in the subquery</em>

Let's see why this is important. Consider a semi-join subquery:

```sql
select * from Country 
where 
  Continent='Europe' and 
  Country.Code in (select City.country 
                   from City 
                   where City.Population>1*1000*1000);
```

One can execute it "naturally", by starting from countries in Europe and checking if they have populous Cities:

<img src="/kb/en/semi-join-subquery-optimizations/+image/semi-join-outer-to-inner" alt="semi-join-outer-to-inner" title="semi-join-outer-to-inner">

The semi-join property also allows "backwards" execution: we can start from big cities, and check which countries they are in:

<img src="/kb/en/semi-join-subquery-optimizations/+image/semi-join-inner-to-outer" alt="semi-join-inner-to-outer" title="semi-join-inner-to-outer">

To contrast, let's change the subquery to be non-semi-join:

```sql
select * from Country 
where 
   Country.Continent='Europe' and 
   (Country.Code in (select City.country 
                   from City where City.Population>1*1000*1000) 
    or Country.SurfaceArea > 100*1000  -- Added this part
   );
```

It is still possible to start from countries, and then check

- if a country has any big cities
- if it has a large surface area:

<img src="/kb/en/semi-join-subquery-optimizations/+image/non-semi-join-subquery" alt="non-semi-join-subquery" title="non-semi-join-subquery">

The opposite, city-to-country way is not possible. This is not a semi-join.

### Difference from inner joins

Semi-join operations are similar to regular relational joins. There is a difference though: with semi-joins, you don't care how many matches an inner table has for an outer row. In the above countries-with-big-cities example, Germany will be returned once, even if it has three cities with populations of more than one million each.

## Semi-join optimizations in MariaDB

MariaDB uses semi-join optimizations to run IN subqueries starting from [MariaDB 5.3](/kb/en/what-is-mariadb-53/). Starting in [MariaDB 5.3.3](/kb/en/mariadb-533-release-notes/), Semi-join subquery optimizations are enabled by default. You can disable them by turning off their [optimizer_switch](/kb/en/server-system-variables/#optimizer_switch) like so:

```sql
SET optimizer_switch='semijoin=off'
```

MariaDB has five different semi-join execution strategies:

- [Table pullout optimization](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/table-pullout-optimization/)
- [FirstMatch execution strategy](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/firstmatch-strategy/)
- [Semi-join Materialization execution strategy](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/semi-join-materialization-strategy/)
- [LooseScan execution strategy](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/loosescan-strategy/)
- [DuplicateWeedout execution strategy](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/duplicateweedout-strategy/)

## See Also

- [What is MariaDB 5.3](/kb/en/what-is-mariadb-53/)
- [Subquery Optimizations Map](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/subquery-optimizations-map/)
- ["Observations about subquery use cases"](http://s.petrunia.net/blog/?p=35) blog post
- [http:<em>en.wikipedia.org/wiki/Semijoin</em>](http://en.wikipedia.org/wiki/Semijoin)