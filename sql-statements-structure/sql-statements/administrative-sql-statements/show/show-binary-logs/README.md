# SHOW BINARY LOGS

## Syntax

```sql
SHOW BINARY LOGS
SHOW MASTER LOGS
```

## Description

Lists the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) files on the server. This statement is used as part of the
procedure described in <a undefined>PURGE BINARY LOGS</a>, that shows how to
determine which logs can be purged.

This statement requires the [SUPER](/kb/en/grant/#super) privilege, the [REPLICATION_CLIENT](/kb/en/grant/#replication-client) privilege, or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [BINLOG MONITOR](/kb/en/grant/#binlog-monitor) privilege.

## Examples

```sql
SHOW BINARY LOGS;
+--------------------+-----------+
| Log_name           | File_size |
+--------------------+-----------+
| mariadb-bin.000001 |     19039 |
| mariadb-bin.000002 |    717389 |
| mariadb-bin.000003 |       300 |
| mariadb-bin.000004 |       333 |
| mariadb-bin.000005 |       899 |
| mariadb-bin.000006 |       125 |
| mariadb-bin.000007 |     18907 |
| mariadb-bin.000008 |     19530 |
| mariadb-bin.000009 |       151 |
| mariadb-bin.000010 |       151 |
| mariadb-bin.000011 |       125 |
| mariadb-bin.000012 |       151 |
| mariadb-bin.000013 |       151 |
| mariadb-bin.000014 |       125 |
| mariadb-bin.000015 |       151 |
| mariadb-bin.000016 |       314 |
+--------------------+-----------+
```