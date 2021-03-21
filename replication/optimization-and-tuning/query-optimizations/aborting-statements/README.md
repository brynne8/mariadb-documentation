# Aborting Statements that Exceed a Certain Time to Execute

##### MariaDB starting with [10.1.1](/kb/en/mariadb-1011-release-notes/)

[max_statement_time](/kb/en/server-system-variables/#max_statement_time) and the associated functionality was introduced in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

## Overview

[MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/) introduced the [max_statement_time](/kb/en/server-system-variables/#max_statement_time) system variable. When set to a non-zero value, any queries taking longer than this time in seconds will be aborted. The default is zero, and no limits are then applied. The aborted query has no effect on any larger transaction or connection contexts. The variable is of type double, thus you can use subsecond timeout. For example you can use value 0.01 for 10 milliseconds timeout.

The value can be set globally or per session, as well as per user or per query (see below).
Slave's are not affected by this variable.

An associated status variable, [max_statement_time_exceeded](/kb/en/server-status-variables/#max_statement_time_exceeded), stores the number of queries that have exceeded the execution time specified by [max_statement_time](/kb/en/server-system-variables/#max_statement_time), and a `MAX_STATEMENT_TIME_EXCEEDED` column was added to the [CLIENT_STATISTICS](/kb/en/information-schema-client_statistics-table/) and [USER STATISTICS](/kb/en/information-schema-user_statistics-table/) Information Schema tables.

The feature was based upon a patch by Davi Arnaut.

## User [max_statement_time](/kb/en/server-system-variables/#max_statement_time)

[max_statement_time](/kb/en/server-system-variables/#max_statement_time) can be stored per user with the [GRANT ... MAX_STATEMENT_TIME](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) syntax.

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

## Per-query [max_statement_time](/kb/en/server-system-variables/#max_statement_time)

By using [max_statement_time](/kb/en/server-system-variables/#max_statement_time) in conjunction with [SET STATEMENT](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set-statement/), it is possible to limit the execution time of individual queries. For example:

```sql
SET STATEMENT max_statement_time=100 FOR 
  SELECT field1 FROM table_name ORDER BY field1;
```

max_statement_time per query
Individual queries can also be limited by adding a `MAX_STATEMENT_TIME` clause to the query. For example:

```sql
SELECT MAX_STATEMENT_TIME=2 * FROM t1;
```

## Limitations

- [max_statement_time](/kb/en/server-system-variables/#max_statement_time) does not work in embedded servers.
- [max_statement_time](/kb/en/server-system-variables/#max_statement_time) does not work for [COMMIT](/sql-statements-structure/sql-statements/transactions/commit/) statements in a Galera cluster (see [MDEV-18673](https://jira.mariadb.org/browse/MDEV-18673) for discussion).

## Differences Between the MariaDB and MySQL Implementations

MySQL 5.7.4 introduced similar functionality, but the MariaDB implementation differs in a number of ways.

- The MySQL version of [max_statement_time](/kb/en/server-system-variables/#max_statement_time) (`max_execution_time`) is defined in millseconds, not seconds
- MySQL's implementation can only kill SELECTs, while MariaDB's can kill any queries (excluding stored procedures).
- MariaDB only introduced the [max_statement_time_exceeded](/kb/en/server-status-variables/#max_statement_time_exceeded) status variable, while MySQL also introduced a number of other variables which were not seen as necessary in MariaDB.
- The `SELECT MAX_STATEMENT_TIME = N ...` syntax is not valid in MariaDB.

## See Also

- [Query limits and timeouts](/replication/optimization-and-tuning/query-optimizations/query-limits-and-timeouts/)
- [lock_wait_timeout](/kb/en/server-system-variables/#lock_wait_timeout) variable