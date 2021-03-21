# InnoDB Undo Log

## Overview

When a [transaction](/sql-statements-structure/sql-statements/transactions) writes data, it always inserts them in the table indexes or data (in the buffer pool or in physical files). No private copies are created. The old versions of data being modified by active [XtraDB/InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) transactions are stored in the undo log. The original data can then be restored, or viewed by a consistent read.

## Implementation Details

Before a row is modified, it is copied into the undo log. Each normal row contains a pointer to the most recent version of the same row in the undo log. Each row in the undo log contains a pointer to previous version, if any. So, each modified row has an history chain.

Rows are never physically deleted until a transaction ends. If they were deleted, the restore would be impossible. Thus, rows are simply marked for deletion.

Each transaction uses a <em>view</em> of the records. The [transaction level](/kb/en/set-transaction/#isolation-levels) determines how this view is created. For example, READ UNCOMMITTED usually uses the current version of rows, even if they are not committed (<em>dirty reads</em>). Other isolation levels require that the most recent committed version of rows is searched in the undo log. READ COMMITTED uses a different view for each table, while REPEATABLE READ and SERIALIZABLE use the same view for all tables.

There is also a global history list of the data. When a transaction is committed, its history is added to this history list. The order of the list is the chronological order of the commits.

The purge thread deletes the rows in the undo log which are not needed by any existing view. The rows for which a most recent version exists are deleted, as well as the delete-marked rows.

If InnoDB needs to restore an old version, it will simply replace the newer version with the older one. When a transaction inserts a new row, there is no older version. However, in that case, the restore can be done by deleting the inserted rows.

## Effects of Long-Running Transactions

Understanding how the undo log works helps with understanding the negative effects long transactions.

- Long transactions generate several old versions of the rows in the undo log. Those rows will probably be needed for a longer time, because other long transactions will need them. Since those transactions will generate more modified rows, a sort of combinatory explosion can be observed. Thus, the undo log requires more space.
- Transaction may need to read very old versions of the rows in the history list, thus their performance will degrade.

Of course read-only transactions do not write more entries in the undo log; however, they delay the purging of existing entries.

Also, long transactions can more likely result in deadlocks, but this problem is not related to the undo log.

## Configuration

The undo log is not a log file that can be viewed on disk in the usual sense, such as the [error log](/mariadb-administration/server-monitoring-logs/error-log) or [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log), rather an area of storage.

The undo log is usually part of the physical system tablespace, but from [MariaDB 10.0](/kb/en/what-is-mariadb-100/), the [innodb_undo_directory](/kb/en/xtradbinnodb-server-system-variables/#innodb_undo_directory) and [innodb_undo_tablespaces](/kb/en/xtradbinnodb-server-system-variables/#innodb_undo_tablespaces) system variables can be used to split into different tablespaces and store in a different location (perhaps on a different storage device).

Each insert or update portion of the undo log is known as a rollback segment. The [innodb_undo_logs](/kb/en/xtradbinnodb-server-system-variables/#innodb_undo_logs) system variable specifies the number of rollback segments to be used per transaction.

The related [innodb_available_undo_logs](/kb/en/xtradbinnodb-server-status-variables/#innodb_available_undo_logs) status variable stores the total number of available InnoDB undo logs.