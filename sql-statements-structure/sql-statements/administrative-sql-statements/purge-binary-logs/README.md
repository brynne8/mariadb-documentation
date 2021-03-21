# PURGE BINARY LOGS

## Syntax

```sql
PURGE { BINARY | MASTER } LOGS
    { TO 'log_name' | BEFORE datetime_expr }
```

## Description

The <code class="fixed" style="white-space:pre-wrap">PURGE BINARY LOGS</code> statement deletes all the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/)
files listed in the log index file prior to the specified log file name or
date. <code class="fixed" style="white-space:pre-wrap">BINARY</code> and <code class="fixed" style="white-space:pre-wrap">MASTER</code> are synonyms.
Deleted log files also are removed from the list recorded in the index file, so
that the given log file becomes the first in the list.

The datetime expression is in the format 'YYYY-MM-DD hh:mm:ss'.

If a replica is active but has yet to read from a binary log file you attempt to delete, the statement will fail with an error. However, if the replica is not connected and has yet to read from a log file you delete, the file will be deleted, but the replica will be unable to continue replicating once it connects again.

This statement has no effect if the server was not started with the
[--log-bin](/kb/en/replication-and-binary-log-system-variables/#log_bin) option to enable binary logging.

To list the binary log files on the server, use [SHOW BINARY LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-binary-logs/). To see which files they are reading, use [SHOW SLAVE STATUS](/kb/en/show-slave-status/) (or [SHOW REPLICA STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-replica-status/) from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)). You can only delete the files that are older than the oldest file that is used by the slaves.

To delete all binary log files, use [RESET MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-master/).
To move to a new log file (for example if you want to remove the current log file), use [FLUSH LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) before you execute `PURGE LOGS`.

If the [expire_logs_days](/kb/en/server-system-variables/#expire_logs_days) server system variable is not set to 0, the server automatically deletes binary log files after the given number of days.

Requires the [SUPER](super) privilege or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [BINLOG ADMIN](/kb/en/grant/#binlog-admin) privilege, to run.

## Examples

```sql
PURGE BINARY LOGS TO 'mariadb-bin.000063';
```

```sql
PURGE BINARY LOGS BEFORE '2013-04-21';
```

```sql
PURGE BINARY LOGS BEFORE '2013-04-22 09:55:22';
```

## See Also

- [Using and Maintaining the Binary Log](/mariadb-administration/server-monitoring-logs/binary-log/using-and-maintaining-the-binary-log/)
- [FLUSH LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).