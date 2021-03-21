# Conversion of Big IN Predicates Into Subqueries

Starting from [MariaDB 10.3](/kb/en/what-is-mariadb-103/), the optimizer will convert certain big IN predicates into IN subqueries.

That is, an IN predicate in the form

```sql
column [NOT] IN (const1, const2, .... )
```

is converted into an equivalent IN-subquery:

```sql
column [NOT] IN (select ... from temporary_table)
```

which opens new opportunities for the query optimizer.

The conversion happens if the following conditions are met:

- the IN list has more than 1000 elements  (One can control it through the [in_predicate_conversion_threshold](/kb/en/server-system-variables/#in_predicate_conversion_threshold) parameter).
- the [NOT] IN condition is at the top level of the WHERE/ON clause.

## Controlling the Optimization

- The optimization is on by default. [MariaDB 10.3.18](/kb/en/mariadb-10318-release-notes/) (and debug builds prior to that) introduced the [in_predicate_conversion_threshold](/kb/en/server-system-variables/#in_predicate_conversion_threshold) variable. Set to `0` to disable the optimization.

## Links

[https://jira.mariadb.org/browse/MDEV-12176](https://jira.mariadb.org/browse/MDEV-12176)