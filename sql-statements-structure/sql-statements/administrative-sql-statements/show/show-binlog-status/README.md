# SHOW MASTER STATUS

## Syntax

```sql
SHOW MASTER STATUS
SHOW BINLOG STATUS -- From MariaDB 10.5.2
```

## Description

Provides status information about the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) files of the primary.

This statement requires the [SUPER](/kb/en/grant/#super) privilege, the [REPLICATION_CLIENT](/kb/en/grant/#replication-client) privilege, or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [BINLOG MONITOR](/kb/en/grant/#binlog-monitor) privilege.

To see information about the current GTIDs in the binary log, use the
[gtid_binlog_pos](/kb/en/global-transaction-id/#gtid_binlog_pos) variable.

`SHOW MASTER STATUS` was renamed to `SHOW BINLOG STATUS` in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), but the old name remains an alias for compatibility purposes.

## Example

```sql
SHOW MASTER STATUS;
+--------------------+----------+--------------+------------------+
| File               | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+--------------------+----------+--------------+------------------+
| mariadb-bin.000016 |      475 |              |                  |
+--------------------+----------+--------------+------------------+
SELECT @@global.gtid_binlog_pos;
+--------------------------+
| @@global.gtid_binlog_pos |
+--------------------------+
| 0-1-2                    |
+--------------------------+
```

## See Also

- [MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/)
- [Using and Maintaining the Binary Log](/mariadb-administration/server-monitoring-logs/binary-log/using-and-maintaining-the-binary-log/)
- [The gtid_binlog_pos variable](/kb/en/global-transaction-id/#gtid_binlog_pos)