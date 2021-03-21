# InnoDB Redo Log

## Overview

The redo log is used by [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) during crash recovery. The redo log files have names like `ib_logfileN`, where `N` is an integer (from [MariaDB 10.5](/kb/en/what-is-mariadb-105/), there is only one redo log, so the file will always be named `ib_logfile0`. If the [innodb_log_group_home_dir](/kb/en/innodb-system-variables/#innodb_log_group_home_dir) system variable is configured, then the redo log files will be created in that directory. Otherwise, they will be created in the directory defined by the [datadir](/kb/en/server-system-variables/#datadir) system variable.

## Flushing Effects on Performance and Consistency

The [innodb_flush_log_at_trx_commit](/kb/en/innodb-system-variables/#innodb_flush_log_at_trx_commit) system variable determines how often the transactions are flushed to the redo log, and it is important to achieve a good balance between speed and reliability.

### Binary Log Group Commit and Redo Log Flushing

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and above, when both [innodb_flush_log_at_trx_commit=1](/kb/en/innodb-system-variables/#innodb_flush_log_at_trx_commit) (the default) is set and the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) is enabled, there is now one less sync to disk inside InnoDB during commit (2 syncs shared between a group of transactions instead of 3). See [Binary Log Group Commit and InnoDB Flushing Performance](/columns-storage-engines-and-plugins/storage-engines/innodb/binary-log-group-commit-and-innodb-flushing-performance) for more information.

## Redo Log Group Capacity

The redo log group capacity is the total combined size of all InnoDB redo logs. The relevant factors are:

- From [MariaDB 10.5](/kb/en/what-is-mariadb-105/), there is 1 redo log. For [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and before, the number of redo log files is configured by the [innodb_log_files_in_group](/kb/en/innodb-system-variables/#innodb_log_files_in_group) system variable.
- The size of each redo log file is configured by the [innodb_log_file_size](/kb/en/innodb-system-variables/#innodb_log_file_size) system variable.

Therefore, redo log group capacity is determined by the following calculation:

`innodb_log_group_capacity` =  <a undefined>innodb_log_file_size</a> * <a undefined>innodb_log_files_in_group</a>

For example, if [innodb_log_file_size](/kb/en/innodb-system-variables/#innodb_log_file_size) is set to `2G` and [innodb_log_files_in_group](/kb/en/innodb-system-variables/#innodb_log_files_in_group) is set to `2`, then we would have the following:

- `innodb_log_group_capacity` =  <a undefined>innodb_log_file_size</a> * <a undefined>innodb_log_files_in_group</a>
- = `2G` * `2`
- = `4G`

### Changing the Redo Log Group Capacity

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later, the number (until [MariaDB 10.4](/kb/en/what-is-mariadb-104/) only - from [MariaDB 10.5](/kb/en/what-is-mariadb-105/) there is only 1 redo log) or size of redo log files can be changed with the following process:

- Stop the server.
- To change the log file size, configure [innodb_log_file_size](/kb/en/innodb-system-variables/#innodb_log_file_size). To increase the number of log files (until [MariaDB 10.4](/kb/en/what-is-mariadb-104/) only), configure [innodb_log_files_in_group](/kb/en/innodb-system-variables/#innodb_log_files_in_group).
- Start the server.

##### MariaDB until [5.5](/kb/en/what-is-mariadb-55/)

In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and before, the number or size of redo log files can be changed with the following process:

- If [innodb_fast_shutdown](/kb/en/innodb-system-variables/#innodb_fast_shutdown) is set to `2`, then set it to `1`:

```sql
SET GLOBAL innodb_fast_shutdown = 1;
```

- Stop the server.
- Copy the pre-existing redo log files to a safe place.
- Delete the pre-existing redo log files from the `datadir`.
- To change the log file size, configure [innodb_log_file_size](/kb/en/innodb-system-variables/#innodb_log_file_size). To increase the number of log files, configure [innodb_log_files_in_group](/kb/en/innodb-system-variables/#innodb_log_files_in_group).
- Start the server.

## Log Sequence Number (LSN)

Records within the InnoDB redo log are identified via a log sequence number (LSN).

## Checkpoints

When InnoDB performs a checkpoint, it writes the LSN of the oldest dirty page in the [InnoDB buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool) to the InnoDB redo log. If a page is the oldest dirty page in the  [InnoDB buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool), then that means that all pages with lower LSNs have been flushed to the physical InnoDB tablespace files. If the server were to crash, then InnoDB would perform crash recovery by only applying log records with LSNs that are greater than or equal to the LSN of the oldest dirty page written in the last checkpoint.

Checkpoints are one of the tasks performed by the InnoDB master background thread. This thread schedules checkpoints 7 seconds apart when the server is very active, but checkpoints can happen more frequently when the server is less active.

Dirty pages are not actually flushed from the buffer pool to the physical InnoDB tablespace files during a checkpoint. That process happens asynchronously on a continuous basis by InnoDB's write I/O background threads configured by the [innodb_write_io_threads](/kb/en/innodb-system-variables/#innodb_write_io_threads) system variable. If you want to make this process more aggressive, then you can decrease the value of the [innodb_max_dirty_pages_pct](/kb/en/innodb-system-variables/#innodb_max_dirty_pages_pct) system variable. You may also need to better tune InnoDB's I/O capacity on your system by setting the [innodb_io_capacity](/kb/en/innodb-system-variables/#innodb_io_capacity) system variable.

### Determining the Checkpoint Age

The checkpoint age is the amount of data written to the InnoDB redo log since the last checkpoint.

#### Determining the Checkpoint Age in InnoDB

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, MariaDB uses InnoDB. In those versions, the checkpoint age can be determined by the process shown below.

To determine the InnoDB checkpoint age, do the following:

- Query [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status).
- Find the `LOG` section. For example:

```sql
---
LOG
---
Log sequence number 252794398789379
Log flushed up to 252794398789379
Pages flushed up to 252792767756840
Last checkpoint at 252792767756840
0 pending log flushes, 0 pending chkp writes
23930412 log i/o's done, 2.03 log i/o's/second
```

- Perform the following calculation:

`innodb_checkpoint_age` = `Log sequence number` - `Last checkpoint at`

In the example above, that would be:

- `innodb_checkpoint_age` = `Log sequence number` - `Last checkpoint at`
- = 252794398789379 - 252792767756840
- = 1631032539 bytes
- = 1631032539 byes / (1024 * 1024 * 1024) (GB/bytes)
- = 1.5 GB of redo log written since last checkpoint

#### Determining the Checkpoint Age in XtraDB

##### MariaDB until [10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, MariaDB uses XtraDB by default. In those versions, the checkpoint age can be determined by the [Innodb_checkpoint_age](/kb/en/innodb-status-variables/#innodb_checkpoint_age) status variable.

## Determining the Redo Log Occupancy

The redo log occupancy is the percentage of the InnoDB redo log capacity that is taken up by dirty pages that have not yet been flushed to the physical InnoDB tablespace files in a checkpoint. Therefore, it's determined by the following calculation:

`innodb_log_occupancy` = <a undefined>innodb_checkpoint_age</a> / <a undefined>innodb_log_group_capacity</a>

For example, if <a undefined>innodb_checkpoint_age</a> is `1.5G` and <a undefined>innodb_log_group_capacity</a> is `4G`, then we would have the following:

- `innodb_log_occupancy` = <a undefined>innodb_checkpoint_age</a> / <a undefined>innodb_log_group_capacity</a>
- = `1.5G` / `4G`
- = `0.375`

If the calculated value for redo log occupancy is too close to `1.0`, then the InnoDB redo log capacity may be too small for the current workload.