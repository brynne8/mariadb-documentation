# Read-Only Replicas

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

A common [replication](/replication/standard-replication) setup is to have the replicas
[read-only](/kb/en/server-system-variables/#read_only) to ensure that no one
accidentally updates them. If the replica has [binary logging enabled](/replication/standard-replication/setting-up-replication) and [gtid_strict_mode](/kb/en/gtid/#gtid_strict_mode) is used, then any update that causes changes to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) will stop replication.

When the variable `read_only` is set to 1, no updates are permitted except from users with the [SUPER](/kb/en/grant/#super) privilege (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)) or [READ ONLY ADMIN](/kb/en/grant/#read_only-admin) privilege (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)) or replica servers updating from a primary. Inserting rows to log tables, updates to temporary tables and [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table) or [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table) statements are excluded from this limitation.

If read_only is set to 1, then the [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password) statement is limited only to users with the [SUPER](/kb/en/grant/#super) privilege (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)) or [READ ONLY ADMIN](/kb/en/grant/#read_only-admin) privilege (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)).

Attempting to set the `read_only`  variable to 1 will fail if the current session has table locks or transactions pending.

The statement will wait for other sessions that hold table locks. While the attempt to set read_only is waiting, other requests for table locks or transactions will also wait until read_only has been set.

Starting with [MariaDB 10.3.20](/kb/en/mariadb-10320-release-notes/) we have fixed some issues related to read only replicas:

- [CREATE](/sql-statements-structure/sql-statements/data-definition/create/create-table), [DROP](/sql-statements-structure/sql-statements/data-definition/drop/drop-table), [ALTER](/sql-statements-structure/sql-statements/data-definition/alter/alter-table), [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert) and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) of temporary tables are not logged to binary log, even in [statement](/kb/en/binary-log-formats/#statement-based-logging) or [mixed](/kb/en/binary-log-formats/#mixed-logging) mode.  With earlier MariaDB versions, one can avoid the problem with temporary tables by using [binlog_format=ROW](/kb/en/binary-log-formats/#row-based-logging) in which cases temporary tables are never logged.
- Changes to temporary tables created during `read_only` will not be logged even after `read_only` mode is disabled (for example if the slave is promoted to a master).
- The Admin statements [ANALYZE](/sql-statements-structure/sql-statements/table-statements/analyze-table), [CHECK](/sql-statements-structure/sql-statements/table-statements/check-table) and [REPAIR](/sql-statements-structure/sql-statements/table-statements/repair-table) will not be logged to the binary log under read-only.

### Older MariaDB Versions

If you are using an older MariaDB version with read-only replicas and binary logging enabled on the replica, and you need to do some changes but don't want to have them logged to the binary log, the easiest way to avoid the logging is to [disable binary logging](/mariadb-administration/server-monitoring-logs/binary-log/activating-the-binary-log) while running as root during maintenance:

```sql
set sql_log_bin=0;
alter table test engine=rocksdb;
```

The above changes the test table on the slave to rocksdb without registering
the change in the binary log.