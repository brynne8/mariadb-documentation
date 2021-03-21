# Heuristic Recovery with the Transaction Coordinator Log

The transaction coordinator log (tc_log) is used to coordinate transactions that affect multiple [XA-capable](/sql-statements-structure/sql-statements/transactions/xa-transactions) [storage engines](/columns-storage-engines-and-plugins/storage-engines). One of the main purposes of this log is in crash recovery.

## Modes of Crash Recovery

There are two modes of crash recovery:

- Automatic crash recovery.
- Manual heuristic recovery when <a undefined>--tc-heuristic-recover</a> is set to some value other than `OFF`.

## Automatic Crash Recovery

Automatic crash recovery occurs during startup when MariaDB needs to recover from a crash and <a undefined>--tc-heuristic-recover</a> is set to `OFF`, which is the default value.

### Automatic Crash Recovery with the Binary Log-Based Transaction Coordinator Log

If MariaDB needs to perform automatic crash recovery and if the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) is enabled, then the [error log](/mariadb-administration/server-monitoring-logs/error-log) will contain messages like this:

```sql
[Note] Recovering after a crash using cmdb-mariadb-0-bin
[Note] InnoDB: Buffer pool(s) load completed at 190313 11:24:29
[Note] Starting crash recovery...
[Note] Crash recovery finished.
```

### Automatic Crash Recovery with the Memory-Mapped File-Based Transaction Coordinator Log

If MariaDB needs to perform automatic crash recovery and if the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) is <strong>not</strong> enabled, then the [error log](/mariadb-administration/server-monitoring-logs/error-log) will contain messages like this:

```sql
[Note] Recovering after a crash using tc.log
[Note] InnoDB: Buffer pool(s) load completed at 190313 11:26:32
[Note] Starting crash recovery...
[Note] Crash recovery finished.
```

## Manual Heuristic Recovery

Manual heuristic recovery occurs when <a undefined>--tc-heuristic-recover</a> is set to some value other than `OFF`. This might be needed if the server finds prepared transactions during crash recovery that are not in the transaction coordinator log. For example, the [error log](/mariadb-administration/server-monitoring-logs/error-log) might contain an error like this:

```sql
[ERROR] Found 1 prepared transactions! It means that mysqld was not shut down properly last time and critical recovery information (last binlog or tc.log file) was manually deleted after a crash. You have to start mysqld with --tc-heuristic-recover switch to commit or rollback pending transactions.
```

When manual heuristic recovery is initiated, MariaDB will ignore information about transactions in the transaction coordinator log during the recovery process. Prepared transactions that are encountered during the recovery process will either be rolled back or committed, depending on the value of <a undefined>--tc-heuristic-recover</a>.

When manual heuristic recovery is initiated, the [error log](/mariadb-administration/server-monitoring-logs/error-log) will contain a message like this:

```sql
[Note] Heuristic crash recovery mode
```

### Manual Heuristic Recovery with the Binary Log-Based Transaction Coordinator Log

If <a undefined>--tc-heuristic-recover</a> is set to some value other than `OFF` and if the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) is enabled, then MariaDB will ignore information about transactions in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) during the recovery process. Prepared transactions that are encountered during the recovery process will either be rolled back or committed, depending on the value of <a undefined>--tc-heuristic-recover</a>.

After the recovery process is complete, MariaDB will create a new empty [binary log](/mariadb-administration/server-monitoring-logs/binary-log) file, so that the old corrupt ones can be ignored.

### Manual Heuristic Recovery with the Memory-Mapped File-Based Transaction Coordinator Log

If <a undefined>--tc-heuristic-recover</a> is set to some value other than `OFF` and if the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) is <strong>not</strong> enabled, then MariaDB will ignore information about transactions in the the memory-mapped file defined by the <a undefined>--log-tc</a> option during the recovery process. Prepared transactions that are encountered during the recovery process will either be rolled back or committed, depending on the value of <a undefined>--tc-heuristic-recover</a>.