# Subquery Optimizations Map

Below is a map showing all types of subqueries allowed in the SQL language, and
the optimizer strategies available to handle them.

- Uncolored areas represent different kinds of subqueries, for example:
<ul start="1"><li>Subqueries that have form <code class="fixed" style="white-space:pre-wrap">x IN (SELECT ...)</code>
</li><li>Subqueries that are in the <code class="fixed" style="white-space:pre-wrap">FROM</code> clause
</li><li>.. and so forth
</li></ul>
- The size of each uncolored area roughly corresponds to how important (i.e.
  frequently used) that kind of subquery is. For
  example, <code class="fixed" style="white-space:pre-wrap">x IN (SELECT ...)</code> queries are the most important,
  and <code class="fixed" style="white-space:pre-wrap">EXISTS (SELECT ...)</code> are relatively unimportant.
- Colored areas represent optimizations/execution strategies that are applied
  to handle various kinds of subqueries.
- The color of optimization indicates which version of MySQL/MariaDB it was
  available in (see legend below)

<img src="/kb/en/subquery-optimizations-map/+image/subquery-map-2013" alt="subquery-map-2013" title="subquery-map-2013">

Some things are not on the map:

- MariaDB doesn't evaluate expensive subqueries when doing optimization 
  (this means, EXPLAIN is always fast). MySQL 5.6 has made a progress in this regard but its optimizer will still evaluate certain kinds of subqueries (for example, scalar-context subqueries used in range predicates)

## Links to pages about individual optimizations:

- [IN-&gt;EXISTS](/kb/en/non-semi-join-subquery-optimizations/#the-in-to-exists-transformation)
- [Subquery Caching](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/subquery-cache/)

- [Semi-join optimizations](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/semi-join-subquery-optimizations/)
<ul start="1"><li>[Table pullout](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/table-pullout-optimization/)
</li><li>[FirstMatch](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/firstmatch-strategy/)
</li><li>[Materialization, +scan, +lookup](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/semi-join-materialization-strategy/)
</li><li>[LooseScan](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/loosescan-strategy/)
</li><li>[DuplicateWeedout execution strategy](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/duplicateweedout-strategy/)
</li></ul>

- Non-semi-join [Materialization](/kb/en/non-semi-join-subquery-optimizations/#materialization-for-non-correlated-in-subqueries) (including NULL-aware and partial matching)

- Derived table optimizations
<ul start="1"><li>[Derived table merge](/replication/optimization-and-tuning/query-optimizations/optimizations-for-derived-tables/derived-table-merge-optimization/)
</li><li>[Derived table with keys](/replication/optimization-and-tuning/query-optimizations/optimizations-for-derived-tables/derived-table-with-key-optimization/)
</li></ul>

## See also

- [Subquery optimizations in MariaDB 5.3](/kb/en/what-is-mariadb-53/#subquery-optimizations)