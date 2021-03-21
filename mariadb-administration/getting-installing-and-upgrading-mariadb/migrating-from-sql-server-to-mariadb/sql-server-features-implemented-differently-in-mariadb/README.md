# SQL Server Features Implemented Differently in MariaDB

Modern DBMSs implement several advanced features. While an SQL standard exists, the complete feature list is different for every database system. Sometimes different features allow achieving the same purpose, but with a different logic and different limitations. This is something to take into account when planning a migration.

Some features are implemented by different DBMSs, with a similar logic and similar syntax. But there could be important differences that users should be aware of.

This page has a list of SQL Server features that MariaDB implements in a different way, and SQL Server features for which MariaDB has an alternative feature. Minor differences are not taken into account here. The list is not exhaustive.

## SQL

- The list of supported [data types](/columns-storage-engines-and-plugins/data-types/) is different.
- There are relevant [differences in transaction isolation levels](/kb/en/mariadb-transactions-and-isolation-levels-for-sql-server-users/#isolation-levels-and-locks).
- `SNAPSHOT` isolation level is not supported. Instead, you can use `START TRANSACTION WITH CONSISTENT SNAPSHOT` to acquire a snapshot at the beginning of the transaction. This is compatible with all isolation levels. See [How Isolation Levels are Implemented in MariaDB](/kb/en/mariadb-transactions-and-isolation-levels-for-sql-server-users/#how-isolation-levels-are-implemented-in-mariadb).
- JSON support is [different](/kb/en/sql-server-and-mariadb-types-comparison/#json).

## Indexes and Performance

- Clustered indexes. In MariaDB, the physical order of rows is delegated to the storage engine. InnoDB uses the primary key as a clustered index.
- Hash indexes. Only some storage engines support `HASH` indexes.
<ul start="1"><li>The [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) storage engine has a feature called adaptive hash index, enabled by default. It means that in InnoDB all indexes are created as `BTREE`, and depending on how they are used, InnoDB could convert them from BTree to hash indexes, or the other way around. This happens in the background.
</li><li>The [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) storage engine uses hash indexes by default, if we don't specify the `BTREE` keyword.
</li><li>See [Storage Engine Index Types](/replication/optimization-and-tuning/optimization-and-indexes/storage-engine-index-types/) for more information.
</li></ul>
- Query store. MariaDB allows query performance analysis using the [slow log](/mariadb-administration/server-monitoring-logs/slow-query-log/) and [performance_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/). Some open source or commercial 3rd party tools read that information to produce statistics and make it easy to identify slow queries.

## Tables

- Computed columns are called [generated columns](/sql-statements-structure/sql-statements/data-definition/create/generated-columns/) in MariaDB and are created with a different syntax. See also [Implementation Differences Compared to Microsoft SQL Server](/kb/en/generated-columns/#implementation-differences-compared-to-microsoft-sql-server).
- [Temporal tables](/kb/en/temporal-data-tables/) use a different (more standard) syntax on MariaDB. In MariaDB, the history is stored in the same table as current data (but optionally in different partitions). MariaDB supports both [SYSTEM_TIME](/kb/en/temporal-data-tables/#creating-a-system-versioned-table) and [APPLICATION_TIME](/kb/en/temporal-data-tables/#application-time-periods).
- Hidden columns are [Invisible columns](/sql-statements-structure/sql-statements/data-definition/create/invisible-columns/) in MariaDB.
- [Temporary tables](/kb/en/create-table/#create-temporary-table) are implemented and used differently.

## High Availability

- `NOT FOR REPLICATION`
<ul start="1"><li>MariaDB supports [replication filters](/replication/standard-replication/replication-filters/) to exclude some tables or databases from replication
</li><li>It is possible to keep a table empty in a slave (or in the master) by using the [BLACKHOLE storage engine](/columns-storage-engines-and-plugins/storage-engines/blackhole/).
</li><li>The master can have columns that are not present in a slave (the other way around is also supported). Before using this feature, carefully read the [Replication When the Master and Slave Have Different Table Definitions](/kb/en/replication-when-the-master-and-slave-have-different-table-definitions/#different-number-or-order-of-columns) page.
</li><li>With MariaDB it's possible to [prevent a trigger from running on slaves](/replication/standard-replication/running-triggers-on-the-slave-for-row-based-events/).
</li><li>It's possible to run [events](/programming-customizing-mariadb/triggers-events/event-scheduler/) without replicating them. The same applies to some administrative statements.
</li><li>MariaDB superusers can run statements without replicating them, by using the [sql_log_bin](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set-sql_log_bin/) system variable.
</li><li>Constraints and triggers cannot be disabled for replication, but it is possible to drop them on the slaves.
</li><li>The `IF EXISTS` syntax allows one to easily create a table on the master that already exists (possibly in a different version) on a slave.
</li></ul>
- pollinginterval option. See [Delayed Replication](/replication/standard-replication/delayed-replication/).

## Security

- The list of [permissions](/kb/en/grant/#privilege-levels) is different.
- Security policies. MariaDB allows one to achieve the same results by assigning permissions on views and stored procedures. However, this is not a common practice and it's more complicated than defining security policies. See [Other Uses of Views](/kb/en/creating-using-views/#other-uses-of-views).
- MariaDB does not support an `OUTPUT` clause. Instead, we can use [DELETE RETURNING](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) and, since [MariaDB 10.5](/kb/en/what-is-mariadb-105/), [INSERT RETURNING](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insertreturning/) and [REPLACE RETURNING](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replacereturning/).

## Other Features

- Linked servers. MariaDB supports storage engines to read from, and write to, remote tables. When using the [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) engine, those tables could be in different DBMSs, including SQL Server.
- Job scheduler: MariaDB uses an [event scheduler](/programming-customizing-mariadb/triggers-events/event-scheduler/) to schedule events instead.

## See Also

- [SQL Server features not available in MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/migrating-from-sql-server-to-mariadb/sql-server-features-not-available-in-mariadb/)