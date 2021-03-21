# Mariabackup Options

There are a number of options available in `Mariabackup`.

## List of Options

### `--apply-log`

Prepares an existing backup to restore to the MariaDB Server. This is only valid in `innobackupex` mode, which can be enabled with the <a undefined>--innobackupex</a> option.

Files that Mariabackup generates during <a undefined>--backup</a> operations in the target directory are not ready for use on the Server.  Before you can restore the data to MariaDB, you first need to prepare the backup.

In the case of full backups, the files are not point in time consistent, since they were taken at different times.  If you try to restore the database without first preparing the data, InnoDB rejects the new data as corrupt.  Running Mariabackup with the `--prepare` command readies the data so you can restore it to MariaDB Server.  When working with incremental backups, you need to use the `--prepare` command and the <a undefined>--incremental-dir</a> option to update the base backup with the deltas from an incremental backup.

```sql
$ mariabackup --innobackupex --apply-log
```

Once the backup is ready, you can use the <a undefined>--copy-back</a> or the <a undefined>--move-back</a> commands to restore the backup to the server.

### `--apply-log-only`

If this option is used when preparing a backup, then only the redo log apply stage will be performed, and other stages of crash recovery will be ignored. This option is used with [incremental backups](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/incremental-backup-and-restore-with-mariabackup).

This option is only supported in [MariaDB 10.1](/kb/en/what-is-mariadb-101/). In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this option is not needed or supported.

### `--backup`

Backs up your databases.

Using this command option, Mariabackup performs a backup operation on your database or databases.  The backups are written to the target directory, as set by the <a undefined>--target-dir</a> option.

```sql
$ mariabackup --backup 
      --target-dir /path/to/backup \
      --user user_name --password user_passwd
```

Mariabackup can perform full and incremental backups.  A full backup creates a snapshot of the database in the target directory.  An incremental backup checks the database against a previously taken full backup, (defined by the <a undefined>--incremental-basedir</a> option) and creates delta files for these changes.

In order to restore from a backup, you first need to run Mariabackup with the <a undefined>--prepare</a> command option, to make a full backup point-in-time consistent or to apply incremental backup deltas to base.  Then you can run Mariabackup again with either the <a undefined>--copy-back</a> or <a undefined>--move-back</a> commands to restore the database.

For more information, see [Full Backup and Restore](/kb/en/full-backup-and-restore-with-mariadb-backup/) and [Incremental Backup and Restore](/kb/en/incremental-backup-and-restore-with-mariadb-backup/).

### `--binlog-info`

Defines how Mariabackup retrieves the binary log coordinates from the server.

```sql
--binlog-info[=OFF | ON | LOCKLESS | AUTO]
```

The `--binlog-info` option supports the following retrieval methods.  When no retrieval method is provided, it defaults to `AUTO`.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>OFF</code></td><td>Disables the retrieval of binary log information</td></tr>
<tr><td><code>ON</code></td><td>Enables the retrieval of binary log information, performs locking where available to ensure consistency</td></tr>
<tr><td><code>LOCKLESS</code></td><td>Unsupported option</td></tr>
<tr><td><code>AUTO</code></td><td>Enables the retrieval of binary log information using <code>ON</code> or <code>LOCKLESS</code> where supported</td></tr>
</tbody></table>

Using this option, you can control how Mariabackup retrieves the server's binary log coordinates corresponding to the backup.

When enabled, whether using `ON` or `AUTO`, Mariabackup retrieves information from the binlog during the backup process.  When disabled with `OFF`, Mariabackup runs without attempting to retrieve binary log information.  You may find this useful when you need to copy data without metadata like the binlog or replication coordinates.

```sql
$ mariabackup --binlog-info --backup
```

Currently, the `LOCKLESS` option depends on features unsupported by MariaDB Server.  See the description of the <a undefined>xtrabackup_binlog_pos_innodb</a> file for more information. If you attempt to run Mariabackup with this option, then it causes the utility to exit with an error.

### `--close-files`

Defines whether you want to close file handles.

Using this option, you can tell Mariabackup that you want to close file handles.  Without this option, Mariabackup keeps files open in order to manage DDL operations.  When working with particularly large tablespaces, closing the file can make the backup more manageable.  However, it can also lead to inconsistent backups. Use at your own risk.

```sql
$ mariabackup --close-files --prepare
```

### `--compress`

Defines the compression algorithm for backup files. Deprecated. It is recommended to backup to stream (stdout), and use a 3rd party compression library to compress the stream, as described in 
[Using Encryption and Compression Tools With Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/using-encryption-and-compression-tools-with-mariabackup).

```sql
--compress[=compression_algorithm]
```

The `--compress` option supports the following algorithms.  When no algorithm is provided, it defaults to `quicklz`.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>quicklz</code></td><td>Uses the QuickLZ compression algorithm</td></tr>
</tbody></table>

Using this option, you can tell Mariabackup to compress its backup files before writing them to disk.  You may find this useful when backing up particularly large databases.

You can optionally pass a value to this option, defining what compression algorithm you want to use for this process.  However, currently, Mariabackup only supports the QuickLZ algorithm, which is the default value.

```sql
$ mariabackup --compress --backup
```

To further configure backup compression, see the <a undefined>--compress-threads</a> and <a undefined>--compress-chunk-size</a> options.

If a backup is compressed, then Mariabackup will record that detail in the <a undefined>xtrabackup_info</a> file.

### `--compress-chunk-size`

Defines the working buffer size for compression threads.

```sql
--compress-chunk-size=#
```

Mariabackup can perform compression operations on the backup files before writing them to disk.  It can also use multiple threads for parallel data compression during this process.  Using this option, you can set the chunk size each thread uses during compression.  It defaults to 64K.

```sql
$ mariabackup --backup --compress \
     --compress-threads=12 --compress-chunk-size=5M
```

To further configure backup compression, see the <a undefined>--compress</a> and <a undefined>--compress-threads</a> options.

### `--compress-threads`

Defines the number of threads to use in compression.

```sql
--compress-threads=#
```

Mariabackup can perform compression operations on the backup files before writing them to disk.  Using this option, you can define the number of threads you want to use for this operation.  You may find this useful in speeding up the compression of particularly large databases.  It defaults to single-threaded.

```sql
$ mariabackup --compress --compress-threads=12 --backup
```

To further configure backup compression, see the <a undefined>--compress</a> and <a undefined>--compress-chunk-size</a> options.

### `--copy-back`

Restores the backup to the data directory.

Using this command, Mariabackup copies the backup from the target directory to the data directory, as defined by the <a undefined>--datadir</a> option. You must stop the MariaDB Server before running this command. The data directory must be empty. If you want to overwrite the data directory with the backup, use the <a undefined>--force-non-empty-directories</a> option.

Bear in mind, before you can restore a backup, you first need to run Mariabackup with the <a undefined>--prepare</a> option.  In the case of full backups, this makes the files point-in-time consistent.  With incremental backups, this applies the deltas to the base backup.  Once the backup is prepared, you can run `--copy-back` to apply it to MariaDB Server.

```sql
$ mariabackup --copy-back --force-non-empty-directories
```

Running the `--copy-back` command copies the backup files to the data directory.  Use this command if you want to save the backup for later.  If you don't want to save the backup for later, use the <a undefined>--move-back</a> command.

### `--core-file`

Defines whether to write a core file.

Using this option, you can configure Mariabackup to dump its core to file in the event that it encounters fatal signals.  You may find this useful for review and debugging purposes.

```sql
$ mariabackup --core-file --backup
```

### `--databases`

Defines the databases and tables you want to back up.

```sql
--databases="database[.table][ database[.table] ...]"
```

Using this option, you can define the specific database or databases you want to back up.  In cases where you have a particularly large database or otherwise only want to back up a portion of it, you can optionally also define the tables on the database.

```sql
$ mariabackup --backup \
      --databases="example.table1 example.table2"
```

In cases where you want to back up most databases on a server or tables on a database, but not all, you can set the specific databases or tables you don't want to back up using the <a undefined>--databases-exclude</a> option.

If a backup is a [partial backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup), then Mariabackup will record that detail in the <a undefined>xtrabackup_info</a> file.

In `innobackupex` mode, which can be enabled with the <a undefined>--innobackupex</a> option, the `--databases` option can be used as described above, or it can be used to refer to a file, just as the <a undefined>--databases-file</a> option can in the normal mode.

### `--databases-exclude`

Defines the databases you don't want to back up.

```sql
--databases-exclude="database[.table][ database[.table] ...]"
```

Using this option, you can define the specific database or databases you want to exclude from the backup process.  You may find it useful when you want to back up most databases on the server or tables on a database, but would like to exclude a few from the process.

```sql
$ mariabackup --backup \
      --databases="example" \
      --databases-exclude="example.table1 example.table2"
```

To include databases in the backup, see the <a undefined>--databases</a> option option

If a backup is a [partial backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup), then Mariabackup will record that detail in the <a undefined>xtrabackup_info</a> file.

### `--databases-file`

Defines the path to a file listing databases and/or tables you want to back up.

```sql
--databases-file="/path/to/database-file"
```

Format the databases file to list one element per line, with the following syntax:

```sql
database[.table]
```

In cases where you need to back up a number of databases or specific tables in a database, you may find the syntax for the <a undefined>--databases</a> and <a undefined>--databases-exclude</a> options a little cumbersome.  Using this option you can set the path to a file listing the databases or databases and tables you want to back up.

For instance, imagine you list the databases and tables for a backup in a file called `main-backup`.

```sql
$ cat main-backup
example1
example2.table1
example2.table2

$ mariabackup --backup --databases-file=main-backup
```

If a backup is a [partial backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup), then Mariabackup will record that detail in the <a undefined>xtrabackup_info</a> file.

### `-h, --datadir`

Defines the path to the database root.

```sql
--datadir=PATH
```

Using this option, you can define the path to the source directory.  This is the directory that Mariabackup reads for the data it backs up.  It should be the same as the MariaDB Server <a undefined>datadir</a> system variable.

```sql
$ mariabackup --backup -h /var/lib64/mysql
```

### `--debug-sleep-before-unlock`

This is a debug-only option used by the Xtrabackup test suite.

### `--decompress`

Defines whether you want to decompress previously compressed backup files. Deprecated. It is recommended to backup to stream (stdout), and use a 3rd party compression library to compress the stream, as described in 
[Using Encryption and Compression Tools With Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/using-encryption-and-compression-tools-with-mariabackup).

When you run Mariabackup with the <a undefined>--compress</a> option, it compresses the subsequent backup files, using the QuickLZ algorithm by default, (which is currently the only available compression algorithm).  Using this option, Mariabackup decompresses the compressed files from a previous backup.

For instance, run a backup with compression,

```sql
$ mariabackup --compress --backup
```

Then decompress the backup,

```sql
$ mariabackup --decompress
```

You can enable the decryption of multiple files at a time using the <a undefined>--parallel</a> option. By default, Mariabackup does not remove the compressed files from the target directory. If you want to delete these files, use the <a undefined>--remove-original</a> option.

This option requires that you have the `qpress` utility installed on your system.

### `--debug-sync`

Defines the debug sync point.  This option is only used by the Mariabackup test suite.

### `--defaults-extra-file`

Defines the path to an extra default [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

```sql
--defaults-extra-file=/path/to/config
```

Using this option, you can define an extra default [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) for Mariabackup.  Unlike <a undefined>--defaults-file</a>, this file is read after the default [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) are read, allowing you to only overwrite the existing defaults.

```sql
$ mariabackup --backup \
      --defaults-file-extra=addition-config.cnf \
      --defaults-file=config.cnf
```

### `--defaults-file`

Defines the path to the default [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

```sql
--defaults-file=/path/to/config
```

Using this option, you can define a default [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) for Mariabackup.  Unlike the <a undefined>--defaults-extra-file</a> option, when this option is provided, it completely replaces all default [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

```sql
$ mariabackup --backup \
     --defaults-file="config.cnf
```

### `--defaults-group`

Defines the [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) to read in the [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

```sql
--defaults-group="name"
```

In situations where you find yourself using certain Mariabackup options consistently every time you call it, you can set the options in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).  The `--defaults-group` option defines what option group Mariabackup reads for its options.

Options you define from the command-line can be set in the configuration file using minor formatting changes.  For instance, if you find yourself perform compression operations frequently, you might set <a undefined>--compress-threads</a> and <a undefined>--compress-chunk-size</a> options in this way:

```sql
[mariabackup]
compress_threads = 12
compress_chunk_size = 64K
```

Now whenever you run a backup with the <a undefined>--compress</a> option, it always performs the compression using 12 threads and 64K chunks.

```sql
$ mariabackup --compress --backup
```

See [Mariabackup Overview: Server Option Groups](/kb/en/mariabackup-overview/#server-option-groups) and [Mariabackup Overview: Client Option Groups](/kb/en/mariabackup-overview/#client-option-groups) for a list of the option groups read by Mariabackup by default.

### `--encrypted-backup`

When this option is used with `--backup`, if Mariabackup encounters a page that has a non-zero `key_version` value, then Mariabackup assumes that the page is encrypted.

Use `--skip-encrypted-backup` instead to allow Mariabackup to copy unencrypted tables that were originally created before MySQL 5.1.48.

This option was added in [MariaDB 10.2.22](/kb/en/mariadb-10222-release-notes/), [MariaDB 10.3.13](/kb/en/mariadb-10313-release-notes/), and [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/).

### `--export`

If this option is provided during the `--prepare` stage, then it tells Mariabackup to create `.cfg` files for each [InnoDB file-per-table tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces). These `.cfg` files are used to [import transportable tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces) in the process of [restoring partial backups](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup) and [restoring individual tables and partitions](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/restoring-individual-tables-and-partitions-with-mariabackup). .

```sql
$ mariabackup --prepare --export
```

##### MariaDB until [10.2.8](/kb/en/mariadb-1028-release-notes/)

In [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/) and before, Mariabackup did not support the <a undefined>--export</a> option. See [MDEV-13466](https://jira.mariadb.org/browse/MDEV-13466) about that. In earlier versions of MariaDB, this means that Mariabackup could not create `.cfg` files for [InnoDB file-per-table tablespaces](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces) during the `--prepare` stage. You can still [import file-per-table tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces) without the `.cfg` files in many cases, so it may still be possible in those versions to [restore partial backups](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup) or to [restore individual tables and partitions](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/restoring-individual-tables-and-partitions-with-mariabackup) with just the `.ibd` files. If you have a [full backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup) and you need to create `.cfg` files for [InnoDB file-per-table tablespaces](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces), then you can do so by preparing the backup as usual without the `--export` option, and then restoring the backup, and then starting the server. At that point, you can use the server's built-in features to [copy the transportable tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces).

### `--extra-lsndir`

Saves an extra copy of the <a undefined>xtrabackup_checkpoints</a> and <a undefined>xtrabackup_info</a> files into the given directory.

```sql
--extra-lsndir=PATH
```

When using the <a undefined>--backup</a> command option, Mariabackup produces a number of backup files in the target directory.  Using this option, you can have Mariabackup produce additional copies of the <a undefined>xtrabackup_checkpoints</a>  and <a undefined>xtrabackup_info</a># files in the given directory.

```sql
$ mariabackup --extra-lsndir=extras/ --backup
```

### `--force-non-empty-directories`

Allows <a undefined>--copy-back</a> or <a undefined>--move-back</a> command options to use non-empty target directories.

When using Mariabackup with the <a undefined>--copy-back</a> or <a undefined>--move-back</a> command options, they normally require a non-empty target directory to avoid conflicts.  Using this option with either of command allows Mariabackup to use a non-empty directory.

```sql
$ mariabackup --force-on-empty-directories --copy-back
```

Bear in mind that this option does not enable overwrites.  When copying or moving files into the target directory, if Mariabackup finds that the target file already exists, it fails with an error.

### `--ftwrl-wait-query-type`

Defines the type of query allowed to complete before Mariabackup issues the global lock.

```sql
--ftwrl-wait-query-type=[ALL | UPDATE | SELECT]
```

The `--ftwrl-wait-query-type` option supports the following query types.  The default value is `ALL`.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>ALL</code></td><td>Waits until all queries complete before issuing the global lock</td></tr>
<tr><td><code>SELECT</code></td><td>Waits until <code><a href="/kb/en/select/">SELECT</a></code> statements complete before issuing the global lock</td></tr>
<tr><td><code>UPDATE</code></td><td>Waits until <code><a href="/kb/en/update/">UPDATE</a></code> statements complete before issuing the global lock</td></tr>
</tbody></table>

When Mariabackup runs, it issues a global lock to prevent data from changing during the backup process.  When it encounters a statement in the process of executing, it waits until the statement is finished before issuing the global lock. Using this option, you can modify this default behavior to ensure that it waits only for certain query types, such as for [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) and [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) statements.

```sql
$ mariabackup --backup  \
      --ftwrl-wait-query-type=UPDATE
```

### `--ftwrl-wait-threshold`

Defines the minimum threshold for identifying long-running queries for FTWRL.

```sql
--ftwrl-wait-threshold=#
```

When Mariabackup runs, it issues a global lock to prevent data from changing during the backup process and ensure a consistent record.  If it encounters statements still in the process of executing, it waits until they complete before setting the lock.  Using this option, you can set the threshold at which Mariabackup engages FTWRL. When it <a undefined>--ftwrl-wait-timeout</a> is not 0 and a statement has run for at least the amount of time given this argument, Mariabackup waits until the statement completes or until
the <a undefined>--ftwrl-wait-timeout</a> expires before setting the global lock and starting the backup.

```sql
$ mariabackup --backup \
     --ftwrl-wait-timeout=90 \
     --ftwrl-wait-threshold=30
```

### `--ftwrl-wait-timeout`

Defines the timeout to wait for queries before trying to acquire the global lock. In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, the global lock refers to <a undefined>BACKUP STAGE BLOCK_COMMIT</a>. In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, the global lock refers to <a undefined>FLUSH TABLES WITH READ LOCK (FTWRL)</a>.

```sql
--ftwrl-wait-timeout=#
```

When Mariabackup runs, it acquires a global lock to prevent data from changing during the backup process and ensure a consistent record. If it encounters statements still in the process of executing, it can be configured to wait until the statements complete before trying to acquire the global lock.

If the `--ftwrl-wait-timeout` is set to 0, then Mariabackup tries to acquire the global lock immediately without waiting. This is the default value.

If the `--ftwrl-wait-timeout` is set to a non-zero value, then Mariabackup waits for the configured number of seconds until trying to acquire the global lock.

Starting in [MariaDB 10.5.3](/kb/en/mariadb-1053-release-notes/), [MariaDB 10.4.13](/kb/en/mariadb-10413-release-notes/), [MariaDB 10.3.23](/kb/en/mariadb-10323-release-notes/), and [MariaDB 10.2.32](/kb/en/mariadb-10232-release-notes/), Mariabackup will exit if it can't acquire the global lock after waiting for the configured number of seconds. In earlier versions, it could wait for the global lock indefinitely, even if `--ftwrl-wait-timeout` was set to a non-zero value.

```sql
$ mariabackup --backup \
      --ftwrl-wait-query-type=UPDATE \
      --ftwrl-wait-timeout=5
```

### `--galera-info`

Defines whether you want to back up information about a [Galera Cluster](/kb/en/galera/) node's state.

When this option is used, Mariabackup creates an additional file called <a undefined>xtrabackup_galera_info</a>, which records information about a [Galera Cluster](/kb/en/galera/) node's state. It records the values of the <a undefined>wsrep_local_state_uuid</a> and <a undefined>wsrep_last_committed</a> status variables.

You should only use this option when backing up a [Galera Cluster](/kb/en/galera/) node. If the server is not a [Galera Cluster](/kb/en/galera/) node, then this option has no effect.

```sql
$ mariabackup --backup --galera-info
```

### `--history`

Defines whether you want to track backup history in the `PERCONA_SCHEMA.xtrabackup_history` table.

```sql
--history[=name]
```

When using this option, Mariabackup records its operation in a table on the MariaDB Server.  Passing a name to this option allows you group backups under arbitrary terms for later processing and analysis.

```sql
$ mariabackup --backup --history=backup_all
```

Currently, the table it uses is named `PERCONA_SCHEMA.xtrabackup_history`, but expect that name to change in future releases. See [MDEV-19246](https://jira.mariadb.org/browse/MDEV-19246) for more information.

Mariabackup will also record this in the <a undefined>xtrabackup_info</a> file.

### `-H, --host`

Defines the host for the MariaDB Server you want to backup.

```sql
--host=name
```

Using this option, you can define the host to use when connecting to a MariaDB Server over TCP/IP.  By default, Mariabackup attempts to connect to the local host.

```sql
$ mariabackup --backup \
      --host="example.com"
```

### `--include`

This option is a regular expression to be matched against table names in databasename.tablename format. It is
equivalent to the <a undefined>--tables</a> option. This is only valid in `innobackupex` mode, which can be enabled with the <a undefined>--innobackupex</a> option.

### `--incremental`

Defines whether you want to take an increment backup, based on another backup. This is only valid in `innobackupex` mode, which can be enabled with the <a undefined>--innobackupex</a> option.

```sql
mariabackup --innobackupex --incremental
```

Using this option with the <a undefined>--backup</a> command option makes the operation incremental rather than a complete overwrite. When this option is specified, either the <a undefined>--incremental-lsn</a> or [--incremental-basedir](#-incremental-basedir)` options can also be given. If neither option is given, option `[--incremental-basedir](#-incremental-basedir)` is used by default, set to the first timestamped backup directory in the backup base directory.`

```sql
$ mariabackup --innobackupex --backup --incremental \
     --incremental-basedir=/data/backups \
     --target-dir=/data/backups
```

If a backup is a [incremental backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/incremental-backup-and-restore-with-mariabackup), then Mariabackup will record that detail in the <a undefined>xtrabackup_info</a> file.

### `--incremental-basedir`

Defines whether you want to take an incremental backup, based on another backup.

```sql
--incremental-basedir=PATH
```

Using this option with the <a undefined>--backup</a> command option makes the operation incremental rather than a complete overwrite. Mariabackup will only copy pages from `.ibd` files if they are newer than the backup in the specified directory.

```sql
$ mariabackup --backup \
     --incremental-basedir=/data/backups \
     --target-dir=/data/backups
```

If a backup is a [incremental backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/incremental-backup-and-restore-with-mariabackup), then Mariabackup will record that detail in the <a undefined>xtrabackup_info</a> file.

### `--incremental-dir`

Defines whether you want to take an incremental backup, based on another backup.

```sql
--increment-dir=PATH
```

Using this option with <a undefined>--prepare</a> command option makes the operation incremental rather than a complete overwrite. Mariabackup will apply `.delta` files and log files into the target directory.

```sql
$ mariabackup --prepare \
      --increment-dir=backups/
```

If a backup is a [incremental backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/incremental-backup-and-restore-with-mariabackup), then Mariabackup will record that detail in the <a undefined>xtrabackup_info</a> file.

### `--incremental-force-scan`

Defines whether you want to force a full scan for incremental backups.

When using Mariabackup to perform an incremental backup, this option forces it to also perform a full scan of the data pages being backed up, even when there's bitmap data on the changes. [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later does not support changed page bitmaps, so this option is useless in those versions. See [MDEV-18985](https://jira.mariadb.org/browse/MDEV-18985) for more information.

```sql
$ mariabackup --backup \
     --incremental-basedir=/path/to/target \
     --incremental-force-scan
```

### `--incremental-history-name`

Defines a logical name for the backup.

```sql
--incremental-history-name=name
```

Mariabackup can store data about its operations on the MariaDB Server.  Using this option, you can define the logical name it uses in identifying the backup.

```sql
$ mariabackup --backup \
     --incremental-history-name=morning_backup
```

Currently, the table it uses is named `PERCONA_SCHEMA.xtrabackup_history`, but expect that name to change in future releases. See [MDEV-19246](https://jira.mariadb.org/browse/MDEV-19246) for more information.

Mariabackup will also record this in the <a undefined>xtrabackup_info</a> file.

### `--incremental-history-uuid`

Defines a UUID for the backup.

```sql
--incremental-history-uuid=name
```

Mariabackup can store data about its operations on the MariaDB Server.  Using this option, you can define the UUID it uses in identifying a previous backup to increment from.  It checks <a undefined>--incremental-history-name</a>, <a undefined>--incremental-basedir</a>, and <a undefined>--incremental-lsn</a>.  If Mariabackup fails to find a valid lsn, it generates an error.

```sql
$ mariabackup --backup \
      --incremental-history-uuid=main-backup012345678
```

Currently, the table it uses is named `PERCONA_SCHEMA.xtrabackup_history`, but expect that name to change in future releases. See [MDEV-19246](https://jira.mariadb.org/browse/MDEV-19246) for more information.

Mariabackup will also record this in the <a undefined>xtrabackup_info</a> file.

### `--incremental-lsn`

Defines the sequence number for incremental backups.

```sql
--incremental-lsn=name
```

Using this option, you can define the sequence number (LSN) value for <a undefined>--backup</a> operations.  During backups, Mariabackup only copies `.ibd` pages newer than the specified values.

<strong>WARNING</strong>: Incorrect LSN values can make the backup unusable.  It is impossible to diagnose this issue.

### `--innobackupex`

Enables `innobackupex` mode, which is a compatibility mode.

```sql
$ mariabackup --innobackupex
```

In `innobackupex` mode, Mariabackup has the following differences:

- To prepare a backup, the <a undefined>--apply-log</a> option is used instead of the <a undefined>--prepare</a> option.
- To create an [incremental backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/incremental-backup-and-restore-with-mariabackup), the <a undefined>--incremental</a> option is supported.
- The <a undefined>--no-timestamp</a> option is supported.
- To create a [partial backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup), the <a undefined>--include</a> option is used instead of the <a undefined>--tables</a> option.
- To create a [partial backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup), the <a undefined>--databases</a> option can still be used, but it's behavior changes slightly.
- The <a undefined>--target-dir</a> option is not used to specify the backup directory. The backup directory should instead be specified as a standalone argument.

The primary purpose of `innobackupex` mode is to allow scripts and tools to more easily migrate to Mariabackup if they were originally designed to use the `innobackupex` utility that is included with [Percona XtraBackup](/mariadb-administration/backing-up-and-restoring-databases/backing-up-and-restoring-databases-percona-xtrabackup/percona-xtrabackup-overview). It is not recommended to use this mode in new scripts, since it is not guaranteed to be supported forever. See [MDEV-20552](https://jira.mariadb.org/browse/MDEV-20552) for more information.

### `--innodb`

This option has no effect.  Set only for MySQL option compatibility.

### `--innodb-adaptive-hash-index`

Enables InnoDB Adaptive Hash Index.

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option you can explicitly enable the InnoDB Adaptive Hash Index.  This feature is enabled by default for Mariabackup.  If you want to disable it, use <a undefined>--skip-innodb-adaptive-hash-index</a>.

```sql
$ mariabackup --backup \
      --innodb-adaptive-hash-index
```

### `--innodb-autoextend-increment`

Defines the increment in megabytes for auto-extending the size of tablespace file.

```sql
--innodb-autoextend-increment=36
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can set the increment in megabytes for automatically extending the size of tablespace data file in InnoDB.

```sql
$ mariabackup --backup \
     --innodb-autoextend-increment=35
```

### `--innodb-buffer-pool-filename`

Using this option has no effect.  It is available to provide compatibility with the MariaDB Server.

### `--innodb-buffer-pool-size`

Defines the memory buffer size InnoDB uses the cache data and indexes of the table.

```sql
--innodb-buffer-pool-size=124M
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can configure the buffer pool for InnoDB operations.

```sql
$ mariabackup --backup \
      --innodb-buffer-pool-size=124M
```

### `--innodb-checksum-algorithm`

Defines the checksum algorithm.

```sql
--innodb-checksum-algorithm=crc32
                           | strict_crc32
                           | innodb
                           | strict_innodb
                           | none
                           | strict_none 
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can specify the algorithm Mariabackup uses when checksumming on InnoDB tables.  Currently, MariaDB supports the following algorithms `CRC32`, `STRICT_CRC32`, `INNODB`, `STRICT_INNODB`, `NONE`, `STRICT_NONE`.

```sql
$ mariabackup --backup \
      ---innodb-checksum-algorithm=strict_innodb
```

### `--innodb-data-file-path`

Defines the path to individual data files.

```sql
--innodb-data-file-path=/path/to/file
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file. Using this option you can define the path to InnoDB data files. Each path is appended to the <a undefined>--innodb-data-home-dir</a> option.

```sql
$ mariabackup --backup \
     --innodb-data-file-path=ibdata1:13M:autoextend \
     --innodb-data-home-dir=/var/dbs/mysql/data
```

### `--innodb-data-home-dir`

Defines the home directory for InnoDB data files.

```sql
--innodb-data-home-dir=PATH
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option you can define the path to the directory containing InnoDB data files.  You can specific the files using the <a undefined>--innodb-data-file-path</a> option.

```sql
$ mariabackup --backup \
     --innodb-data-file-path=ibdata1:13M:autoextend \
     --innodb-data-home-dir=/var/dbs/mysql/data
```

### `--innodb-doublewrite`

Enables doublewrites for InnoDB tables.

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  When using this option, Mariabackup improves fault tolerance on InnoDB tables with a doublewrite buffer.  By default, this feature is enabled.  Use this option to explicitly enable it.  To disable doublewrites, use the <a undefined>--skip-innodb-doublewrite</a> option.

```sql
$ mariabackup --backup \
     --innodb-doublewrite
```

### `--innodb-encrypt-log`

Defines whether you want to encrypt InnoDB logs.

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can tell Mariabackup that you want to encrypt logs from its InnoDB activity.

### `--innodb-file-io-threads`

Defines the number of file I/O threads in InnoDB.

```sql
--innodb-file-io-threads=#
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can define the number of file I/O threads Mariabackup uses on InnoDB tables.

```sql
$ mariabackup --backup \
     --innodb-file-io-threads=5
```

### `--innodb-file-per-table`

Defines whether you want to store each InnoDB table as an `.ibd` file.

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option causes Mariabackup to store each InnoDB table as an `.ibd` file in the target directory.

### `--innodb-flush-method`

Defines the data flush method.

```sql
--innodb-flush-method=fdatasync 
                     | O_DSYNC 
                     | O_DIRECT 
                     | O_DIRECT_NO_FSYNC 
                     | ALL_O_DIRECT
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can define the data flush method Mariabackup uses with InnoDB tables.

```sql
$ mariabackup --backup \
      --innodb-flush-method==_DIRECT_NO_FSYNC
```

Note, the `0_DIRECT_NO_FSYNC` method is only available with [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later.  The `ALL_O_DIRECT` method available with version 5.5 and later, but only with tables using the XtraDB storage engine.

### `--innodb-io-capacity`

Defines the number of IOP's the utility can perform.

```sql
--innodb-io-capacity=#
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can limit the I/O activity for InnoDB background tasks.  It should be set around the number of I/O operations per second that the system can handle, based on drive or drives being used.

```sql
$ mariabackup --backup \
     --innodb-io-capacity=200
```

### `--innodb-log-checksums`

Defines whether to include checksums in the InnoDB logs.

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can explicitly set Mariabackup to include checksums in the InnoDB logs.  The feature is enabled by default.  To disable it, use the <a undefined>--skip-innodb-log-checksums</a> option.

```sql
$ mariabackup --backup \
      --innodb-log-checksums
```

### `--innodb-log-buffer-size`

This option has no functionality in Mariabackup.  It exists for MariaDB Server compatibility.

### `--innodb-log-files-in-group`

This option has no functionality in Mariabackup.  It exists for MariaDB Server compatibility.

### `--innodb-log-group-home-dir`

Defines the path to InnoDB log files.

```sql
--innodb-log-group-home-dir=PATH
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file. Using this option, you can define the path to InnoDB log files.

```sql
$ mariabackup --backup \
     --innodb-log-group-home-dir=/path/to/logs
```

### `--innodb-max-dirty-pages-pct`

Defines the percentage of dirty pages allowed in the InnoDB buffer pool.

```sql
--innodb-max-dirty-pages-pct=#
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can define the maximum percentage of dirty, (that is, unwritten) pages that Mariabackup allows in the InnoDB buffer pool.

```sql
$ mariabackup --backup \
     --innodb-max-dirty-pages-pct=80
```

### `--innodb-open-files`

Defines the number of files kept open at a time.

```sql
--innodb-open-files=#
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can set the maximum number of files InnoDB keeps open at a given time during backups.

```sql
$ mariabackup --backup \
      --innodb-open-files=10
```

### `--innodb-page-size`

Defines the universal page size.

```sql
--innodb-page-size=#
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can define the universal page size in bytes for Mariabackup.

```sql
$ mariabackup --backup \
     --innodb-page-size=16k
```

### `--innodb-read-io-threads`

Defines the number of background read I/O threads in InnoDB.

```sql
--innodb-read-io-threads=#
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can set the number of I/O threads MariaDB uses when reading from InnoDB.

```sql
$ mariabackup --backup \
      --innodb-read-io-threads=4
```

### `--innodb-undo-directory`

Defines the directory for the undo tablespace files.

```sql
--innodb-undo-directory=PATH
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can define the path to the directory where you want MariaDB to store the undo tablespace on InnoDB tables.  The path can be absolute.

```sql
$ mariabackup --backup \
     --innodb-undo-directory=/path/to/innodb_undo
```

### `--innodb-undo-tablespaces`

Defines the number of undo tablespaces to use.

```sql
--innodb-undo-tablespaces=#
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can define the number of undo tablespaces you want to use during the backup.

```sql
$ mariabackup --backup \
      --innodb-undo-tablespaces=10
```

### `--innodb-use-native-aio`

Defines whether you want to use native AI/O.

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can enable the use of the native asynchronous I/O subsystem.  It is only available on Linux operating systems.

```sql
$ mariabackup --backup \
      --innodb-use-native-aio
```

### `--innodb-write-io-threads`

Defines the number of background write I/O threads in InnoDB.

```sql
--innodb-write-io-threads=#
```

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can set the number of background write I/O threads Mariabackup uses.

```sql
$ mariabackup --backup \
     --innodb-write-io-threads=4
```

### `--kill-long-queries-timeout`

Defines the timeout for blocking queries.

```sql
--kill-long-queries-timeout=#
```

When Mariabackup runs, it issues a `FLUSH TABLES WITH READ LOCK` statement.  It then identifies blocking queries.  Using this option you can set a timeout in seconds for these blocking queries.  When the time runs out, Mariabackup kills the queries.

The default value is 0, which causes Mariabackup to not attempt killing any queries.

```sql
$ mariabackup --backup \
      --kill-long-queries-timeout=10
```

### `--kill-long-query-type`

Defines the query type the utility can kill to unblock the global lock.

```sql
--kill-long-query-type=ALL | UPDATE | SELECT
```

When Mariabackup encounters a query that sets a global lock, it can kill the query in order to free up MariaDB Server for the backup.  Using this option, you can choose the types of query it kills: [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update), or both set with `ALL`.  The default is `ALL`.

```sql
$ mariabackup --backup \
      --kill-long-query-type=UPDATE
```

### `--lock-ddl-per-table`

Prevents DDL for each table to be backed up by acquiring MDL lock on that.
NOTE: Unless  --no-lock option was also specified,  conflicting DDL queries , will be killed at the end of backup  This is done avoid deadlock between "FLUSH TABLE WITH READ LOCK", user's DDL query (ALTER, RENAME), and MDL lock on table. Only available in [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/) and later.

### `--log`

This option has no functionality.  It is set to ensure compatibility with MySQL.

### `--log-bin`

Defines the base name for the log sequence.

```sql
--log-bin[=name]
```

Using this option you, you can set the base name for Mariabackup to use in log sequences.

### `--log-copy-interval`

Defines the copy interval between checks done by the log copying thread.

```sql
--log-copy-interval=#
```

Using this option, you can define the copy interval Mariabackup uses between checks done by the log copying thread.  The given value is in milliseconds.

```sql
$ mariabackup --backup \
      --log-copy-interval=50
```

### `--log-innodb-page-corruption`

Continue backup if InnoDB corrupted pages are found. The pages are logged in `innodb_corrupted_pages` and backup is finished with error. [--prepare](#-prepare) will try to fix corrupted pages. If `innodb_corrupted_pages` exists after [--prepare](#-prepare) in base backup directory, backup still contains corrupted pages and can not be considered as consistent.

Added in [MariaDB 10.2.37](/kb/en/mariadb-10237-release-notes/), [MariaDB 10.3.28](/kb/en/mariadb-10328-release-notes/), [MariaDB 10.4.18](/kb/en/mariadb-10418-release-notes/), [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/)

### `--move-back`

Restores the backup to the data directory.

Using this command, Mariabackup moves the backup from the target directory to the data directory, as defined by the <a undefined>--datadir</a> option.  You must stop the MariaDB Server before running this command.  The data directory must be empty.  If you want to overwrite the data directory with the backup, use the <a undefined>--force-non-empty-directories</a> option.

Bear in mind, before you can restore a backup, you first need to run Mariabackup with the <a undefined>--prepare</a> option.  In the case of full backups, this makes the files point-in-time consistent.  With incremental backups, this applies the deltas to the base backup.  Once the backup is prepared, you can run `--move-back` to apply it to MariaDB Server.

```sql
$ mariabackup --move-back \
      --datadir=/var/mysql
```

Running the `--move-back` command moves the backup files to the data directory.  Use this command if you don't want to save the backup for later.  If you do want to save the backup for later, use the <a undefined>--copy-back</a> command.

### `--mysqld`

Used internally to prepare a backup.

### `--no-backup-locks`

Mariabackup locks the database by default when it runs. This option disables support for Percona Server's backup locks.

When backing up Percona Server, Mariabackup would use backup locks by default. To be specific, backup locks refers to the `LOCK TABLES FOR BACKUP` and `LOCK BINLOG FOR BACKUP` statements. This option can be used to disable support for Percona Server's backup locks. This option has no effect when the server does not support Percona's backup locks.

This option may eventually be removed. See [MDEV-19753](https://jira.mariadb.org/browse/MDEV-19753) for more information.

```sql
$ mariabackup --backup --no-backup-locks
```

### `--no-lock`

Disables table locks with the `FLUSH TABLE WITH READ LOCK` statement.

Using this option causes Mariabackup to disable table locks with the `FLUSH TABLE WITH READ LOCK` statement.  Only use this option if:

- You are not executing DML statements on non-InnoDB tables during the backup. This includes the `mysql` database system tables (which are MyISAM).
- You are not executing any DDL statements during the backup.
- You <strong>do not care</strong> if the binary log position included with the backup in <a undefined>xtrabackup_binlog_info</a> is consistent with the data.
- <strong>All</strong> tables you're backing up use the InnoDB storage engine.

```sql
$ mariabackup --backup --no-lock
```

If you're considering `--no-lock` due to backups failing to acquire locks, this may be due to incoming replication events preventing the lock.  Consider using the <a undefined>--safe-slave-backup</a> option to momentarily stop the replication slave thread.  This alternative may help the backup to succeed without resorting to `--no-lock`.

Do not use this option. This option may cause data consistency issues. This option is not supported.

### `--no-timestamp`

This option prevents creation of a time-stamped subdirectory of the BACKUP-ROOT-DIR given on the command line. When it is specified, the backup is done in BACKUP-ROOT-DIR instead. This is only valid in `innobackupex` mode, which can be enabled with the <a undefined>--innobackupex</a> option.

### `--no-version-check`

Disables version check.

Using this option, you can disable Mariabackup version check.  If you would like to enable the version check, use the <a undefined>--version-check</a> option.

```sql
$ mariabackup --backup --no-version-check
```

### `--open-files-limit`

Defines the maximum number of file descriptors.

```sql
--open-files-limit=#
```

Using this option, you can define the maximum number of file descriptors Mariabackup reserves with `setrlimit()`.

```sql
$ mariabackup --backup \
      --open-files-limit=
```

### `--parallel`

Defines the number of threads to use for parallel data file transfer.

```sql
--parallel=#
```

Using this option, you can set the number of threads Mariabackup uses for parallel data file transfers.  By default, it is set to 1.

### `-p, --password`

Defines the password to use to connect to MariaDB Server.

```sql
--password=passwd
```

When you run Mariabackup, it connects to MariaDB Server in order to access and back up the databases and tables.  Using this option, you can set the password Mariabackup uses to access the server.  To set the user, use the <a undefined>--user</a> option.

```sql
$ mariabackup --backup \
      --user=root \
      --password=root_password
```

### `--plugin-dir`

Defines the directory for server plugins.

```sql
--plugin-dir=PATH
```

Using this option, you can define the path Mariabackup reads for MariaDB Server plugins.  It only uses it during the <a undefined>--prepare</a> phase to load the encryption plugin.  It defaults to the <a undefined>plugin_dir</a> server system variable.

```sql
$ mariabackup --backup \
      --plugin-dir=/var/mysql/lib/plugin
```

### `--plugin-load`

Defines the encryption plugins to load.

```sql
--plugin-load=name
```

Using this option, you can define the encryption plugin you want to load.  It is only used during the <a undefined>--prepare</a> phase to load the encryption plugin.  It defaults to the server `--plugin-load` option.

The option was removed starting from [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/)

### `-P, --port`

Defines the server port to connect to.

```sql
--port=#
```

When you run Mariabackup, it connects to MariaDB Server in order to access and back up your databases and tables.  Using this option, you can set the port the utility uses to access the server over TCP/IP. To set the host, see the <a undefined>--host</a> option.  Use `mysql --help` for more details.

```sql
$ mariabackup --backup \
      --host=192.168.11.1 \
      --port=3306
```

### `--prepare`

Prepares an existing backup to restore to the MariaDB Server.

Files that Mariabackup generates during <a undefined>--backup</a> operations in the target directory are not ready for use on the Server.  Before you can restore the data to MariaDB, you first need to prepare the backup.

In the case of full backups, the files are not point in time consistent, since they were taken at different times.  If you try to restore the database without first preparing the data, InnoDB rejects the new data as corrupt.  Running Mariabackup with the `--prepare` command readies the data so you can restore it to MariaDB Server.  When working with incremental backups, you need to use the `--prepare` command and the <a undefined>--incremental-dir</a> option to update the base backup with the deltas from an incremental backup.

```sql
$ mariabackup --prepare
```

Once the backup is ready, you can use the <a undefined>--copy-back</a> or the <a undefined>--move-back</a> commands to restore the backup to the server.

### `--print-defaults`

Prints the utility argument list, then exits.

Using this argument, MariaDB prints the argument list to stdout and then exits.  You may find this useful in debugging to see how the options are set for the utility.

```sql
$ mariabackup --print-defaults
```

### `--print-param`

Prints the MariaDB Server options needed for copyback.

Using this option, Mariabackup prints to stdout the MariaDB Server options that the utility requires to run the <a undefined>--copy-back</a> command option.

```sql
$ mariabackup --print-param
```

### `--rollback-xa`

This is an experimental option. Do not use this option in versions older than [MariaDB 10.2.33](/kb/en/mariadb-10233-release-notes/), [MariaDB 10.3.24](/kb/en/mariadb-10324-release-notes/), and [MariaDB 10.4.14](/kb/en/mariadb-10414-release-notes/). Older implementation can cause corruption of InnoDB data.

### `--rsync`

Defines whether to use rsync.

During normal operation, Mariabackup transfers local non-InnoDB files using a separate call to `cp` for each file.  Using this option, you can optimize this process by performing this transfer with rsync, instead.

```sql
$ mariabackup --backup --rsync
```

This option is not compatible with the <a undefined>--stream</a> option.

### `--safe-slave-backup`

Stops slave SQL threads for backups.

When running Mariabackup on a server that uses replication, you may occasionally encounter locks that block backups.  Using this option, it stops slave SQL threads and waits until the `Slave_open_temp_tables` in the `SHOW STATUS` statement is zero.  If there are no open temporary tables, the backup runs, otherwise the SQL thread starts and stops until there are no open temporary tables.

```sql
$ mariabackup --backup \
      --safe-slave-backup \
      --safe-slave-backup-timeout=500
```

The backup fails if the `Slave_open_temp_tables` doesn't reach zero after the timeout period set by the <a undefined>--safe-slave-backup-timeout</a> option.

### `--safe-slave-backup-timeout`

Defines the timeout for slave backups.

```sql
--safe-slave-backup-timeout=#
```

When running Mariabackup on a server that uses replication, you may occasionally encounter locks that block backups.  With the <a undefined>--safe-slave-backup</a> option, it waits until the `Slave_open_temp_tables` in the `SHOW STATUS` statement reaches zero.  Using this option, you set how long it waits.  It defaults to 300.

```sql
$ mariabackup --backup \
      --safe-slave-backup \
      --safe-slave-backup-timeout=500
```

### `--secure-auth`

Refuses client connections to servers using the older protocol.

Using this option, you can set it explicitly to refuse client connections to the server when using the older protocol, from before 4.1.1.  This feature is enabled by default.  Use the <a undefined>--skip-secure-auth</a> option to disable it.

```sql
$ mariabackup --backup --secure-auth
```

### `--skip-innodb-adaptive-hash-index`

Disables InnoDB Adaptive Hash Index.

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option you can explicitly disable the InnoDB Adaptive Hash Index.  This feature is enabled by default for Mariabackup.  If you want to explicitly enable it, use <a undefined>--innodb-adaptive-hash-index</a>.

```sql
$ mariabackup --backup \
      --skip-innodb-adaptive-hash-index
```

### `--skip-innodb-doublewrite`

Disables doublewrites for InnoDB tables.

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  When doublewrites are enabled, InnoDB improves fault tolerance with a doublewrite buffer.  By default this feature is turned on.  Using this option you can disable it for Mariabackup.  To explicitly enable doublewrites, use the <a undefined>--innodb-doublewrite</a> option.

```sql
$ mariabackup --backup \
     --skip-innodb-doublewrite
```

### `--skip-innodb-log-checksums`

Defines whether to exclude checksums in the InnoDB logs.

Mariabackup initializes its own embedded instance of InnoDB using the same configuration as defined in the configuration file.  Using this option, you can set Mariabackup to exclude checksums in the InnoDB logs.  The feature is enabled by default.  To explicitly enable it, use the <a undefined>--innodb-log-checksums</a> option.

### `--skip-secure-auth`

Refuses client connections to servers using the older protocol.

Using this option, you can set it accept client connections to the server when using the older protocol, from before 4.1.1.  By default, it refuses these connections.  Use the <a undefined>--secure-auth</a> option to explicitly enable it.

```sql
$ mariabackup --backup --skip-secure-auth
```

### `--slave-info`

Prints the binary log position and the name of the master server.

If the server is a [replication slave](/replication), then this option causes Mariabackup to print the hostname of the slave's replication master and the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) file and position of the [slave's SQL thread](/kb/en/replication-threads/#slave-sql-thread) to `stdout`.

This option also causes Mariabackup to record this information as a [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) command that can be used to set up a new server as a slave of the original server's master after the backup has been restored. This information will be written to to the <a undefined>xtrabackup_slave_info</a> file.

Mariabackup does <strong>not</strong> check if [GTIDs](/replication/standard-replication/gtid) are being used in replication. It takes a shortcut and assumes that if the <a undefined>gtid_slave_pos</a> system variable is non-empty, then it writes the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) command with the <a undefined>MASTER_USE_GTID</a> option set to `slave_pos`. Otherwise, it writes the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) command with the <a undefined>MASTER_LOG_FILE</a> and <a undefined>MASTER_LOG_POS</a> options using the master's [binary log](/mariadb-administration/server-monitoring-logs/binary-log) file and position. See [MDEV-19264](https://jira.mariadb.org/browse/MDEV-19264) for more information.

```sql
$ mariabackup --slave-info
```

### `-S, --socket`

Defines the socket for connecting to local database.

```sql
--socket=name
```

Using this option, you can define the UNIX domain socket you want to use when connecting to a local database server.  The option accepts a string argument.  For more information, see the `mysql --help` command.

```sql
$ mariabackup --backup \
      --socket=/var/mysql/mysql.sock
```

### `--ssl`

Enables [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). By using this option, you can explicitly configure Mariabackup to to encrypt its connection with [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption) when communicating with the server. You may find this useful when performing backups in environments where security is extra important or when operating over an insecure network.

TLS is also enabled even without setting this option when certain other TLS options are set. For example, see the descriptions of the following options:

- <a undefined>--ssl-ca</a>
- <a undefined>--ssl-capath</a>
- <a undefined>--ssl-cert</a>
- <a undefined>--ssl-cipher</a>
- <a undefined>--ssl-key</a>

### `--ssl-ca`

Defines a path to a PEM file that should contain one or more X509 certificates for trusted Certificate Authorities (CAs) to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). This option requires that you use the absolute path, not a relative path. For example:

```sql
--ssl-ca=/etc/my.cnf.d/certificates/ca.pem
```

This option is usually used with other TLS options. For example:

```sql
$ mariabackup --backup \
   --ssl-cert=/etc/my.cnf.d/certificates/client-cert.pem \
   --ssl-key=/etc/my.cnf.d/certificates/client-key.pem \
   --ssl-ca=/etc/my.cnf.d/certificates/ca.pem 
```

See [Secure Connections Overview: Certificate Authorities (CAs)](/kb/en/secure-connections-overview/#certificate-authorities-cas) for more information.

This option implies the <a undefined>--ssl</a> option.

### `--ssl-capath`

Defines a path to a directory that contains one or more PEM files that should each contain one X509 certificate for a trusted Certificate Authority (CA) to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). This option requires that you use the absolute path, not a relative path. For example:

```sql
--ssl-capath=/etc/my.cnf.d/certificates/ca/
```

This option is usually used with other TLS options. For example:

```sql
$ mariabackup --backup \
   --ssl-cert=/etc/my.cnf.d/certificates/client-cert.pem \
   --ssl-key=/etc/my.cnf.d/certificates/client-key.pem \
   --ssl-ca=/etc/my.cnf.d/certificates/ca.pem \
   --ssl-capath=/etc/my.cnf.d/certificates/ca/
```

The directory specified by this option needs to be run through the <a undefined>openssl rehash</a> command.

See [Secure Connections Overview: Certificate Authorities (CAs)](/kb/en/secure-connections-overview/#certificate-authorities-cas) for more information

This option implies the <a undefined>--ssl</a> option.

### `--ssl-cert`

Defines a path to the X509 certificate file to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). This option requires that you use the absolute path, not a relative path. For example:

```sql
--ssl-cert=/etc/my.cnf.d/certificates/client-cert.pem
```

This option is usually used with other TLS options. For example:

```sql
$ mariabackup --backup \
   --ssl-cert=/etc/my.cnf.d/certificates/client-cert.pem \
   --ssl-key=/etc/my.cnf.d/certificates/client-key.pem \
   --ssl-ca=/etc/my.cnf.d/certificates/ca.pem
```

This option implies the <a undefined>--ssl</a> option.

### `--ssl-cipher`

Defines the list of permitted ciphers or cipher suites to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). For example:

```sql
--ssl-cipher=name
```

This option is usually used with other TLS options. For example:

```sql
$ mariabackup --backup \
   --ssl-cert=/etc/my.cnf.d/certificates/client-cert.pem \
   --ssl-key=/etc/my.cnf.d/certificates/client-key.pem \
   --ssl-ca=/etc/my.cnf.d/certificates/ca.pem
   --ssl-cipher=TLSv1.2
```

To determine if the server restricts clients to specific ciphers, check the <a undefined>ssl_cipher</a> system variable.

This option implies the <a undefined>--ssl</a> option.

### `--ssl-crl`

Defines a path to a PEM file that should contain one or more revoked X509 certificates to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). This option requires that you use the absolute path, not a relative path. For example:

```sql
--ssl-crl=/etc/my.cnf.d/certificates/crl.pem
```

This option is usually used with other TLS options. For example:

```sql
$ mariabackup --backup \
   --ssl-cert=/etc/my.cnf.d/certificates/client-cert.pem \
   --ssl-key=/etc/my.cnf.d/certificates/client-key.pem \
   --ssl-ca=/etc/my.cnf.d/certificates/ca.pem \
   --ssl-crl=/etc/my.cnf.d/certificates/crl.pem
```

See [Secure Connections Overview: Certificate Revocation Lists (CRLs)](/kb/en/secure-connections-overview/#certificate-revocation-lists-crls) for more information.

This option is only supported if Mariabackup was built with OpenSSL. If Mariabackup was built with yaSSL, then this option is not supported. See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb) for more information about which libraries are used on which platforms.

### `--ssl-crlpath`

Defines a path to a directory that contains one or more PEM files that should each contain one revoked X509 certificate to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). This option requires that you use the absolute path, not a relative path. For example:

```sql
--ssl-crlpath=/etc/my.cnf.d/certificates/crl/
```

This option is usually used with other TLS options. For example:

```sql
$ mariabackup --backup \
   --ssl-cert=/etc/my.cnf.d/certificates/client-cert.pem \
   --ssl-key=/etc/my.cnf.d/certificates/client-key.pem \
   --ssl-ca=/etc/my.cnf.d/certificates/ca.pem \
   --ssl-crlpath=/etc/my.cnf.d/certificates/crl/ 
```

The directory specified by this option needs to be run through the <a undefined>openssl rehash</a> command.

See [Secure Connections Overview: Certificate Revocation Lists (CRLs)](/kb/en/secure-connections-overview/#certificate-revocation-lists-crls) for more information.

This option is only supported if Mariabackup was built with OpenSSL. If Mariabackup was built with yaSSL, then this option is not supported. See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb) for more information about which libraries are used on which platforms.

### `--ssl-key`

Defines a path to a private key file to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). This option requires that you use the absolute path, not a relative path. For example:

```sql
--ssl-key=/etc/my.cnf.d/certificates/client-key.pem
```

This option is usually used with other TLS options. For example:

```sql
$ mariabackup --backup \
   --ssl-cert=/etc/my.cnf.d/certificates/client-cert.pem \
   --ssl-key=/etc/my.cnf.d/certificates/client-key.pem \
   --ssl-ca=/etc/my.cnf.d/certificates/ca.pem
```

This option implies the <a undefined>--ssl</a> option.

### `--ssl-verify-server-cert`

Enables [server certificate verification](/kb/en/secure-connections-overview/#server-certificate-verification). This option is disabled by default.

This option is usually used with other TLS options. For example:

```sql
$ mariabackup --backup \
   --ssl-cert=/etc/my.cnf.d/certificates/client-cert.pem \
   --ssl-key=/etc/my.cnf.d/certificates/client-key.pem \
   --ssl-ca=/etc/my.cnf.d/certificates/ca.pem \
   --ssl-verify-server-cert
```

### `--stream`

Streams backup files to stdout.

```sql
--stream=xbstream
```

Using this command option, you can set Mariabackup to stream the backup files to stdout in the given format.  Currently, the supported format is `xbstream`.

```sql
$ mariabackup --stream=xbstream > backup.xb
```

To extract all files from the xbstream archive into a directory use  the `mbstream` utility

```sql
$ mbstream  -x < backup.xb
```

If a backup is streamed, then Mariabackup will record the format in the <a undefined>xtrabackup_info</a> file.

### `--tables`

Defines the tables you want to include in the backup.

```sql
--tables=REGEX
```

Using this option, you can define what tables you want Mariabackup to back up from the database.  The table values are defined using Regular Expressions. To define the tables you want to exclude from the backup, see the <a undefined>--tables-exclude</a> option.

```sql
$ mariabackup --backup \
     --databases=example
     --tables=nodes_* \
     --tables-exclude=nodes_tmp
```

If a backup is a [partial backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup), then Mariabackup will record that detail in the <a undefined>xtrabackup_info</a> file.

### `--tables-exclude`

Defines the tables you want to exclude from the backup.

```sql
--tables-exclude=REGEX
```

Using this option, you can define what tables you want Mariabackup to exclude from the backup.  The table values are defined using Regular Expressions. To define the tables you want to include from the backup, see the <a undefined>--tables</a> option.

```sql
$ mariabackup --backup \
     --databases=example
     --tables=nodes_* \
     --tables-exclude=nodes_tmp
```

If a backup is a [partial backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup), then Mariabackup will record that detail in the <a undefined>xtrabackup_info</a> file.

### `--tables-file`

Defines path to file with tables for backups.

```sql
--tables-file=/path/to/file
```

Using this option, you can set a path to a file listing the tables you want to back up.  Mariabackup iterates over each line in the file.  The format is `database.table`.

```sql
$ mariabackup --backup \
     --databases=example \
     --tables-file=/etc/mysql/backup-file
```

If a backup is a [partial backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup), then Mariabackup will record that detail in the <a undefined>xtrabackup_info</a> file.

### `--target-dir`

Defines the destination directory.

```sql
--target-dir=/path/to/target
```

Using this option you can define the destination directory for the backup.  Mariabackup writes all backup files to this directory. Mariabackup will create the directory, if it does not exist (but it will not create the full path recursively, i.e. at least parent directory if the --target-dir must exist=

```sql
$ mariabackup --backup \
       --target-dir=/data/backups
```

### `--throttle`

Defines the limit for I/O operations per second in IOS values.

```sql
--throttle=#
```

Using this option, you can set a limit on the I/O operations Mariabackup performs per second in IOS values.  It is only used during the <a undefined>--backup</a> command option.

### `--tls-version`

This option accepts a comma-separated list of TLS protocol versions. A TLS protocol version will only be enabled if it is present in this list. All other TLS protocol versions will not be permitted. For example:

```sql
--tls-version="TLSv1.2,TLSv1.3"
```

This option is usually used with other TLS options. For example:

```sql
$ mariabackup --backup \
   --ssl-cert=/etc/my.cnf.d/certificates/client-cert.pem \
   --ssl-key=/etc/my.cnf.d/certificates/client-key.pem \
   --ssl-ca=/etc/my.cnf.d/certificates/ca.pem \
   --tls-version="TLSv1.2,TLSv1.3"
```

This option was added in [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/).

See [Secure Connections Overview: TLS Protocol Versions](/kb/en/secure-connections-overview/#tls-protocol-versions) for more information.

### `-t, --tmpdir`

Defines path for temporary files.

```sql
--tmpdir=/path/tmp[;/path/tmp...]
```

Using this option, you can define the path to a directory Mariabackup uses in writing temporary files.  If you want to use more than one, separate the values by a semicolon (that is, `;`).  When passing multiple temporary directories, it cycles through them using round-robin.

```sql
$ mariabackup --backup \
     --tmpdir=/data/tmp;/tmp
```

### `--use-memory`

Defines the buffer pool size that is used during the prepare stage.

```sql
--use-memory=124M
```

Using this option, you can define the buffer pool size for Mariabackup. Use it instead of `buffer_pool_size`.

```sql
$ mariabackup --prepare \
      --use-memory=124M
```

### `--user`

Defines the username for connecting to the MariaDB Server.

```sql
--user=name
-u name
```

When Mariabackup runs it connects to the specified MariaDB Server to get its backups.  Using this option, you can define the database user uses for authentication.

```sql
$ mariabackup --backup \
      --user=root \
      --password=root_passwd
```

### `--version`

Prints version information.

Using this option, you can print the Mariabackup version information to stdout.

```sql
$ mariabackup --version
```

### `--version-check`

Enables version check.

Using this option, you can enable Mariabackup version check.  If you would like to disable the version check, use the <a undefined>--no-version-check</a> option.

```sql
$ mariabackup --backup --version-check
```