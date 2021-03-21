# IGNORE INDEX

## Syntax

```sql
IGNORE INDEX [{FOR {JOIN|ORDER BY|GROUP BY}] ([index_list])
```

## Description

You can tell the optimizer to not consider a particular index with the IGNORE INDEX option.

The benefit of using IGNORE_INDEX instead of [USE_INDEX](/replication/optimization-and-tuning/query-optimizations/use-index/) is that it will not disable a new index which you may add later.

### Index Prefixes

When using index hints (USE, FORCE or IGNORE INDEX), the index name value can also be an unambiguous prefix of an index name.

## Example

This is used after the table name in the FROM clause:

```sql
CREATE INDEX Name ON City (Name);
CREATE INDEX CountryCode ON City (Countrycode);
EXPLAIN SELECT Name FROM City IGNORE INDEX (Name)
WHERE name="Helsingborg" AND countrycode="SWE";
```

This produces:

```sql
id	select_type	table	type	possible_keys	key	key_len	ref	rows	Extra
1	SIMPLE	City	ref	CountryCode	CountryCode	3	const	14	Using where
```

## See Also

- See [Index Hints: How to Force Query Plans](/replication/optimization-and-tuning/query-optimizations/index-hints-how-to-force-query-plans/) for more details
- [USE INDEX](/replication/optimization-and-tuning/query-optimizations/use-index/)
- [FORCE INDEX](/replication/optimization-and-tuning/query-optimizations/force-index/)