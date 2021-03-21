# HANDLER for MEMORY Tables

This article explains how to use [HANDLER commands](/sql-statements-structure/nosql/handler/handler-commands) efficiently with [MEMORY/HEAP](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine) tables.

If you want to scan a table for over different key values, not just search for exact key values, you should create your keys with 'USING BTREE':

```sql
CREATE TABLE t1 (a INT, b INT, KEY(a), KEY b USING BTREE (b)) engine=memory;
```

In the above table, `a` is a [HASH](/kb/en/storage-engine-index-types/#hash-indexes) key that only supports exact matches (=) while `b` is a [BTREE](/kb/en/storage-engine-index-types/#b-tree-indexes) key that you can use to scan the table in key order, starting from start or from a given key value.

The limitations for HANDLER READ with Memory|HEAP tables are:

## Limitations for HASH keys

- You must use all key parts when searching for a row.
- You can't do a key scan of all values. You can only find all rows with the same key value.
- READ NEXT gives error 1031 if the tables changed since last read.

## Limitations for BTREE keys

- READ NEXT gives error 1031 if the tables changed since last read. This limitation can be lifted in the future.

## Limitations for table scans

- READ NEXT gives error 1031 if the table was truncated since last READ call.

## See also

See also the the limitations listed in [HANDLER commands](/sql-statements-structure/nosql/handler/handler-commands).