# Overview of MariaDB Logs

There are many variables in MariaDB that you can use to define what to log and when to log.

This article will give you an overview of the different logs and how to enable/disable logging to these.

Note that storage engines can have their logs too: for example, InnoDB keeps an [Undo Log](/kb/en/undo-log/) and a Redo Log which are used for rollback and crash recovery. However, this page only lists MariaDB server logs.

### [The error log](/mariadb-administration/server-monitoring-logs/error-log/)

- Always enabled
- Usually a file in the data directory, but some distributions may move this to other locations.
- All critical errors are logged here.
- One can get warnings to be logged by setting [log_warnings](/kb/en/server-system-variables/#log_warnings).
- With the [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/) <code class="fixed" style="white-space:pre-wrap">--syslog</code> option one can duplicate the messages to the system's syslog.

### [General query log](/mariadb-administration/server-monitoring-logs/general-query-log/)

- Enabled with [--general-log](/kb/en/server-system-variables/#general_log)
- Logs all queries to a [file or table](/kb/en/server-system-variables/#log_output).
- Useful for debugging or auditing queries.
- The super user can disable logging to it for a connection by setting [SQL_LOG_OFF](/kb/en/server-system-variables/#sql_log_off) to 1.

### [Slow Query log](/mariadb-administration/server-monitoring-logs/slow-query-log/)

- Enabled by starting mysqld with [--slow-query-log](/kb/en/server-system-variables/#slow_query_log)
- Logs all queries to a [file or table](/kb/en/server-system-variables/#log_output).
- Useful to find queries that causes performance problems.
- Logs all queries that takes more than [long_query_time](/kb/en/server-system-variables/#long_query_time) to run.
- One can decide what to log with the options [--log-slow-admin-statements](/kb/en/server-system-variables/#log_slow_admin_statements), [--log-slow-slave-statements](/kb/en/server-system-variables/#log_slow_slave_statements), [log_slow_filter](/kb/en/server-system-variables/#log_slow_filter) or  [log_slow_rate_limit](/kb/en/server-system-variables/#log_slow_rate_limit).
- One can change what is logged by setting [log_slow_verbosity](/kb/en/server-system-variables/#log_slow_verbosity).
- One can disable it globally by setting [global.slow_query_log](/kb/en/server-system-variables/#slow_query_log) to 0
- In 10.1 one can disable it for a connection by setting [local.slow_query_log](/kb/en/server-system-variables/#slow_query_log) to 0.

### [The binary log](/mariadb-administration/server-monitoring-logs/binary-log/overview-of-the-binary-log/)

- Enabled by starting mysqld with [--log-bin](/kb/en/replication-and-binary-log-server-system-variables/#log_bin)
- Used on machines that are, or may become, replication masters.
- Required for point-in-time recovery.
- Binary log files are mainly used by replication and can also be used with [mysqlbinlog](/clients-utilities/mysqlbinlog/) to apply on a backup to get the database up to date.
- One can decide what to log with [--binlog-ignore-db=database_name](/kb/en/mysqld-options/#-binlog-ignore-db) or [--binlog-do-db=database_name](/kb/en/mysqld-options/#-binlog-do-db).
- The super user can disable logging for a connection by [setting SQL_LOG_BIN](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set-sql_log_bin/) to 0. However while this is 0, no changes done in this connection will be replicated to the slaves!
- For examples, see [Using and Maintaining the Binary Log](/mariadb-administration/server-monitoring-logs/binary-log/using-and-maintaining-the-binary-log/).

### Examples

If you know that your next query will be slow and you don't want to log it in the slow query log, do:

```sql
SET LOCAL SLOW_QUERY_LOG=0;
```

If you are a super user running a log batch job that you don't want to have logged (for example mysqldump), do:

```sql
SET LOCAL SQL_LOG_OFF=1, LOCAL SLOW_QUERY_LOG=0;
```

[mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) since [MariaDB 10.1](/kb/en/what-is-mariadb-101/) will add this automatically to your dump file if you run it with the <code class="fixed" style="white-space:pre-wrap">--skip-log-queries</code> option.

## See also

- [MariaDB audit plugin](/kb/en/server_audit-mariadb-audit-plugin/)