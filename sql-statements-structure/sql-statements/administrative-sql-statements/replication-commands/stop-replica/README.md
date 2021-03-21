# STOP SLAVE

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

## Syntax

```sql
STOP SLAVE ["connection_name"] [thread_type [, thread_type] ... ]

STOP ALL SLAVES [thread_type [, thread_type]]

STOP REPLICA ["connection_name"] [thread_type [, thread_type] ... ] -- from 10.5.1

STOP ALL REPLICAS [thread_type [, thread_type]] -- from 10.5.1

thread_type: IO_THREAD | SQL_THREAD
```

## Description

Stops the replica threads. `STOP SLAVE` requires the [SUPER](/kb/en/grant/#super) privilege, or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [REPLICATION SLAVE ADMIN](/kb/en/grant/#replication-slave-admin) privilege.

Like [START SLAVE](/kb/en/start-slave/), this statement may be used with the `IO_THREAD` and
`SQL_THREAD` options to name the thread or threads to be stopped. In almost all cases, one never need to use the `thread_type` options.

`STOP SLAVE` waits until any current replication event group affecting
one or more non-transactional tables has finished executing (if there
is any such replication group), or until the user issues a [KILL QUERY](/kb/en/kill-connection-query/) or [KILL CONNECTION](/kb/en/kill-connection-query/) statement.

Note that `STOP SLAVE` doesn't delete the connection permanently.  Next time you execute [START SLAVE](/kb/en/start-slave/) or the MariaDB server restarts, the replica connection is restored with it's [original arguments](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to). If you want to delete a connection, you should execute [RESET SLAVE](/kb/en/reset-slave/).

#### STOP ALL SLAVES

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

<code class="highlight fixed" style="white-space:pre-wrap">STOP ALL SLAVES</code> stops all your running replicas. It will give you a <code class="highlight fixed" style="white-space:pre-wrap">note</code> for every stopped connection. You can check the notes with [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings).

#### connection_name

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `connection_name` option was added as part of [multi-source replication](/replication/standard-replication/multi-source-replication) added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

If there is only one nameless master, or the default master (as specified by the [default_master_connection](/kb/en/replication-and-binary-log-server-system-variables/#default_master_connection) system variable) is intended, `connection_name` can be omitted. If provided, the `STOP SLAVE` statement will apply to the specified master. `connection_name` is case-insensitive.

#### STOP REPLICA

##### MariaDB starting with [10.5.1](/kb/en/mariadb-1051-release-notes/)

`STOP REPLICA` is an alias for `STOP SLAVE` from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/).

## See Also

- [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) is used to create and change connections.
- [START SLAVE](/kb/en/start-slave/) is used to start a predefined connection.
- [RESET SLAVE](/kb/en/reset-slave/) is used to reset parameters for a connection and also to permanently delete a master connection.