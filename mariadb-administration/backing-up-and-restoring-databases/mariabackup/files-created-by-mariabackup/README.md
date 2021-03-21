# Files Created by Mariabackup

Mariabackup creates the following files:

## `backup-my.cnf`

During the backup, any server options relevant to Mariabackup are written to the `backup-my.cnf` [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), so that they can be re-read later during the `--prepare` stage.

## `ib_logfile0`

In [MariaDB 10.2.10](/kb/en/mariadb-10210-release-notes/) and later, Mariabackup creates an empty [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) file called `ib_logfile0` as part of the <a undefined>--prepare</a> stage. This file has 3 roles:

1 In the source server, `ib_logfile0` is the first (and possibly the only) [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) file.
2 In the non-prepared backup, `ib_logfile0` contains all of the [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) copied during the backup. Previous versions of Mariabackup would use a file called <a undefined>xtrabackup_logfile</a> for this.
3 During the <a undefined>--prepare</a> stage,  `ib_logfile0` would previously be deleted. Now during the `--prepare` stage, `ib_logfile0` is initialized as an empty [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) file. That way, if the backup is manually restored, any pre-existing [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) files would get overwritten by the empty one. This helps to prevent certain kinds of known issues. For example, see [Mariabackup Overview: Manual Restore with Pre-existing InnoDB Redo Log files](/kb/en/mariabackup-overview/#manual-restore-with-pre-existing-innodb-redo-log-files).

## `xtrabackup_logfile`

In [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/) and before, Mariabackup creates `xtrabackup_logfile` to store the [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/), In later versions, <a undefined>ib_logfile0</a> is created instead.

## `xtrabackup_binlog_info`

This file stores the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) file name and position that corresponds to the backup.

This file also stores the value of the <a undefined>gtid_current_pos</a> system variable that correspond to the backup.

For example:

```sql
mariadb-bin.000096 568 0-1-2
```

The values in this file are only guaranteed to be consistent with the backup if the <a undefined>--no-lock</a> option was <strong>not</strong> provided when the backup was taken.

## `xtrabackup_binlog_pos_innodb`

Mariabackup inherited this file from Percona XtraBackup 2.3. However, this file is only safe to use with Percona Server versions that have a special lockless binary log feature.

This feature is described by the [Percona Server documentation](https://www.percona.com/doc/percona-server/5.6/management/backup_locks.html#backup-safe-binlog-information):

Starting with Percona Server for MySQL 5.6.26-74.0 LOCK TABLES FOR BACKUP flushes the current binary log coordinates to InnoDB. Thus, under active LOCK TABLES FOR BACKUP, the binary log coordinates in InnoDB are consistent with its redo log and any non-transactional updates (as the latter are blocked by LOCK TABLES FOR BACKUP). It is planned that this change will enable Percona XtraBackup to avoid issuing the more invasive LOCK BINLOG FOR BACKUP command under some circumstances.

And the way it's used by Percona XtraBackup 2.3 is described by the [Percona XtraBackup 2.3 documentation](https://www.percona.com/doc/percona-xtrabackup/2.3/advanced/lockless_bin-log.html):

Percona XtraBackup implemented support for lock-less binary log information in 2.3.2. When the Lockless binary log information feature is available [1] on the server, Percona XtraBackup can trust binary log information stored in the InnoDB system header and avoid executing LOCK BINLOG FOR BACKUP (and thus, blocking commits for the duration of finalizing the REDO log copy) under a number of circumstances:

- when the server is not a GTID-enabled Galera cluster node
- when the replication I/O thread information should not be stored as a part of the backup (i.e. when the xtrabackup --slave-info option is not specified)

Mariabackup no longer creates this file starting with [MariaDB 10.1.39](/kb/en/mariadb-10139-release-notes/), [MariaDB 10.2.23](/kb/en/mariadb-10223-release-notes/), [MariaDB 10.3.14](/kb/en/mariadb-10314-release-notes/), and [MariaDB 10.4.4](/kb/en/mariadb-1044-release-notes/).

See [MDEV-18917](https://jira.mariadb.org/browse/MDEV-18917) for more information.

## `xtrabackup_checkpoints`

The `xtrabackup_checkpoints` file contains metadata about the backup.

For example:

```sql
backup_type = full-backuped
from_lsn = 0
to_lsn = 1635102
last_lsn = 1635102
recover_binlog_info = 0
```

See below for a description of the fields.

If the <a undefined>--extra-lsndir</a> option is provided, then an extra copy of this file will be saved in that directory.

### `backup_type`

If the backup is a non-prepared [full backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup/) or a non-prepared [partial backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup/), then `backup_type`  is set to `full-backuped`.

If the backup is a non-prepared [incremental backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/incremental-backup-and-restore-with-mariabackup/), then `backup_type`  is set to `incremental`.

If the backup has already been prepared, then `backup_type`  is set to `log-applied`.

### `from_lsn`

If `backup_type` is `full-backuped`, then `from_lsn` has the value of `0`.

If `backup_type` is `incremental`, then `from_lsn` has the value of the [log sequence number (LSN)](/kb/en/innodb-redo-log/#log-sequence-number-lsn) at which the backup started reading from the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/). This is internally used by Mariabackup when preparing incremental backups.

This value can be manually set during an [incremental backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/incremental-backup-and-restore-with-mariabackup/) with the <a undefined>--incremental-lsn</a> option. However, it is generally better to let Mariabackup figure out the `from_lsn` automatically by specifying a parent backup with the <a undefined>--incremental-basedir</a> option.

### `to_lsn`

`to_lsn` has the value of the [log sequence number (LSN)](/kb/en/innodb-redo-log/#log-sequence-number-lsn) of the last checkpoint in the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/). This is internally used by Mariabackup when preparing incremental backups.

### `last_lsn`

`last_lsn` has the value of the last [log sequence number (LSN)](/kb/en/innodb-redo-log/#log-sequence-number-lsn) read from the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/). This is internally used by Mariabackup when preparing incremental backups.

## `xtrabackup_info`

The `xtrabackup_info` file contains information about the backup. The fields in this file are listed below.

If the <a undefined>--extra-lsndir</a> option is provided, then an extra copy of this file will be saved in that directory.

### `uuid`

If a UUID was provided by the <a undefined>--incremental-history-uuid</a> option, then it will be saved here. Otherwise, this will be the empty string.

### `name`

If a name was provided by the <a undefined>--history</a> or the <a undefined>---incremental-history-name</a> options, then it will be saved here. Otherwise, this will be the empty string.

### `tool_name`

The name of the Mariabackup executable that performed the backup. This is generally `mariabackup`.

### `tool_command`

The arguments that were provided to Mariabackup when it performed the backup.

### `tool_version`

The version of Mariabackup that performed the backup.

### `ibbackup_version`

The version of Mariabackup that performed the backup.

### `server_version`

The version of MariaDB Server that was backed up.

### `start_time`

The time that the backup started.

### `end_time`

The time that the backup ended.

### `lock_time`

The amount of time that Mariabackup held its locks.

### `binlog_pos`

This field stores the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) file name and position that corresponds to the backup.

This field also stores the value of the <a undefined>gtid_current_pos</a> system variable that correspond to the backup.

The values in this field are only guaranteed to be consistent with the backup if the <a undefined>--no-lock</a> option was <strong>not</strong> provided when the backup was taken.

### `innodb_from_lsn`

This is identical to `from_lsn` in <a undefined>xtrabackup_checkpoints</a>.

If the backup is a [full backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup/), then `innodb_from_lsn` has the value of `0`.

If the backup is an [incremental backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/incremental-backup-and-restore-with-mariabackup/), then `innodb_from_lsn` has the value of the [log sequence number (LSN)](/kb/en/innodb-redo-log/#log-sequence-number-lsn) at which the backup started reading from the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/).

### `innodb_to_lsn`

This is identical to `to_lsn` in <a undefined>xtrabackup_checkpoints</a>.

`innodb_to_lsn` has the value of the [log sequence number (LSN)](/kb/en/innodb-redo-log/#log-sequence-number-lsn) of the last checkpoint in the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/).

### `partial`

If the backup is a [partial backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup/), then this value will be `Y`.

Otherwise, this value will be `N`.

### `incremental`

If the backup is an [incremental backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/incremental-backup-and-restore-with-mariabackup/), then this value will be `Y`.

Otherwise, this value will be `N`.

### `format`

This field's value is the format of the backup.

If the <a undefined>--stream</a> option was set to `xbstream`, then this value will be `xbstream`.

If the <a undefined>--stream</a> option was <strong>not</strong> provided, then this value will be `file`.

### `compressed`

If the <a undefined>--compress</a> option was provided, then this value will be `compressed`.

Otherwise, this value will be `N`.

## `xtrabackup_slave_info`

If the <a undefined>--slave-info</a> option is provided, then this file contains the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) command that can be used to set up a new server as a slave of the original server's master after the backup has been restored.

Mariabackup does <strong>not</strong> check if [GTIDs](/replication/standard-replication/gtid/) are being used in replication. It takes a shortcut and assumes that if the <a undefined>gtid_slave_pos</a> system variable is non-empty, then it writes the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) command with the <a undefined>MASTER_USE_GTID</a> option set to `slave_pos`. Otherwise, it writes the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) command with the <a undefined>MASTER_LOG_FILE</a> and <a undefined>MASTER_LOG_POS</a> options using the master's [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) file and position. See [MDEV-19264](https://jira.mariadb.org/browse/MDEV-19264) for more information.

## `xtrabackup_galera_info`

If the <a undefined>--galera-info</a> option is provided, then this file contains information about a [Galera Cluster](/kb/en/galera/) node's state.

The file contains the values of the <a undefined>wsrep_local_state_uuid</a> and <a undefined>wsrep_last_committed</a> status variables.

The values are written in the following format:

```sql
wsrep_local_state_uuid:wsrep_last_committed
```

For example:

```sql
d38587ce-246c-11e5-bcce-6bbd0831cc0f:1352215
```

## `&lt;table&gt;.delta`

If the backup is an [incremental backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/incremental-backup-and-restore-with-mariabackup/), then this file contains changed pages for the table.

## `&lt;table&gt;.delta.meta`

If the backup is an [incremental backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/incremental-backup-and-restore-with-mariabackup/), then this file contains metadata about `&lt;table&gt;.delta` files. The fields in this file are listed below.

### `page_size`

This field contains either the value of <a undefined>innodb_page_size</a> or the value of the <a undefined>KEY_BLOCK_SIZE</a> table option for the table if the <a undefined>ROW_FORMAT</a> table option for the table is set to <a undefined>COMPRESSED</a>.

### `zip_size`

If the <a undefined>ROW_FORMAT</a> table option for this table is set to <a undefined>COMPRESSED</a>, then this field contains the value of the compressed page size.

### `space_id`

This field contains the value of the table's `space_id`.