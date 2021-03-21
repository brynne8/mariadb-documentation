# Slow Query Log Overview

The slow query log is a record of SQL queries that took a long time to perform.

Note that, if your queries contain user's passwords, the slow query log may contain passwords too. Thus, it should be protected.

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

The number of rows affected by the slow query have been recorded in the slow query log since [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/).

## Enabling the Slow Query Log

The slow query log is disabled by default.

To enable the slow query log, set the [slow_query_log](/kb/en/server-system-variables/#slow_query_log) system variable to `1`. It can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL slow_query_log=1;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
slow_query_log
```

## Configuring the Slow Query Log Filename

By default, the slow query log is written to `${hostname}-slow.log` in the [datadir](/kb/en/server-system-variables/#datadir) directory. However, this can be changed.

One way to configure the slow query log filename is to set the [slow_query_log_file](/kb/en/server-system-variables/#slow_query_log_file) system variable. It can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL slow_query_log_file='mariadb-slow.log';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
slow_query_log
slow_query_log_file=mariadb-slow.log
```

If it is a relative path, then the [slow_query_log_file](/kb/en/server-system-variables/#slow_query_log_file) is relative to the [datadir](/kb/en/server-system-variables/#datadir) directory.

However, the [slow_query_log_file](/kb/en/server-system-variables/#slow_query_log_file) system variable can also be an absolute path. For example:

```sql
[mariadb]
...
slow_query_log
slow_query_log_file=/var/log/mysql/mariadb-slow.log
```

Another way to configure the slow query log filename is to set the [log-basename](/kb/en/mysqld-options/#-log-basename) option, which configures MariaDB to use a common prefix for all log files (e.g. slow query log, [general query log](/mariadb-administration/server-monitoring-logs/general-query-log/), [error log](/mariadb-administration/server-monitoring-logs/error-log/), [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/), etc.). The slow query log filename will be built by adding `-slow.log` to this prefix. This option cannot be set dynamically. It can be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log-basename=mariadb
slow_query_log
```

The [log-basename](/kb/en/mysqld-options/#-log-basename) cannot be an absolute path. The log file name is relative to the [datadir](/kb/en/server-system-variables/#datadir) directory.

## Choosing the Slow Query Log Output Destination

The slow query log can either be written to a file on disk, or it can be written to the [slow_log](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlslow_log-table/) table in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/) database. To choose the slow query log output destination, set the [log_output](/kb/en/server-system-variables/#log_output) system variable.

### Writing the Slow Query Log to a File

The slow query log is output to a file by default. However, it can be explicitly chosen by setting the [log_output](/kb/en/server-system-variables/#log_output) system variable to `FILE`. It can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL log_output='FILE';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=FILE
slow_query_log
slow_query_log_file=slow-queries.log
```

### Writing the Slow Query Log to a Table

The slow query log can either be written to the [slow_log](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlslow_log-table/) table in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/) database by setting the [log_output](/kb/en/server-system-variables/#log_output) system variable to `TABLE`. It can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL log_output='TABLE';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=TABLE
slow_query_log
```

Some rows in this table might look like this:

```sql
SELECT * FROM mysql.slow_log\G
...
*************************** 2. row ***************************
    start_time: 2014-11-11 07:56:28.721519
     user_host: root[root] @ localhost []
    query_time: 00:00:12.000215
     lock_time: 00:00:00.000000
     rows_sent: 1
 rows_examined: 0
            db: test
last_insert_id: 0
     insert_id: 0
     server_id: 1
      sql_text: SELECT SLEEP(12)
     thread_id: 74
...
```

See [Writing logs into tables](/mariadb-administration/server-monitoring-logs/writing-logs-into-tables/) for more information.

## Disabling the Slow Query Log for a Session

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, a user can disable logging to the slow query log for a connection by setting the [slow_query_log](/kb/en/server-system-variables/#slow_query_log) system variable to `0`. For example:

```sql
SET SESSION slow_query_log=0;
```

## Disabling the Slow Query Log for Specific Statements

In [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/) and later, it is possible to disable logging to the slow query log for specific types of statements by setting the [log_slow_disabled_statements](/kb/en/server-system-variables/#log_slow_disabled_statements) system variable. This option cannot be set dynamically. It can be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=FILE
general_log
general_log_file=queries.log
log_slow_disabled_statements='admin,call,slave,sp'
```

## Configuring the Slow Query Log Time

The time that defines a slow query can be configured by setting the [long_query_time](/kb/en/server-system-variables/#long_query_time) system variable. It uses a units of seconds, with an optional milliseconds component. The default value is `10`. It can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL long_query_time=5.0;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=FILE
slow_query_log
slow_query_log_file=slow-queries.log
long_query_time=5.0
```

## Logging Queries That Don't Use Indexes

It can be beneficial to log queries that don't use indexes to the slow query log, since queries that don't use indexes can usually be optimized either by adding an index or by doing a slight rewrite. The slow query log can be configured to log queries that don't use indexes regardless of their execution time by setting the [log_queries_not_using_indexes](/kb/en/server-system-variables/#log_queries_not_using_indexes) system variable.  It can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL log_queries_not_using_indexes=ON;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=FILE
slow_query_log
slow_query_log_file=slow-queries.log
long_query_time=5.0
log_queries_not_using_indexes=ON
```

As a significant number of queries can run quickly even without indexes, you can use the [min_examined_row_limit](/kb/en/server-system-variables/#min_examined_row_limit) system variable with [log_queries_not_using_indexes](/kb/en/server-system-variables/#log_queries_not_using_indexes) to limit the logged queries to those having a material impact on the server.

## Logging Queries That Examine a Minimum Row Limit

It can be beneficial to log queries that examine a minimum number of rows. The slow query log can be configured to log queries that examine a minimum number of rows regardless of their execution time by setting the [min_examined_row_limit](/kb/en/server-system-variables/#min_examined_row_limit) system variable. It can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL min_examined_row_limit=100000;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=FILE
slow_query_log
slow_query_log_file=slow-queries.log
long_query_time=5.0
min_examined_row_limit=100000
```

## Logging Slow Administrative Statements

By default, the Slow Query Log only logs slow non-administrative statements.  To log administrative statements, set the [log_slow_admin_statements](/kb/en/server-system-variables/#log_slow_admin_statements) system variable.  The Slow Query Log considers the following statements administrative: [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/), [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/), [CHECK TABLE](/kb/en/sql-commands-check-table/), [CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index/), [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index/), [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/), and [REPAIR TABLE](/sql-statements-structure/sql-statements/table-statements/repair-table/).  In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and later, this also includes [ALTER SEQUENCE](/sql-statements-structure/sequences/alter-sequence/) statements.

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, you can dynamically enable this feature using a [SET GLOBAL](/kb/en/set/#global-session) statement.  For example:

```sql
SET GLOBAL log_slow_admin_statements=ON;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=FILE
slow_query_log
slow_query_log_file=slow-queries.log
long_query_time=5.0
log_slow_admin_statements=ON
```

## Enabling the Slow Query Log for Specific Criteria

It is possible to enable logging to the slow query log for queries that meet specific criteria by configuring the [log_slow_filter](/kb/en/server-system-variables/#log_slow_filter) system variable. It can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL log_slow_filter='filesort,filesort_on_disk,tmp_table,tmp_table_on_disk';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=FILE
slow_query_log
slow_query_log_file=slow-queries.log
long_query_time=5.0
log_slow_filter=filesort,filesort_on_disk,tmp_table,tmp_table_on_disk
```

## Throttling the Slow Query Log

The slow query log can create a lot of I/O, so it can be beneficial to throttle it in some cases. The slow query log can be throttled by configuring the [log_slow_rate_limit](/kb/en/server-system-variables/#log_slow_rate_limit) system variable. It can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL log_slow_rate_limit=5;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=FILE
slow_query_log
slow_query_log_file=slow-queries.log
long_query_time=5.0
log_slow_rate_limit=5
```

## Configuring the Slow Query Log Verbosity

There are a few optional pieces of information that can be included in the slow query log for each query. This optional information can be included by configuring the [log_slow_verbosity](/kb/en/server-system-variables/#log_slow_verbosity) system variable. It can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL log_slow_verbosity='query_plan,explain';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
log_output=FILE
slow_query_log
slow_query_log_file=slow-queries.log
long_query_time=5.0
log_slow_verbosity=query_plan,explain
```

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

Starting from [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/), it is possible to have [EXPLAIN output printed in the slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/explain-in-the-slow-query-log/).

## Viewing the Slow Query Log

Slow query logs written to file can be viewed with any text editor, or you can use the [mysqldumpslow](/clients-utilities/mysqldumpslow/) tool to ease the process by summarizing the information.

Queries that you find in the log are key queries to try to optimize by constructing a [more efficient query](/replication/optimization-and-tuning/query-optimizations/) or by making [better use of indexes](/replication/optimization-and-tuning/optimization-and-indexes/).

For queries that appear in the log that cannot be optimized in the above ways, perhaps because they are simply very large selects, due to slow hardware, or very high lock/cpu/io contention, using shard/clustering/load balancing solutions, better hardware, or stats tables may help to improve these queries.

Slow query logs written to table can be viewed by querying the [slow_log](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlslow_log-table/) table.

## Variables Related to the Slow Query Log

- [slow_query_log](/kb/en/server-system-variables/#slow_query_log) - enable/disable the slow query log
- [log_output](/kb/en/server-system-variables/#log_output) - how the output will be written
- [slow_query_log_file](/kb/en/server-system-variables/#slow_query_log_file) - name of the slow query log file
- [long_query_time](/kb/en/server-system-variables/#long_query_time) - time in seconds/microseconds defining a slow query
- [log_queries_not_using_indexes](/kb/en/server-system-variables/#log_queries_not_using_indexes) - whether to log queries that don't use indexes
- [log_slow_admin_statements](/kb/en/server-system-variables/#log_slow_admin_statements) - whether to log certain admin statements
- [log_slow_disabled_statements](/kb/en/server-system-variables/#log_slow_disabled_statements) - types of statements that should not be logged in the slow query log
- [min_examined_row_limit](/kb/en/server-system-variables/#min_examined_row_limit) - minimum rows a query must examine to be slow
- [log_slow_rate_limit](/kb/en/server-system-variables/#log_slow_rate_limit) - permits a fraction of slow queries to be logged
- [log_slow_verbosity](/kb/en/server-system-variables/#log_slow_verbosity) - amount of detail in the log
- [log_slow_filter](/kb/en/server-system-variables/#log_slow_filter) - limit which queries to log

## Rotating the Slow Query Log on Unix and Linux

Unix and Linux distributions offer the [logrotate](https://linux.die.net/man/8/logrotate) utility, which makes it very easy to rotate log files. See [Rotating Logs on Unix and Linux](/mariadb-administration/server-monitoring-logs/rotating-logs-on-unix-and-linux/) for more information on how to use this utility to rotate the slow query log.