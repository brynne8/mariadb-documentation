# Binary Log Group Commit and InnoDB Flushing Performance

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

[MariaDB 10.0](/kb/en/what-is-mariadb-100/) introduced a performance improvement related to [group commit](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/) that affects the performance of flushing [InnoDB](/kb/en/xtradb-and-innodb/) transactions when the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is enabled.

## Overview

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and above, when both <a undefined>innodb_flush_log_at_trx_commit=1</a> (the default) is set and the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is enabled, there is now one less sync to disk inside InnoDB during commit (2 syncs shared between a group of transactions instead of 3).

Durability of commits is not decreased <span>—</span> this is because even if the server crashes before the commit is written to disk by InnoDB, it will be recovered from the binary log at next server startup (and it is guaranteed that sufficient information is synced to disk so that such a recovery is always possible).

## Switching to Old Flushing Behavior

The old behavior, with 3 syncs to disk per (group) commit (and consequently lower performance), can be selected with the new <a undefined>innodb_flush_log_at_trx_commit=3</a> option. There is normally no
benefit to doing this, however there are a couple of edge cases to be aware of.

### Non-durable Binary Log Settings

If <a undefined>innodb_flush_log_at_trx_commit=1</a> is set and the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is enabled, but <a undefined>sync_binlog=0</a> is set, then commits are not guaranteed durable inside InnoDB after commit. This is because if <a undefined>sync_binlog=0</a> is set and if the server crashes, then transactions that were not flushed to the binary log prior to the crash will be missing from the binary log.

In this specific scenario, <a undefined>innodb_flush_log_at_trx_commit=3</a> can be set to ensure that transactions will be durable in InnoDB, even if they are not necessarily durable from the perspective of the binary log.

One should be aware that if <a undefined>sync_binlog=0</a> is set, then a crash is nevertheless likely to cause transactions to be missing from the binary log. This will cause the binary log and InnoDB to be inconsistent with each other. This is also likely to cause any [replication slaves](/kb/en/high-availability-performance-tuning-mariadb-replication/) to become inconsistent, since transactions are replicated through the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/). Thus it is recommended to set <a undefined>sync_binlog=1</a>. With the [group commit](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/) improvements introduced in [MariaDB 5.3](/kb/en/what-is-mariadb-53/), this setting has much less penalty in recent versions compared to older versions of MariaDB and MySQL.

### Recent Transactions Missing from Backups

[Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/) and [Percona XtraBackup](/kb/en/backup-restore-and-import-xtrabackup/) only see transactions that have been flushed to the [redo log](/kb/en/xtradbinnodb-redo-log/). With the [group commit](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/) improvements, there may be a small delay (defined by the <a undefined>binlog_commit_wait_usec</a> system variable) between when a commit happens and when the commit will be included in a backup.

Note that the backup will still be fully consistent with itself and the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/). This problem is normally not an issue in practice. A backup usually takes a long time to complete (relative to the 1 second or so that <a undefined>binlog_commit_wait_usec</a> is normally set to), and a backup usually includes a lot of transactions that were committed during the backup. With this in mind, it is not generally noticeable if the backup does not include transactions that were committed during the last 1 second or so of the backup process. It is just mentioned here for completeness.