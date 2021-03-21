# Big Query Settings

[MariaDB 5.3](/kb/en/what-is-mariadb-53/) and beyond have a number of features that are targeted at big queries and so are disabled by default.

This page describes recommended settings for IO-bound queries that shovel through lots of records.

First, turn on [Batched Key Access](/replication/optimization-and-tuning/query-optimizations/block-based-join-algorithms):

```sql
# Turn on disk-ordered reads
optimizer_switch='mrr=on'
optimizer_switch='mrr_cost_based=off'

# Turn on Batched Key Access (BKA)
join_cache_level = 6
```

Give BKA buffer space to operate on. 
Ideally, it should have enough space to fit all the data examined by the query.

```sql
# Size limit for the whole join
join_buffer_space_limit = 300M

# Limit for each individual table
join_buffer_size = 100M
```

Turn on [index_merge/sort-intersection](/kb/en/index_merge_sort_intersection/):

```sql
optimizer_switch='index_merge_sort_intersection=on'
```

If your queries examine big fraction of the tables (somewhere more than ~ 30%), turn on [hash join](hash-join):

```sql
# Turn on both Hash Join and Batched Key Access
join_cache_level = 8
```