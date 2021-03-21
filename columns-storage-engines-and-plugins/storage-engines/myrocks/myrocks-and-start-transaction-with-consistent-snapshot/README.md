# MyRocks and START TRANSACTION WITH CONSISTENT SNAPSHOT

FB/MySQL has added new syntax:

```sql
START TRANSACTION WITH CONSISTENT ROCKSDB|INNODB SNAPSHOT;
```

The statement returns the binlog coordinates pointing at the snapshot.

MariaDB (and Percona Server) support extension to the regular

```sql
START TRANSACTION WITH CONSISTENT SNAPSHOT;
```

syntax as documented in [Enhancements for START TRANSACTION WITH CONSISTENT SNAPSHOT](/replication/standard-replication/enhancements-for-start-transaction-with-consistent-snapshot).

After issuing the statement, one can examine the [binlog_snapshot_file](/kb/en/replication-and-binary-log-status-variables/#binlog_snapshot_file) and [binlog_snapshot_position](/kb/en/replication-and-binary-log-status-variables/#binlog_snapshot_position) status variables to see the binlog position that corresponds to the snapshot.

## See Also

- [START TRANSACTION](/sql-statements-structure/sql-statements/transactions/start-transaction)
- [Enhancements for START TRANSACTION WITH CONSISTENT SNAPSHOT](/replication/standard-replication/enhancements-for-start-transaction-with-consistent-snapshot)