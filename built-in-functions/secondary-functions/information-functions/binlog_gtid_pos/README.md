# BINLOG_GTID_POS

##### MariaDB starting with [10.0.2](/kb/en/mariadb-1002-release-notes/)

From version 10.0.2, MariaDB supports [global transaction IDs](/kb/en/global-transaction-id/) for replication.

## Syntax

```sql
BINLOG_GTID_POS(binlog_filename,binlog_offset)
```

## Description

The BINLOG_GTID_POS() function takes as input an old-style [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position in the form of a file name and a file offset. It looks up the position in the current binlog, and returns a string representation of the corresponding [GTID](/kb/en/global-transaction-id/) position. If the position is not found in the current binlog, NULL is returned.

## Examples

```sql
SELECT BINLOG_GTID_POS("master-bin.000001", 600);
```

## See Also

- [SHOW BINLOG EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-binlog-events/) - Show events and their positions in the binary log