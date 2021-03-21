# Backup and Restore Overview

This article briefly discusses the main ways to backup MariaDB. For detailed descriptions and syntax, see the individual pages. More detail is in the process of being added.

## Logical vs Physical Backups

Logical backups consist of the SQL statements necessary to restore the data, such as [CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database/), [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) and [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/).

Physical backups are performed by copying the individual data files or directories.

The main differences are as follows:

- logical backups are more flexible, as the data can be restored on other hardware configurations, MariaDB versions or even on another DBMS, while physical backups cannot be imported on significantly different hardware, a different DBMS, or potentially even a different MariaDB version.
- logical backups can be performed at the level of database and table, while physical databases are the level of directories and files. In the [MyISAM](/kb/en/myisam/) and [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) storage engines, each table has an equivalent set of files. (In versions prior to [MariaDB 5.5](/kb/en/what-is-mariadb-55/), by default a number of InnoDB tables are stored in the same file, in which case it is not possible to backup by table. See [innodb_file_per_table](/kb/en/xtradbinnodb-server-system-variables/#innodb_file_per_table).)
- logical backups are larger in size than the equivalent physical backup.
- logical backups takes more time to both backup and restore than the equivalent physical backup.
- log files and configuration files are not part of a logical backup

## Backup Tools

### Mariabackup

[Mariabackup](/kb/en/mariadb-backup/) is a fork of [Percona XtraBackup](/kb/en/backup-restore-and-import-xtrabackup/) with added support for [MariaDB 10.1](/kb/en/what-is-mariadb-101/) [compression](InnoDB_compression) and [data-at-rest encryption](/kb/en/data-at-rest-encryption/). It is included with [MariaDB 10.1.23](/kb/en/mariadb-10123-release-notes/) and later.

### mysqldump

[mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) performs a logical backup. It is the most flexible way to perform a backup and restore, and a good choice when the data size is relatively small.

For large datasets, the backup file can be large, and the restore time lengthy.

mysqldump dumps the data into SQL format (it can also dump into other formats, such as CSV or XML) which can then easily be imported into another database. The data can be imported into other versions of MariaDB, MySQL, or even another DBMS entirely, assuming there are no version or DBMS-specific statements in the dump.

mysqldump dumps triggers along with tables, as these are part of the table definition. However, [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/), [views](/programming-customizing-mariadb/views/), and [events](/programming-customizing-mariadb/triggers-events/event-scheduler/events/) are not, and need extra parameters to be recreated explicitly (for example, `--routines` and `--events`).  [Procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/) and [functions](functions) are however also part of the system tables (for example [mysql.proc](/kb/en/mysqlproc-table/)).

#### InnoDB Logical Backups

InnoDB uses the [buffer pool](/kb/en/xtradbinnodb-buffer-pool/), which stores data and indexes from its tables in memory. This buffer is very important for performance. If InnoDB data doesn't fit the memory, it is important that the buffer contains the most frequently accessed data. However, last accessed data is candidate for insertion into the buffer pool. If not properly configured, when a table scan happens, InnoDB may copy the whole contents of a table into the buffer pool. The problem with logical backups is that they always imply full table scans.

An easy way to avoid this is by increasing the value of the [innodb_old_blocks_time](/kb/en/xtradbinnodb-server-system-variables/#innodb_old_blocks_time) system variable. It represents the number of milliseconds that must pass before a recently accessed page can be put into the "new" sublist in the buffer pool. Data which is accessed only once should remain in the "old" sublist. This means that they will soon be evicted from the buffer pool. Since during the backup process the "old" sublist is likely to store data that is not useful, one could also consider resizing it by changing the value of the [innodb_old_blocks_pct](/kb/en/xtradbinnodb-server-system-variables/#innodb_old_blocks_pct) system variable.

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

Since [MariaDB 10.0](/kb/en/what-is-mariadb-100/), it is also possible to explicitly dump the buffer pool on disk before starting a logical backup, and restore it after the process. This will undo any negative change to the buffer pool which happens during the backup. To dump the buffer pool, the [innodb_buffer_pool_dump_now](/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_dump_now) system variable can be set to ON. To restore it, the [innodb_buffer_pool_load_now](/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_load_now) system variable can be set to ON.

#### Examples

Backing up a single database

```sql
shell> mysqldump db_name > backup-file.sql
```

Restoring or loading the database

```sql
shell> mysql db_name < backup-file.sql
```

See the [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) page for detailed syntax and examples.

### mysqlhotcopy

mysqlhotcopy is currently deprecated.

[mysqlhotcopy](/clients-utilities/backup-restore-and-import-clients/mysqlhotcopy/) performs a physical backup, and works only for backing up [MyISAM](/kb/en/myisam/) and [ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive/) tables. It can only be run on the same machine as the location of the database directories.

#### Examples

```sql
shell> mysqlhotcopy db_name [/path/to/new_directory]
shell> mysqlhotcopy db_name_1 ... db_name_n /path/to/new_directory
```

### Percona XtraBackup

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/) is the recommended backup method to use instead of Percona XtraBackup.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/), Percona XtraBackup is <strong>not supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), Percona XtraBackup is only <strong>partially supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

[Percona XtraBackup](/kb/en/backup-restore-and-import-xtrabackup/) is a tool for performing fast, hot backups. It was designed specifically for [XtraDB/InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) databases, but can be used with any storage engine (although not with [MariaDB 10.1](/kb/en/what-is-mariadb-101/) [encryption](/kb/en/encryption/) and [compression](/kb/en/compression/)). It is not included by default with MariaDB.

### Filesystem Snapshots

Some filesystems, like Veritas, support snapshots. During the snapshot, the table must be locked. The proper steps to obtain a snapshot are:

- From the mysql client, execute [FLUSH TABLES WITH READ LOCK](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/). The client must remain open.
- From a shell, execute `mount vxfs snapshot`
- The client can execute [UNLOCK TABLES](/kb/en/transactions-lock/).
- Copy the snapshot files.
- From a shell, unmount the snapshot with `umount snapshot`.

### LVM

Widely-used physical backup method, using a Perl script as a wrapper. See [http://www.lenzg.net/mylvmbackup/](http://www.lenzg.net/mylvmbackup/).

### Percona TokuBackup

For details, see:

- [TokuDB Hot Backup – Part 1](https://www.percona.com/blog/2013/09/12/tokudb-hot-backup-part-1/)
- [TokuDB Hot Backup – Part 2](https://www.percona.com/blog/2013/09/19/tokudb-hot-backup-part-2/)
- [TokuDB Hot Backup Now a MySQL Plugin](https://www.percona.com/blog/2015/02/05/tokudb-hot-backup-now-mysql-plugin/)

## See Also

- [Streaming MariaDB backups in the cloud](https://mariadb.com/blog/streaming-mariadb-backups-cloud) (mariadb.com blog)