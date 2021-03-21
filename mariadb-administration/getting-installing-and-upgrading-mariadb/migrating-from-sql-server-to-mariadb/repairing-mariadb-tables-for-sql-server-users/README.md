# Repairing MariaDB Tables for SQL Server Users

Repairing tables in MariaDB is not similar to repairing tables in SQL Server.

The first thing to understand is that every MariaDB table is handled by a [storage engine](/kb/en/understanding-mariadb-architecture/#storage-engines). Storage engines are plugins that know how to physically read and write a table, so each storage engine allows one to repair tables in different ways. The default storage engine is [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb).

MariaDB provides specific SQL statements to deal with corrupted tables:

- [CHECK TABLE](/sql-statements-structure/sql-statements/table-statements/check-table) checks if a table is corrupted;
- [REPAIR TABLE](/sql-statements-structure/sql-statements/table-statements/repair-table) repairs a table if it is corrupted.

As a general rule, there is no reason why a table that is corrupted on a master should also be corrupted on the slaves. Therefore, `REPAIR` is generally used with the `NO_WRITE_TO_BINLOG` option, to avoid replicating it to the slaves.

## Partitioned Tables

[Partitioned tables](/mariadb-administration/partitioning-tables) are normally split into multiple physical files (one per partition). Even if one of the partitions is corrupted, in most cases other partitions are healthy.

For this reason, `CHECK TABLE` and `REPAIR TABLE` don't work on partitioned tables. Instead, use [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) to check or repair a single partition.

For example:

```sql
ALTER TABLE orders CHECK PARTITION p_2019, p_2020;
ALTER TABLE orders REPAIR PARTITION p_2019, p_2020;
```

## Indexes

Indexes can get corrupted. However, as long as data is not corrupted, indexes can always be dropped and rebuilt with [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table):

```sql
ALTER TABLE customer DROP INDEX idx_email;
ALTER TABLE customer ADD INDEX idx_email (email);
```

## Checking and Repairing Tables

Here we discuss how to repair tables, depending on the storage engine.

### InnoDB

InnoDB follows the "fail fast" philosophy. If table corruption is detected, by default InnoDB deliberately causes MariaDB to crash to avoid corruption propagation, logging an error into the [error log](/mariadb-administration/server-monitoring-logs/error-log). This happens even if the corruption is found with a `CHECK TABLE` statement. This behavior can be changed with the [innodb_corrupt_table_action](/kb/en/innodb-system-variables/#innodb_corrupt_table_action) server variable.

To repair an InnoDB table after a crash:

1 Restart MariaDB with the <a undefined>--innodb-force-recovery</a> option set to a low but non-zero value.
2 If MariaDB fails to start, retry with a higher value. Repeat until you succeed.

At this point, you can follow two different procedures, depending if you can use a backup or not. Provided that you have a usable backup, it is often the best option to bring the database up quickly. But if you want to reduce the data loss as much as possible, you prefer to follow the second method.

Restoring a backup:

1 Drop the whole database with [DROP DATABASE](/sql-statements-structure/sql-statements/data-definition/drop/drop-database).
2 Restore a backup of the database. The exact procedure depends on the [type of backup](/mariadb-administration/getting-installing-and-upgrading-mariadb/migrating-from-sql-server-to-mariadb/mariadb-backups-overview-for-sql-server-users).

Recovering existing data:

1 Dump data from the corrupter table, ordered by primary key. MariaDB could crash when it finds damaged data. Repeat the process skipping damaged data.
2 Save somewhere the table structure with [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table).
3 Restart MariaDB.
4 Drop the table with [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table).
5 Recreate the table and restore the dump.

For more details, see [InnoDB Recovery Modes](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-troubleshooting/innodb-recovery-modes).

### Aria and MyISAM

[MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine) is not crash-safe. In case of a MariaDB crash, the changes applied to MyISAM tables but not yet flushed to the disk are lost.

[Aria](/columns-storage-engines-and-plugins/storage-engines/aria) is crash-safe by default, which means that in case of a crash, after repairing any table that is damaged, no changes are lost. However, Aria tables are not crash-safe if created with `TRANSACTIONAL=0` or `ROW_FORMAT` set to `FIXED` or `DYNAMIC`.

System tables use the Aria storage engine and they are crash-safe.

To check if a MyISAM/Aria table is corrupted, we can use [CHECK TABLE](/sql-statements-structure/sql-statements/table-statements/check-table). To repair a MyISAM/Aria table, one can use [REPAIR TABLE](/sql-statements-structure/sql-statements/table-statements/repair-table). Before running `REPAIR TABLE` against big tables, consider increasing [myisam_repair_threads](/kb/en/myisam-system-variables/#myisam_repair_threads) or [aria_repair_threads](/kb/en/aria-system-variables/#aria_repair_threads).

MyISAM and Aria tables can also be automatically repaired when corruption is detected. This is particularly useful for Aria, in case corrupted system tables prevent MariaDB from starting. See [myisam_recover_options](/kb/en/myisam-system-variables/#myisam_recover_options) and [aria_recover_options](/kb/en/aria-system-variables/#aria_recover_options). By default Aria runs the quickest repair type. Occasionally, to repair a system table, we may have to start MariaDB in this way:

```sql
mysqld --aria-recover-options=BACKUP,FORCE
```

It is also possible to stop MariaDB and repair MyISAM tables with [myisamchk](/clients-utilities/myisam-clients-and-utilities/myisamchk), and Aria tables with [aria_chk](/clients-utilities/aria-clients-and-utilities/aria_chk). With default values, a repair can be unnecessarily very slow. Before running these tools, be sure to check the [Memory and Disk Use With myisamchk](/clients-utilities/myisam-clients-and-utilities/memory-and-disk-use-with-myisamchk) page.

### Other Storage Engines

Notes on the different storage engines:

- For [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks), see [MyRocks and CHECK TABLE](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-and-check-table).
- With [ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive), `REPAIR TABLE` also improves the compression rate.
- For [CSV](/columns-storage-engines-and-plugins/storage-engines/csv), see [Checking and Rpairing CSV Tables](/columns-storage-engines-and-plugins/storage-engines/csv/checking-and-repairing-csv-tables).
- Some special storage engines, like [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine) or [BLACKHOLE](/columns-storage-engines-and-plugins/storage-engines/blackhole), do not support any form of check and repair.