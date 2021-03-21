# Concurrent Inserts

The [MyISAM](/kb/en/myisam/) storage engine supports concurrent inserts. This feature allows [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) statements to be executed during [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert) operations, reducing contention.

Whether concurrent inserts can be used or not depends on the value of the [concurrent_insert](/kb/en/server-system-variables/#concurrent_insert) server system variable:

- `NEVER` (0) disables concurrent inserts.
- `AUTO` (1) allows concurrent inserts only when the target table has no free blocks (no data in the middle of the table has been deleted after the last [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table)). This is the default.
- `ALWAYS` (2) always enables concurrent inserts, in which case new rows are added at the end of a table if the table is being used by another thread.

If the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) is used, [CREATE TABLE ... SELECT](/kb/en/create-table/#create-table-select) and [INSERT ... SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select) statements cannot use concurrent inserts. These statements acquire a read lock on the table, so concurrent inserts will need to wait. This way the log can be safely used to restore data.

Concurrent inserts are not used by replicas with the row based [replication](/replication) (see [binary log formats](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats)).

If an [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert) statement contain the [HIGH_PRIORITY](/kb/en/high_priority-and-low_priority-clauses/) clause, concurrent inserts cannot be used. [INSERT ... DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed) is usually unneeded if concurrent inserts are enabled.

[LOAD DATA INFILE](/kb/en/load-data-infile/) uses concurrent inserts if the `CONCURRENT` keyword is specified and  [concurrent_insert](/kb/en/server-system-variables/#concurrent_insert) is not `NEVER`. This makes the statement slower (even if no other sessions access the table) but reduces contention.

[LOCK TABLES](/kb/en/transactions-lock/) allows non-conflicting concurrent inserts if a `READ LOCAL` lock is used. Concurrent inserts are not allowed if the `LOCAL` keyword is omitted.

## Notes

The decision to enable concurrent insert for a table  is done when the table is opened. If you change the value of [concurrent_insert](/kb/en/server-system-variables/#concurrent_insert) it will only affect new opened tables. If you want it to work for also for tables in use or cached, you should do [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) after setting the variable.

## See Also

- [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert)
- [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed)
- [INSERT SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select)
- [HIGH_PRIORITY and LOW_PRIORITY](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/high_priority-and-low_priority)
- [INSERT - Default &amp; Duplicate Values](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-default-duplicate-values)
- [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-ignore)
- [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update)