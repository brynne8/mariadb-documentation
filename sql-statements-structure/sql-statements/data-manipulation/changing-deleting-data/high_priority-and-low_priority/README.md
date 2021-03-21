# HIGH_PRIORITY and LOW_PRIORITY

The [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) storage engine uses row-level locking to ensure data integrity. However some storage engines (such as [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine), [MyISAM](/kb/en/myisam/), [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) and [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge)) lock the whole table to prevent conflicts. These storage engines use two separate queues to remember pending statements; one is for [SELECTs](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) and the other one is for write statements ([INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert), [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update)). By default, the latter has a higher priority.

To give write operations a lower priority, the [low_priority_updates](/kb/en/server-system-variables/#low_priority_updates) server system variable can be set to `ON`. The option is available on both the global and session levels, and it can be set at startup or via the [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set) statement.

When too many table locks have been set by write statements, some pending SELECTs are executed. The maximum number of write locks that can be acquired before this happens is determined by the [max_write_lock_count](/kb/en/server-system-variables/#max_write_lock_count) server system variable, which is dynamic.

If write statements have a higher priority (default), the priority of individual write statements ([INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert), [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update), [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete)) can be changed via the `LOW_PRIORITY` attribute, and the priority of a `SELECT` statement can be raised via the `HIGH_PRIORITY` attribute. Also, [LOCK TABLES](/kb/en/transactions-lock/) supports a `LOW_PRIORITY` attribute for `WRITE` locks.

If read statements have a higher priority, the priority of an `INSERT` can be changed via the `HIGH_PRIORITY` attribute. However, the priority of other write statements cannot be raised individually.

The use of `LOW_PRIORITY` or `HIGH_PRIORITY` for an `INSERT` prevents [Concurrent Inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts) from being used.

## See Also

- [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert)
- [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed)
- [INSERT SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select)
- [Concurrent Inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts)
- [INSERT - Default &amp; Duplicate Values](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-default-duplicate-values)
- [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-ignore)
- [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update)