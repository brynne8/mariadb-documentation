# General Query Log

The general query log is a log of every SQL query received from a client, as well as each client connect and disconnect. Since it's a record of every query received by the server, it can grow large quite quickly.

However, if you only want a record of queries that change data, it might be better to use the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) instead. One important difference is that the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) only logs a query when the transaction is committed by the server, but the general query log logs a query immediately when it is received by the server.

## Enabling the General Query Log

The general query log is disabled by default.

To enable the general query log, set the <a undefined>general_log</a> system variable to `1`. It can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL general_log=1;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
general_log
```

## Configuring the General Query Log Filename

By default, the general query log is written to `${hostname}.log` in the <a undefined>datadir</a> directory. However, this can be changed.

One way to configure the general query log filename is to set the <a undefined>general_log_file</a> system variable. It can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL general_log_file='mariadb.log';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
general_log
general_log_file=mariadb.log
```

If it is a relative path, then the <a undefined>general_log_file</a> is relative to the <a undefined>datadir</a> directory.

However, the <a undefined>general_log_file</a> system variable can also be an absolute path. For example:

```sql
[mariadb]
...
general_log
general_log_file=/var/log/mysql/mariadb.log
```

Another way to configure the general query log filename is to set the <a undefined>log-basename</a> option, which configures MariaDB to use a common prefix for all log files (e.g. general query log, [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/), [error log](/mariadb-administration/server-monitoring-logs/error-log/), [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/), etc.). The general query log filename will be built by adding a `.log` extension to this prefix. This option cannot be set dynamically. It can be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log-basename=mariadb
general_log
```

The <a undefined>log-basename</a> cannot be an absolute path. The log file name is relative to the <a undefined>datadir</a> directory.

## Choosing the General Query Log Output Destination

The general query log can either be written to a file on disk, or it can be written to the [general_log](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgeneral_log-table/) table in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/) database. To choose the general query log output destination, set the <a undefined>log_output</a> system variable.

### Writing the General Query Log to a File

The general query log is output to a file by default. However, it can be explicitly chosen by setting the <a undefined>log_output</a> system variable to `FILE`. It can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL log_output='FILE';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=FILE
general_log
general_log_file=queries.log
```

### Writing the General Query Log to a Table

The general query log can either be written to the [general_log](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgeneral_log-table/) table in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/) database by setting the <a undefined>log_output</a> system variable to `TABLE`. It can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL log_output='TABLE';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=TABLE
general_log
```

Some rows in this table might look like this:

```sql
SELECT * FROM mysql.general_log\G
*************************** 1. row ***************************
  event_time: 2014-11-11 08:40:04.117177
   user_host: root[root] @ localhost []
   thread_id: 74
   server_id: 1
command_type: Query
    argument: SELECT * FROM test.s
*************************** 2. row ***************************
  event_time: 2014-11-11 08:40:10.501131
   user_host: root[root] @ localhost []
   thread_id: 74
   server_id: 1
command_type: Query
    argument: SELECT * FROM mysql.general_log
...
```

See [Writing logs into tables](/mariadb-administration/server-monitoring-logs/writing-logs-into-tables/) for more information.

## Disabling the General Query Log for a Session

A user with the <a undefined>SUPER</a> privilege can disable logging to the general query log for a connection by setting the <a undefined>SQL_LOG_OFF</a> system variable to `1`. For example:

```sql
SET SESSION SQL_LOG_OFF=1;
```

## Disabling the General Query Log for Specific Statements

In [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/) and later, it is possible to disable logging to the general query log for specific types of statements by setting the <a undefined>log_disabled_statements</a> system variable. This option cannot be set dynamically. It can be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=FILE
general_log
general_log_file=queries.log
log_disabled_statements='slave,sp'
```

## Rotating the General Query Log on Unix and Linux

Unix and Linux distributions offer the <a undefined>logrotate</a> utility, which makes it very easy to rotate log files. See [Rotating Logs on Unix and Linux](/mariadb-administration/server-monitoring-logs/rotating-logs-on-unix-and-linux/) for more information on how to use this utility to rotate the general query log.

## See also

- [MariaDB audit plugin](/kb/en/server_audit-mariadb-audit-plugin/)