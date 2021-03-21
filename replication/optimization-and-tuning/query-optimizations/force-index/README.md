# FORCE INDEX

## Description

Forcing an index to be used is mostly useful when the optimizer decides to do a table scan even if you know that using an index would be better. (The optimizer could decide to do a table scan even if there is an available index when it believes that most or all rows will match and it can avoid the overhead of using the index).

FORCE_INDEX works by only considering the given indexes (like with USE_INDEX) but in addition it tells the optimizer to regard a table scan as something very expensive. However if none of the 'forced' indexes can be used, then a table scan will be used anyway.

## Example

```sql
CREATE INDEX Name ON City (Name);
EXPLAIN SELECT Name,CountryCode FROM City FORCE INDEX (Name)
WHERE name>="A" and CountryCode >="A";
```

This produces:

```sql
id	select_type	table	type	possible_keys	key	key_len	ref	rows	Extra
1	SIMPLE	City	range	Name	Name	35	NULL	4079	Using where
```

### Index Prefixes

When using index hints (USE, FORCE or IGNORE INDEX), the index name value can also be an unambiguous prefix of an index name.

## See Also

- [Index Hints: How to Force Query Plans](/replication/optimization-and-tuning/query-optimizations/index-hints-how-to-force-query-plans/) for more details
- [USE INDEX](/replication/optimization-and-tuning/query-optimizations/use-index/)
- [IGNORE INDEX](/replication/optimization-and-tuning/query-optimizations/ignore-index/)