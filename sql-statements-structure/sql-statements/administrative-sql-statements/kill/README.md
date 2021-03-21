# KILL [CONNECTION |&nbsp;QUERY]

## Syntax

```sql
KILL [HARD | SOFT] [CONNECTION | QUERY [ID] ] [thread_id | USER user_name | query_id]
```

## Description

Each connection to mysqld runs in a separate thread. You can see which threads
are running with the <code class="fixed" style="white-space:pre-wrap">SHOW PROCESSLIST</code> statement and kill a
thread with the <code class="fixed" style="white-space:pre-wrap">KILL thread_id</code> statement.                             
<code class="fixed" style="white-space:pre-wrap">KILL</code> allows the optional <code class="fixed" style="white-space:pre-wrap">CONNECTION</code> or
<code class="fixed" style="white-space:pre-wrap">QUERY</code> modifier:

- <code class="fixed" style="white-space:pre-wrap">KILL CONNECTION</code> is the same as <code class="fixed" style="white-space:pre-wrap">KILL</code> with no
  modifier: It terminates the connection associated with the given thread or query id.
- <code class="fixed" style="white-space:pre-wrap">KILL QUERY</code> terminates the statement that the connection thread_id is
  currently executing, but leaves the connection itself intact.
- <code class="fixed" style="white-space:pre-wrap">KILL QUERY ID</code> (introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)) terminates the query by query_id, leaving the connection intact.

If a connection is terminated that has an active transaction, the transaction will be rolled back. If only a query is killed, the current transaction will stay active. See also [idle_transaction_timeout](/kb/en/server-system-variables/#idle_transaction_timeout).

If you have the [PROCESS](/kb/en/grant/#process) privilege, you can see all threads. If
you have the [SUPER](/kb/en/grant/#super) privilege, or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [CONNECTION ADMIN](/kb/en/grant/#connection-admin) privilege, you can kill all threads and
statements. Otherwise, you can see and kill only your own threads and
statements.

Killing queries that repair or create indexes on MyISAM and Aria tables may result in corrupted tables. Use the `SOFT` option to avoid this!

The <code class="fixed" style="white-space:pre-wrap">HARD</code> option (default) kills a command as soon as possible.  If you use
<code class="fixed" style="white-space:pre-wrap">SOFT</code>, then critical operations that may leave a table in an
inconsistent state will not be interrupted. Such operations include `REPAIR` and `INDEX` creation for [MyISAM](/kb/en/myisam/) and [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) tables ([REPAIR TABLE](/sql-statements-structure/sql-statements/table-statements/repair-table/), [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/)).

<code class="fixed" style="white-space:pre-wrap">KILL ... USER username</code> will kill all connections/queries for a
given user. <code class="fixed" style="white-space:pre-wrap">USER</code> can be specified one of the following ways:

- username  (Kill without regard to hostname)
- username@hostname
- [CURRENT_USER](/built-in-functions/secondary-functions/information-functions/current_user/) or [CURRENT_USER()](/built-in-functions/secondary-functions/information-functions/current_user/)

If you specify a thread id and that thread does not exist, you get the following error:

```sql
ERROR 1094 (HY000): Unknown thread id: <thread_id>
```

If you specify a query id that doesn't exist, you get the following error:

```sql
ERROR 1957 (HY000): Unknown query id: <query_id>
```

However, if you specify a user name, no error is issued for non-connected (or even non-existing) users. To check if the connection/query has been killed, you can use the [ROW_COUNT()](/built-in-functions/secondary-functions/information-functions/row_count/) function.

A client whose connection is killed receives the following error:

```sql
ERROR 1317 (70100): Query execution was interrupted
```

To obtain a list of existing sessions, use the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) statement or query the [Information Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) <a undefined>PROCESSLIST</a> table.

<strong>Note:</strong> You cannot use <code class="fixed" style="white-space:pre-wrap">KILL</code> with the Embedded MySQL Server
library because the embedded server merely runs inside the threads of the host
application. It does not create any connection threads of its own.

<strong>Note:</strong> You can also use 
<code class="fixed" style="white-space:pre-wrap">mysqladmin kill thread_id [,thread_id...]</code>
to kill connections. To get a list of running queries,
use <code class="fixed" style="white-space:pre-wrap">mysqladmin processlist</code>. See [mysqladmin](/clients-utilities/mysqladmin/).

[Percona Toolkit](http://www.percona.com/doc/percona-toolkit/) contains a program, [pt-kill](http://www.percona.com/doc/percona-toolkit/pt-kill.html) that can be used to automatically kill connections that match certain criteria. For example, it can be used to terminate idle connections, or connections that have been busy for more than 60 seconds.

## See Also

- [Query limits and timeouts](/replication/optimization-and-tuning/query-optimizations/query-limits-and-timeouts/)
- [Aborting statements that exceed a certain time to execute](/replication/optimization-and-tuning/query-optimizations/aborting-statements/)
- [idle_transaction_timeout](/kb/en/server-system-variables/#idle_transaction_timeout)