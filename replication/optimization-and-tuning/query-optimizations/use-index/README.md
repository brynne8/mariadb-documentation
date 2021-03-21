# USE INDEX

You can limit which indexes are considered with the `USE INDEX` option.

## Syntax

```sql
USE INDEX [{FOR {JOIN|ORDER BY|GROUP BY}] ([index_list])
```

## Description

The default is 'FOR JOIN', which means that the hint only affects how the WHERE clause is optimized.

USE INDEX is used after the table name in the FROM clause.

### Index Prefixes

When using index hints (USE, FORCE or IGNORE INDEX), the index name value can also be an unambiguous prefix of an index name.

## Example

```sql
CREATE INDEX Name ON City (Name);
CREATE INDEX CountryCode ON City (Countrycode);
EXPLAIN SELECT Name FROM City USE INDEX (CountryCode)
WHERE name="Helsingborg" AND countrycode="SWE";
```

This produces:

```sql
id	select_type	table	type	possible_keys	key	key_len	ref	rows	Extra
1	SIMPLE	City	ref	CountryCode	CountryCode	3	const	14	Using where
```

If we had not used USE INDEX, the Name index would have been in possible keys.

## See Also

- [Index Hints: How to Force Query Plans](/replication/optimization-and-tuning/query-optimizations/index-hints-how-to-force-query-plans) for more details
- [IGNORE INDEX](/replication/optimization-and-tuning/query-optimizations/ignore-index)
- [FORCE INDEX](/replication/optimization-and-tuning/query-optimizations/force-index)