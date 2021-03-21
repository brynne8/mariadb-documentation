# Binary Log Formats

## Supported Binary Log Formats

There are three supported formats for [binary log](/mariadb-administration/server-monitoring-logs/binary-log) events:

- Statement-Based Logging
- Row-Based Logging
- Mixed Logging

Regardless of the format, [binary log](/mariadb-administration/server-monitoring-logs/binary-log) events are always stored in a binary format, rather than in plain text. MariaDB includes the [mysqlbinlog](/clients-utilities/mysqlbinlog) utility that can be used to output [binary log](/mariadb-administration/server-monitoring-logs/binary-log) events in a human-readable format.

You may want to set the binary log format in the following cases:

- If you execute single statements that update many rows, then statement-based logging will be more efficient than row-based logging for the slave to download.
- If you execute many statements that don't affect any rows, then row-based logging will be more efficient than statement-based logging for the slave to download.
- If you execute statements that take a long time to complete, but they ultimately only insert, update, or delete a few rows in the table, then row-based logging will be more efficient than statement-based logging for the slave to apply.

The default, since [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/), is [mixed logging](/kb/en/binary-log-formats/#mixed-logging) which is replication-safe and requires less storage space than [row logging](/kb/en/binary-log-formats/#row-based-logging).

The storage engine API also allows storage engines to set or limit the logging format, which helps reduce errors with replicating between masters and slaves with different storage engines.

### Statement-Based Logging

##### MariaDB until [10.2.3](/kb/en/mariadb-1023-release-notes/)

In [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/) and before, statement-based logging is the default. In later versions [mixed logging](/kb/en/binary-log-formats/#mixed-logging) is the default.

When statement-based logging is enabled, statements are logged to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) exactly as they were executed. Temporary tables created on the master will also be created on the slave.
This mode is only recommended where one needs to keep the binary log as small as possible, the master and slave have identical data (including using the same storage engines for all tables), and all functions being used are deterministic (repeatable with the same arguments). 
Statements and tables using timestamps or auto_increment are safe to use with statement-based logging.

This mode can be enabled by setting the [binlog_format](/kb/en/replication-and-binary-log-server-system-variables/#binlog_format) system variable to `STATEMENT`.

In certain cases when it would be impossible to execute the statement on the slave, the server will switch to
row-based logging for the statement. Some cases of this are:

- When replication has been changed from row-based to statement-based and a statement uses data from a temporary table created during row-based mode.  In this case, the temporary tables are not stored on the slave, so row logging is the only alternative.
- [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) of a table using a storage engine that stores data remotely, such as the [S3 storage engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine), to another storage engine.
- One is using [SEQUENCE's](/sql-statements-structure/sequences) in the statement or the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) definition.

In certain cases, a statement may not be deterministic, and therefore not safe for [replication](/replication). If MariaDB determines that an unsafe statement has been executed, then it will issue a warning. For example:

```sql
[Warning] Unsafe statement written to the binary log using statement format since 
  BINLOG_FORMAT = STATEMENT. The statement is unsafe because it uses a LIMIT clause. This 
  is unsafe because the set of rows included cannot be predicted.
```

See [Unsafe Statements for Statement-based Replication](/replication/standard-replication/unsafe-statements-for-statement-based-replication) for more information.

If you need to execute non-deterministic statements, then it is safer to use mixed logging or row-based.

### Mixed Logging

##### MariaDB starting with [10.2.4](/kb/en/mariadb-1024-release-notes/)

In [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/) and later, mixed logging is the default.

When mixed logging is enabled, the server uses a combination of statement-based logging and row-based logging. Statement-based logging is used by default, but when the server determines a statement may not be safe for statement-based logging, it will use row-based logging instead. See [Unsafe Statements for Statement-based Replication: Unsafe Statements](/kb/en/unsafe-statements-for-statement-based-replication/#unsafe-statements) for a list of unsafe statements.

During one transaction, some statements may be logged with row logging while others are logged with statement-based logging.

This mode can be enabled by setting the [[replication-and-binary-log-server-system-variables#binlog_format|binlog_format]# system variable to `MIXED`.

### Row-Based Logging

When row-based logging is enabled, DML statements are <strong>not</strong> logged to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log). Instead, each insert, update, or delete performed by the statement for each row is logged to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) separately. DDL statements are still logged to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).

Row-based logging uses more storage than the other log formats but is the safest to use. In practice [mixed logging](/kb/en/binary-log-formats/#mixed-logging) should be as safe.

If one wants to be able to see the original query that was logged, one can enable [annotated rows events](/kb/en/annotate_rows_event/), that is shown with [mysqlbinlog](/clients-utilities/mysqlbinlog), with [--binlog-annotate-row-events](/kb/en/replication-and-binary-log-server-system-variables/#binlog_annotate_row_events). This option is on by default.

This mode can be enabled by setting the [binlog_format](/kb/en/replication-and-binary-log-server-system-variables/#binlog_format) system variable to `ROW`.

## Compression of the Binary Log

[Compression of the binary log](/replication/standard-replication/compressing-events-to-reduce-size-of-the-binary-log) can be used with any of the binary log formats, but the best results come from using mixed or row-based logging. You can enable compression by using the [--log_bin_compress](/kb/en/replication-and-binary-log-server-system-variables/#log_bin_compress)  startup option.

## Configuring the Binary Log Format

The format for [binary log](/mariadb-administration/server-monitoring-logs/binary-log) events can be configured by setting the [binlog_format](/kb/en/replication-and-binary-log-server-system-variables/#binlog_format) system variable. If you have the [SUPER](/kb/en/grant/#global-privileges) privilege, then you can change it dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL binlog_format='ROW';
```

You can also change it dynamically for just a specific session with [SET SESSION](/kb/en/set/#global-session). For example:

```sql
SET SESSION binlog_format='ROW';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) prior to starting up the server. For example:

```sql
[mariadb]
...
binlog_format=ROW
```

Be careful when changing the binary log format when using [replication](/replication). When you change the binary log format on a server, it only changes the format for that server. Changing the binary log format on a master has no effect on the slave's binary log format. This can cause replication to give inconsistent results or to fail.

Be careful changing the binary log format dynamically when the server is a slave and [parallel replication](/replication/standard-replication/parallel-replication) is enabled. If you change the global value dynamically, then that does not also affect the session values of any currently running threads. This can cause problems with [parallel replication](/replication/standard-replication/parallel-replication), because the [worker threads](/kb/en/replication-threads/#worker-threads) will remain running even after [STOP SLAVE](/kb/en/stop-slave/) is executed. This can be worked around by resetting the [slave_parallel_threads](/kb/en/replication-and-binary-log-server-system-variables/#slave_parallel_threads) system variable. For example:

```sql
STOP SLAVE;
SET GLOBAL slave_parallel_threads=0;
SET GLOBAL binlog_format='ROW';
SET GLOBAL slave_parallel_threads=4;
START SLAVE
```

## Effect of the Binary Log Format on Slaves

In [MariaDB 10.0.22](/kb/en/mariadb-10022-release-notes/) and later, a slave will apply any events it gets from the master, regardless of the binary log format. The [binlog_format](/kb/en/replication-and-binary-log-server-system-variables/#binlog_format) system variable only applies to normal (not replicated) updates.

If you are running MySQL or an older MariaDB than 10.0.22, you should be aware of that if you are running the slave in `binlog_format=STATEMENT` mode, the slave will stop if the master is used with `binlog_format` set to anything else than `STATEMENT`.

The binary log format is upwards-compatible. This means replication should always work if the slave is the same or a newer version of MariaDB than the master.

## The mysql Database

Statements that affect the `mysql` database can be logged in a different way to that expected.

If the mysql database is edited directly, logging is performed as expected according to the [binlog_format](/kb/en/replication-and-binary-log-server-system-variables/#binlog_format). Statements that directly edit the mysql database include [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update), [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete), [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace), [DO](/sql-statements-structure/sql-statements/stored-routine-statements/do), [LOAD DATA INFILE](/kb/en/load-data-infile/), [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select), and [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table).

If the `mysql` database is edited indirectly, statement logging is used regardless of [binlog_format](/kb/en/replication-and-binary-log-server-system-variables/#binlog_format) setting. Statements editing the `mysql` database indirectly include [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant), [REVOKE](/sql-statements-structure/sql-statements/account-management-sql-commands/revoke), [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password), [RENAME USER](/sql-statements-structure/sql-statements/account-management-sql-commands/rename-user), [ALTER](/sql-statements-structure/sql-statements/data-definition/alter), [DROP](/sql-statements-structure/sql-statements/data-definition/drop) and [CREATE](/sql-statements-structure/sql-statements/data-definition/create) (except for the situation described below).

CREATE TABLE ... SELECT can use a combination of logging formats. The [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) portion of the statement is logged using statement-based logging, while the [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) portion is logged according to the value of `binlog_format`.

## See Also

- [Setting up replication](/replication/standard-replication/setting-up-replication)
- [Compressing the binary log](/replication/standard-replication/compressing-events-to-reduce-size-of-the-binary-log)