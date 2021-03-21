# Writing Logs Into Tables

By default, all logs are disabled or written into files. The [general query log](/mariadb-administration/server-monitoring-logs/general-query-log/) and the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) can also be written to special tables in the `mysql` database. During the startup, entries will always be written into files.

Note that [EXPLAIN output](/mariadb-administration/server-monitoring-logs/slow-query-log/explain-in-the-slow-query-log/) will only be recorded if the slow query log is written to a file and not to a table.

To write logs into tables, the [log_output](/kb/en/server-system-variables/#log_output) server system variable is used. Allowed values are `FILE`, `TABLE` and `NONE`. It is possible to specify multiple values, separated with commas, to write the logs into both tables and files. `NONE` disables logging and has precedence over the other values.

So, to write logs into tables, one of the following settings can be used:

```sql
SET GLOBAL log_output = 'TABLE';
SET GLOBAL log_output = 'FILE,TABLE';
```

The general log will be written into the [general_log](/kb/en/mysqlgeneral_log-table/) table, and the slow query log will be written into the [slow_log](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlslow_log-table/) table. Only a limited set of operations are supported for those special tables. For example, direct DML statements (like `INSERT`) on those tables will fail with an error similar to the following:

```sql
ERROR 1556 (HY000): You can't use locks with log tables.
```

To flush data to the tables, use [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) instead of [FLUSH LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).

To empty the contents of the log tables, [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table/) can be used.

The log tables use the [CSV](/columns-storage-engines-and-plugins/storage-engines/csv/) storage engine by default. This allows an external program to read the files if needed: normal CSV files are stored in the `mysql` subdirectory, in the data dir. However that engine is slow because it does not support indexes, so you can convert the tables to [MyISAM](/kb/en/myisam/) (but not other storage engines). To do so, first temporarily disable logging:

```sql
SET GLOBAL general_log = 'OFF';
ALTER TABLE mysql.general_log ENGINE = MyISAM;
ALTER TABLE mysql.slow_log ENGINE = MyISAM;
SET GLOBAL general_log = @old_log_state;
```

[CHECK TABLE](/kb/en/sql-commands-check-table/) and [CHECKSUM TABLE](/sql-statements-structure/sql-statements/table-statements/checksum-table/) are supported.

[CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) is supported. [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/), [RENAME TABLE](/sql-statements-structure/sql-statements/data-definition/rename-table/) and [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/) are supported when logging is disabled, but log tables cannot be partitioned.

The contents of the log tables is not logged in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) thus cannot be replicated.