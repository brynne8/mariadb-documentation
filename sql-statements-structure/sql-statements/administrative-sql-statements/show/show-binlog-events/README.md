# SHOW BINLOG EVENTS

## Syntax

```sql
SHOW BINLOG EVENTS
   [IN 'log_name'] [FROM pos] [LIMIT [offset,] row_count]
```

## Description

Shows the events in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log). If you do not specify '`log_name`',
the first binary log is displayed.

Requires the [BINLOG MONITOR](/kb/en/grant/#binlog-monitor) privilege (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)) or the [REPLICATION SLAVE](/kb/en/grant/#replication-slave) privilege (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)).

## Example

```sql
SHOW BINLOG EVENTS IN 'mysql_sandbox10019-bin.000002';
+-------------------------------+-----+-------------------+-----------+-------------+------------------------------------------------+
| Log_name                      | Pos | Event_type        | Server_id | End_log_pos | Info                                           |
+-------------------------------+-----+-------------------+-----------+-------------+------------------------------------------------+
| mysql_sandbox10019-bin.000002 |   4 | Format_desc       |         1 |         248 | Server ver: 10.0.19-MariaDB-log, Binlog ver: 4 |
| mysql_sandbox10019-bin.000002 | 248 | Gtid_list         |         1 |         273 | []                                             |
| mysql_sandbox10019-bin.000002 | 273 | Binlog_checkpoint |         1 |         325 | mysql_sandbox10019-bin.000002                  |
| mysql_sandbox10019-bin.000002 | 325 | Gtid              |         1 |         363 | GTID 0-1-1                                     |
| mysql_sandbox10019-bin.000002 | 363 | Query             |         1 |         446 | CREATE DATABASE blog                           |
| mysql_sandbox10019-bin.000002 | 446 | Gtid              |         1 |         484 | GTID 0-1-2                                     |
| mysql_sandbox10019-bin.000002 | 484 | Query             |         1 |         571 | use `blog`; CREATE TABLE bb (id INT)           |
+-------------------------------+-----+-------------------+-----------+-------------+------------------------------------------------+
```