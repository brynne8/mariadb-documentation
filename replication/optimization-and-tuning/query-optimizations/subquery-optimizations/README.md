# Subquery Optimizations

Articles about subquery optimizations in MariaDB.

- [Subquery Optimizations Map](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/subquery-optimizations-map/) — Map showing types of subqueries and the optimizer strategies available to handle them
- [Semi-join Subquery Optimizations](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/semi-join-subquery-optimizations/) — MariaDB has a set of optimizations specifically targeted at semi-join subqueries
- [Table Pullout Optimization](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/table-pullout-optimization/) — Table pullout is an optimization for Semi-join subqueries
- [Non-semi-join Subquery Optimizations](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/non-semi-join-subquery-optimizations/) — Alternative strategies for IN-subqueries that cannot be flattened into semi-joins
- [Subquery Cache](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/subquery-cache/) — Subquery cache for optimizing the evaluation of correlated subqueries.
- [Condition Pushdown Into IN subqueries](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/condition-pushdown-into-in-subqueries/) — This article describes Condition Pushdown into IN subqueries as implemented...
- [Conversion of Big IN Predicates Into Subqueries](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/conversion-of-big-in-predicates-into-subqueries/) — The optimizer will convert big IN predicates into subqueries.
- [EXISTS-to-IN Optimization](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/exists-to-in-optimization/) — MariaDB 5.3 introduced a rich set of optimizations for IN subqueries
- [Optimizing GROUP BY and DISTINCT Clauses in Subqueries](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/optimizing-group-by/) — MariaDB removes DISTINCT and GROUP BY without HAVING in certain cases