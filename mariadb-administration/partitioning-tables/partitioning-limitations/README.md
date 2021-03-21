# Partitioning Limitations with MariaDB

The following limitations apply to partitioning in MariaDB:

- Each table can contain a maximum of 8192 partitions (from [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)). In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and until 10.0.3, the limit was 1024.
- Currently, queries are never parallelized, even when they involve multiple partitions.
- A table can only be partitioned if the storage engine supports partitioning.
- All partitions must use the same storage engine. For a workaround, see [Using CONNECT - Partitioning and Sharding](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/using-connect-partitioning-and-sharding).
- A partitioned table cannot contain, or be referenced by, [foreign keys](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys).
- The [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache) is not aware of partitioning and partition pruning. Modifying a partition will invalidate the entries related to the whole table.
- Updates can run more slowly when binlog_format=ROW and a partitioned table is updated than an equivalent update of a non-partitioned table.
- All columns used in the partitioning expression for a partitioned table must be part of every unique key that the table may have.