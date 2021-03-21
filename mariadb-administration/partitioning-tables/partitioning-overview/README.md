# Partitioning Overview

In MariaDB, a table can be split in smaller subsets. Both data and indexes are partitioned.

## Uses for partitioning

There can be several reasons to use this feature:

- Very big tables and indexes can be slow even with optimized queries. But if the target table is partitioned, queries that read a small number of partitions can be much faster.
- Partitioning allows one to distribute files over multiple storage devices. For example, we can have historical data on slower, larger disks (historical data are not supposed to be frequently read); and current data can be on faster disks, or SSD devices.
- In case we separate historical data from recent data, we will probably need to take regular backups of one partition, not the whole table.

### Partitioning for specific storage engines

Some MariaDB [storage engines](/columns-storage-engines-and-plugins/storage-engines) allow more interesting uses for partitioning.

[SPIDER](/columns-storage-engines-and-plugins/storage-engines/spider) allows one to:

- Move partitions of the same table on different servers. In this way, the workload can be distributed on more physical or virtual machines (<em>data sharding</em>).
- All partitions of a SPIDER table can also live on the same machine. In this case there will be a small overhead (SPIDER will use connections to localhost), but queries that read multiple partitions will use parallel threads.

[CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect) allows one to:

- Build a table whose partitions are tables using different storage engines (like InnoDB, MyISAM, or even engines that do not support partitioning).
- Build an indexable, writeable table on several data files. These files can be in different formats.

See also: [Using CONNECT - Partitioning and Sharding](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/using-connect-partitioning-and-sharding)

## Partitioning types

When partitioning a table, the use should decide:

- a <em>partitioning type</em>;
- a <em>partitioning expression</em>.

A partitioning type is the method used by MariaDB to decide how rows are distributed over existing partitions. Choosing the proper partitioning type is important to distribute rows over partitions in an efficient way.

With some partitioning types, a partitioning expression is also required. A partitioning function is an SQL expression returning an integer or temporal value, used to determine which row will contain a given row. The partitioning expression is used for all reads and writes on involving the partitioned table, thus it should be fast.

See [Partioning Types](/mariadb-administration/partitioning-tables/partitioning-types) for a detailed description.

## How to use partitioning

By default, MariaDB permits partitioning. You can determine this by using the [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins) statement, for example:

```sql
SHOW PLUGINS;
...
| Aria                          | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
| FEEDBACK                      | DISABLED | INFORMATION SCHEMA | NULL    | GPL     |
| partition                     | ACTIVE   | STORAGE ENGINE     | NULL    | GPL     |
+-------------------------------+----------+--------------------+---------+---------+
```

If partition is listed as DISABLED:

```sql
| partition                     | DISABLED | STORAGE ENGINE     | NULL    | GPL     |
+-------------------------------+----------+--------------------+---------+---------+
```

MariaDB has either been built without partitioning support, or has been started with the the [--skip-partition](/kb/en/mysqld-options/#-skip-partition) option, or one of its variants:

```sql
--skip-partition
--disable-partition
--partition=OFF
```

and you will not be able to create partitions.

It is possible to create a new partitioned table using [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table).

[ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) allows one to:

- Partition an existing table;
- Remove partitions from a partitioned table (<strong>with all data in the partition</strong>);
- Add/remove partitions, or reorganize them, as long as the partitioning function allows these operations (see below);
- Exchange a partition with a table;
- Perform administrative operations on some or all partitions (analyze, optimize, check, repair).

The [INFORMATION_SCHEMA.PARTITIONS](/kb/en/information-schema-partitions-table/) table contains information about existing partitions.