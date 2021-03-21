# SHOW RELAYLOG EVENTS

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

## Syntax

```sql
SHOW RELAYLOG ['connection_name'] EVENTS
    [IN 'log_name'] [FROM pos] [LIMIT [offset,] row_count]
```

## Description

On [replicas](/replication/standard-replication/), this command shows the events in the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/). If `'log_name'` is not specified, the first relay log is shown.

Syntax for the `LIMIT` clause is the same as for [SELECT ... LIMIT](/kb/en/select/#limit).

Using the `LIMIT` clause is highly recommended because the `SHOW RELAYLOG EVENTS` command returns the complete contents of the relay log, which can be quite large.

This command does not return events related to setting user and system variables. If you need those, use [mariadb-binlog/mysqlbinlog](/clients-utilities/mysqlbinlog/).

On the primary, this command does nothing.

Requires the [REPLICA MONITOR](/kb/en/grant/#replica-monitor) privilege (&gt;= [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/)), the [REPLICATION SLAVE ADMIN](/kb/en/grant/#replication-slave-admin) privilege (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)) or the [REPLICATION SLAVE](/kb/en/grant/#replication-slave) privilege (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)).

#### connection_name

If there is only one nameless primary, or the default primary (as specified by the [default_master_connection](/kb/en/replication-and-binary-log-server-system-variables/#default_master_connection) system variable) is intended, `connection_name` can be omitted. If provided, the `SHOW RELAYLOG` statement will apply to the specified primary. `connection_name` is case-insensitive.