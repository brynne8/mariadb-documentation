# Group Commit for the Binary Log

## Overview

The server supports group commit. This is an important optimization that helps MariaDB reduce the number of expensive disk operations that are performed.

## Durability

In ACID terminology, the "D" stands for durability. In order to ensure durability with group commit, <a undefined>innodb_flush_log_at_trx_commit=1</a> and/or <a undefined>sync_binlog=1</a> should be set. These settings are needed to ensure that if the server crashes, then any transaction that was committed prior to the time of crash will still be present in the database after crash recovery.

### Durable InnoDB Data and Binary Logs

Setting both <a undefined>innodb_flush_log_at_trx_commit=1</a> and <a undefined>sync_binlog=1</a> provides the most durability and the best guarantee of [replication](/kb/en/high-availability-performance-tuning-mariadb-replication/) consistency after a crash.

### Non-Durable InnoDB Data

If <a undefined>sync_binlog=1</a> is set, but <a undefined>innodb_flush_log_at_trx_commit</a> is not set to `1` or `3`, then it is possible after a crash to end up in a state where a transaction present in a server's [binary log](/mariadb-administration/server-monitoring-logs/binary-log) is missing from the server's [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/). If the server is a [replication master](/kb/en/high-availability-performance-tuning-mariadb-replication/), then that means that the server can become inconsistent with its slaves, since the slaves may have replicated transactions from the master's [binary log](/mariadb-administration/server-monitoring-logs/binary-log) that are no longer present in the master's local [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) data.

### Non-Durable Binary Logs

If <a undefined>innodb_flush_log_at_trx_commit</a> is set to `1` or `3`, but <a undefined>sync_binlog=1</a> is not set, then it is possible after a crash to end up in a state where a transaction present in a server's [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) is missing from the server's [binary log](/mariadb-administration/server-monitoring-logs/binary-log). If the server is a [replication master](/kb/en/high-availability-performance-tuning-mariadb-replication/), then that also means that the server can become inconsistent with its slaves, since the server's slaves would not be able to replicate the missing transactions from the server's [binary log](/mariadb-administration/server-monitoring-logs/binary-log).

### Non-Durable InnoDB Data and Binary Logs

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and above, setting <a undefined>innodb_flush_log_at_trx_commit=1</a> when <a undefined>sync_binlog=1</a> is not set can also cause the transaction to be missing from the server's [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) due to some optimizations added in those versions. In that case, it is recommended to always set <a undefined>sync_binlog=1</a>. If you can't do that, then it is recommended to set <a undefined>innodb_flush_log_at_trx_commit</a> to `3`, rather than `1`. See [Non-durable Binary Log Settings](/kb/en/binary-log-group-commit-and-innodb-flushing-performance/#non-durable-binary-log-settings) for more information.

## Amortizing Disk Flush Costs

After every transaction [COMMIT](/sql-statements-structure/sql-statements/transactions/commit), the server normally has to flush any changes the transaction made to the [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) and the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) to disk (i.e. by calling system calls such as `fsync()` or `fdatasync()` or similar). This helps ensure that the data changes made by the transaction are stored durably on the disk. Disk flushing is a time-consuming operation, and can easily impose limits on throughput in terms of the number of transactions-per-second (TPS) which can be committed.

The idea with group commit is to amortize the costs of each flush to disk over multiple commits from multiple parallel transactions. For example, if there are 10 transactions trying to commit in parallel, then we can force all of them to be flushed disk at once with a single system call, rather than do one system call for each commit. This can greatly reduce the need for flush operations, and can consequently greatly improve the throughput of transactions-per-second (TPS).

However, to see the positive effects of group commit, the workload must have sufficient parallelism. A good rule of thumb is that at least three parallel transactions are needed for group commit to be effective. For example, while the first transaction is waiting for its flush operation to complete, the other two transactions will queue up waiting for their turn to flush their changes to disk. When the first transaction is done, a single system call can be used to flush the two queued-up transactions, saving in this case one of the three system calls.

In addition to sufficient parallelism, it is also necessary to have enough transactions per second wanting to commit that the flush operations are a bottleneck. If no such bottleneck exists (i.e. transactions never or rarely
need to wait for the flush of another to complete), then group commit will provide little to no improvement.

## Changing Group Commit Frequency

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The frequency of group commits can be changed by configuring the <a undefined>binlog_commit_wait_usec</a> and <a undefined>binlog_commit_wait_count</a> system variables, which were introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

## Measuring Group Commit Ratio

Two status variables are available for checking how
effective group commit is at reducing flush overhead. These are the <a undefined>Binlog_commits</a> and <a undefined>Binlog_group_commits</a> status variables. We can obtain those values with the following query:

```sql
SHOW GLOBAL STATUS WHERE Variable_name IN('Binlog_commits', 'Binlog_group_commits');
```

<a undefined>Binlog_commits</a> is the total number of transactions committed to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).

<a undefined>Binlog_group_commits</a> is the total number of groups committed to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log). As explained in the previous sections of this page, a group commit is when a group of transactions is flushed to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) together by sharing a single flush system call. When <a undefined>sync_binlog=1</a> is set, then this is also the total number of flush system calls executed in the process of flushing commits to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).

Thus the extent to which group commit is effective at reducing the number of flush system calls on the binary log can be determined by the ratio between these two status variables. <a undefined>Binlog_commits</a> will always be as equal to or greater than <a undefined>Binlog_group_commits</a>. The greater the difference is between these status variables, the more effective group commit was at reducing flush overhead.

To calculate the group commit ratio, we actually need the values of these status variables from two snapshots. Then we can calculate the ratio with the following formula:

`transactions/group commit` = (`Binlog_commits` (<em>snapshot2</em>) - `Binlog_commits` (<em>snapshot1</em>))/(`Binlog_group_commits` (<em>snapshot2</em>) - `Binlog_group_commits` (<em>snapshot1</em>))

For example, if we had the following first snapshot:

```sql
SHOW GLOBAL STATUS WHERE Variable_name IN('Binlog_commits', 'Binlog_group_commits');
+----------------------+-------+
| Variable_name        | Value |
+----------------------+-------+
| Binlog_commits       | 120   |
| Binlog_group_commits | 120   |
+----------------------+-------+
2 rows in set (0.00 sec)
```

And the following second snapshot:

```sql
SHOW GLOBAL STATUS WHERE Variable_name IN('Binlog_commits', 'Binlog_group_commits');
+----------------------+-------+
| Variable_name        | Value |
+----------------------+-------+
| Binlog_commits       | 220   |
| Binlog_group_commits | 145   |
+----------------------+-------+
2 rows in set (0.00 sec)
```

Then we would have:

`transactions/group commit` = (220 - 120) / (145 - 120) = 100 / 25 = 4 `transactions/group commit`

If your group commit ratio is too close to 1, then it may help to [change your group commit frequency](/kb/en/group-commit-for-the-binary-log/#changing-group-commit-frequency).

## Use of Group Commit with Parallel Replication

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and above, group commit is also used to enable [conservative mode of in-order parallel replication](/kb/en/parallel-replication/#conservative-mode-of-in-order-parallel-replication).

## Effects of Group Commit on InnoDB Performance

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and above, when both <a undefined>innodb_flush_log_at_trx_commit=1</a> (the default) is set and the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) is enabled, there is now one less sync to disk inside InnoDB during commit (2 syncs shared between a group of transactions instead of 3). See [Binary Log Group Commit and InnoDB Flushing Performance](/columns-storage-engines-and-plugins/storage-engines/innodb/binary-log-group-commit-and-innodb-flushing-performance) for more information.

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The [InnoDB flushing performance improvements](/columns-storage-engines-and-plugins/storage-engines/innodb/binary-log-group-commit-and-innodb-flushing-performance) related to group commit were first introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

## Status Variables

<a undefined>Binlog_commits</a> is the total number of transactions committed to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).

<a undefined>Binlog_group_commits</a> is the total number of groups committed to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).

<a undefined>Binlog_group_commit_trigger_count</a> is the total number of group commits triggered because of the number of [binary log](/mariadb-administration/server-monitoring-logs/binary-log) commits in the group reached the limit set by the system variable <a undefined>binlog_commit_wait_count</a>.

<a undefined>Binlog_group_commit_trigger_lock_wait</a> is the total number of group commits triggered because a [binary log](/mariadb-administration/server-monitoring-logs/binary-log) commit was being delayed because of a lock wait where the lock was held by a prior binary log commit. When this happens the later binary log commit is placed in the next group commit.

<a undefined>Binlog_group_commit_trigger_timeout</a> is the total number of group commits triggered because of the time since the first [binary log](/mariadb-administration/server-monitoring-logs/binary-log) commit reached the limit set by the system variable <a undefined>binlog_commit_wait_usec</a>.

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The <a undefined>Binlog_group_commit_trigger_count</a>, <a undefined>Binlog_group_commit_trigger_lock_wait</a>, and <a undefined>Binlog_group_commit_trigger_timeout</a> status variables were first introduced in [MariaDB 10.0.18](/kb/en/mariadb-10018-release-notes/) and [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/).

To query these variables, use a statement such as:

```sql
SHOW GLOBAL STATUS LIKE 'Binlog_%commit%';
```

## See Also

- [Parallel Replication](/replication/standard-replication/parallel-replication)
- [Binary Log Group Commit and InnoDB Flushing Performance](/columns-storage-engines-and-plugins/storage-engines/innodb/binary-log-group-commit-and-innodb-flushing-performance)
- [Group commit benchmark](http://www.facebook.com/note.php?note_id=10150211546215933)