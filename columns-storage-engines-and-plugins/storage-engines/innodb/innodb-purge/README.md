# InnoDB Purge

When a transaction updates a row in an InnoDB table, InnoDB's MVCC implementation keeps old versions of the row in the [InnoDB undo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log/). The old versions are kept at least until all transactions older than the transaction that updated the row are no longer open. At that point, the old versions can be deleted. InnoDB has purge process that is used to delete these old versions.

## Optimizing Purge Performance

### Configuring the Purge Threads

The number of purge threads can be set by configuring the <a undefined>innodb_purge_threads</a> system variable. This system variable can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
innodb_purge_threads = 6
```

### Configuring the Purge Batch Size

The purge batch size is defined as the number of [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) records that must be written before triggering purge. The purge batch size can be set by configuring the <a undefined>innodb_purge_batch_size</a> system variable. This system variable can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
innodb_purge_batch_size = 50
```

### Configuring the Max Purge Lag

If purge operations are lagging on a busy server, then this can be a tough situation to recover from. As a solution, InnoDB allows you to set the max purge lag. The max purge lag is defined as the maximum number of [InnoDB undo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log/) that can be waiting to be purged from the history until InnoDB begins delaying DML statements.

The max purge lag can be set by configuring the <a undefined>innodb_max_purge_lag</a> system variable. This system variable can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL innodb_max_purge_lag=1000;
```

This system variable can also be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
innodb_max_purge_lag = 1000
```

The maximum delay can be set by configuring the <a undefined>innodb_max_purge_lag_delay</a> system variable. This system variable can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL innodb_max_purge_lag_delay=100;
```

This system variable can also be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
innodb_max_purge_lag_delay = 100
```

### Configuring the Purge Rollback Segment Truncation Frequency

##### MariaDB starting with [10.2.2](/kb/en/mariadb-1022-release-notes/)

The <a undefined>innodb_purge_rseg_truncate_frequency</a> system variable was first added in [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/).

The purge rollback segment truncation frequency is defined as the number of purge loops that are run before unnecessary rollback segments are truncated. The purge rollback segment truncation frequency can be set by configuring the <a undefined>innodb_purge_rseg_truncate_frequency</a> system variable. This system variable can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL innodb_purge_rseg_truncate_frequency=64;
```

This system variable can also be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
innodb_purge_rseg_truncate_frequency = 64
```

### Configuring the Purge Undo Log Truncation

##### MariaDB starting with [10.2.2](/kb/en/mariadb-1022-release-notes/)

The <a undefined>innodb_undo_log_truncate</a> and <a undefined>innodb_max_undo_log_size</a> system variables were first added in [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/).

Purge undo log truncation occurs when InnoDB truncates an entire [InnoDB undo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log/) tablespace, rather than deleting individual [InnoDB undo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log/) records.

Purge undo log truncation can be enabled by configuring the <a undefined>innodb_undo_log_truncate</a> system variable. This system variable can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL innodb_undo_log_truncate=ON;
```

This system variable can also be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
innodb_undo_log_truncate = ON
```

An [InnoDB undo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log/) tablespace is truncated when it exceeds the maximum size that is configured for [InnoDB undo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log/) tablespaces. The maximum size can be set by configuring the <a undefined>innodb_max_undo_log_size</a> system variable. This system variable can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL innodb_max_undo_log_size='64M';
```

This system variable can also be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
innodb_max_undo_log_size = 64M
```

## Purge's Effect on Row Metadata

An InnoDB table's clustered index has three hidden system columns that are automatically generated. These hidden system columns are:

- `DB_ROW_ID` - If the table has no other `PRIMARY KEY` or no other `UNIQUE KEY` defined as `NOT NULL` that can be promoted to the table's  `PRIMARY KEY`, then InnoDB will use a hidden system column called `DB_ROW_ID`. InnoDB will automatically generated the value for the column from a global InnoDB-wide 48-bit sequence (instead of being table-local).
- `DB_TRX_ID` - The transaction ID of either the transaction that last changed the row or the transaction that currently has the row locked.
- `DB_ROLL_PTR` - A pointer to the [InnoDB undo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log/) that contains the row's previous record. The value of `DB_ROLL_PTR` is only valid if `DB_TRX_ID` belongs to the current read view. The oldest valid read view is the purge view.

If a row's last [InnoDB undo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log/) record is purged, this can obviously effect the value of the row's `DB_ROLL_PTR` column, because there would no longer be any [InnoDB undo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log/) record for the pointer to reference.

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and before, the purge process wouldn't touch the value of the row's `DB_TRX_ID` column.

However, in [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and later, the purge process will set a row's `DB_TRX_ID` column to `0` after all of the row's associated [InnoDB undo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log/) records have been deleted. This change allows InnoDB to perform an optimization: if a query wants to read a row, and if the row's `DB_TRX_ID` column is set to `0`, then it knows that no other transaction has the row locked. Usually, InnoDB needs to lock the transaction system's mutex in order to safely check whether a row is locked, but this optimization allows InnoDB to confirm that the row can be safely read without any heavy internal locking.

This optimization can speed up reads, but it come at a noticeable cost at other times. For example, it can cause the purge process to use more I/O after inserting a lot of rows, since the value of each row's `DB_TRX_ID` column will have to be reset.