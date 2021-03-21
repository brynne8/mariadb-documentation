# Incremental Backup and Restore with Mariabackup

When using Mariabackup, you have the option of performing a full or incremental backup.  Full backups create a complete copy in an empty directory while incremental backups update a previous backup with new data.  This page documents incremental backups.

InnoDB pages contain log sequence numbers, or LSN's.  Whenever you modify a row on any InnoDB table on the database, the storage engine increments this number.  When performing an incremental backup, Mariabackup checks the most recent LSN for the backup against the LSN's contained in the database.  It then updates any of the backup files that have fallen behind.

## Backing up the Database Server

In order to take an incremental backup, you first need to take a [full backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup/). In order to back up the database, you need to run Mariabackup with the <a undefined>--backup</a> option to tell it to perform a backup and with the <a undefined>--target-dir</a> option to tell it where to place the backup files. When taking a full backup, the target directory must be empty or it must not exist.

To take a backup, run the following command:

```sql
$ mariabackup --backup \
   --target-dir=/var/mariadb/backup/ \
   --user=mariabackup --password=mypassword
```

This backs up all databases into the target directory `/var/mariadb/backup`.  If you look in that directory at the <a undefined>xtrabackup_checkpoints</a> file, you can see the LSN data provided by InnoDB.

For example:

```sql
backup_type = full-backuped
from_lsn = 0
to_lsn = 1635102
last_lsn = 1635102
recover_binlog_info = 0
```

## Backing up the Incremental Changes

Once you have created a full backup on your system, you can also back up the incremental changes as often as you would like.

In order to perform an incremental backup, you need to run Mariabackup with the <a undefined>--backup</a> option to tell it to perform a backup and with the <a undefined>--target-dir</a> option to tell it where to place the incremental changes. The target directory must be empty.  You also need to run it with the <a undefined>--incremental-basedir</a> option to tell it the path to the full backup taken above. For example:

```sql
$ mariabackup --backup \
   --target-dir=/var/mariadb/inc1/ \
   --incremental-basedir=/var/mariadb/backup/ \
   --user=mariabackup --password=mypassword
```

This command creates a series of delta files that store the incremental changes in `/var/mariadb/inc1`.  You can find a similar <a undefined>xtrabackup_checkpoints</a> file in this directory, with the updated LSN values.

For example:

```sql
backup_type = full-backuped
from_lsn = 1635102
to_lsn = 1635114
last_lsn = 1635114
recover_binlog_info = 0
```

To perform additional incremental backups, you can then use the target directory of the previous incremental backup as the incremental base directory of the next incremental backup. For example:

```sql
$ mariabackup --backup \
   --target-dir=/var/mariadb/inc2/ \
   --incremental-basedir=/var/mariadb/inc1/ \
   --user=mariabackup --password=mypassword
```

## Preparing the Backup

Following the above steps, you have three backups in `/var/mariadb`:  The first is a full backup, the others are increments on this first backup.  In order to restore a backup to the database, you first need to apply the incremental backups to the base full backup.  This is done using the <a undefined>--prepare</a> command option. In [MariaDB 10.1](/kb/en/what-is-mariadb-101/), you would also have to use the the <a undefined>--apply-log-only</a> option.

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, perform the following process:

First, prepare the base backup:

```sql
$ mariabackup --prepare \
   --target-dir=/var/mariadb/backup
```

Running this command brings the base full backup, that is, `/var/mariadb/backup`, into sync with the changes contained in the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) collected while the backup was taken.

Then, apply the incremental changes to the base full backup:

```sql
$ mariabackup --prepare \
   --target-dir=/var/mariadb/backup \
   --incremental-dir=/var/mariadb/inc1
```

Running this command brings the base full backup, that is, `/var/mariadb/backup`, into sync with the changes contained in the first incremental backup.

For each remaining incremental backup, repeat the last step to bring the base full backup into sync with the changes contained in that incremental backup.

##### MariaDB until [10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/), perform the following process:

First, prepare the base backup:

```sql
$ mariabackup --prepare --apply-log-only \
   --target-dir=/var/mariadb/backup
```

Running this command brings the base full backup, that is, `/var/mariadb/backup`, into sync with the changes contained in the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) collected while the backup was taken.

Then, apply the incremental changes to the base full backup:

```sql
$ mariabackup --prepare --apply-log-only \
   --target-dir=/var/mariadb/backup \
   --incremental-dir=/var/mariadb/inc1
```

Running this command brings the base full backup, that is, `/var/mariadb/backup`, into sync with the changes contained in the first incremental backup.

For each remaining incremental backup, repeat the last step to bring the base full backup into sync with the changes contained in that incremental backup.

## Restoring the Backup

Once you've applied all incremental backups to the base, you can restore the backup using either the <a undefined>--copy-back</a> or the <a undefined>--move-back</a> options. The <a undefined>--copy-back</a> option allows you to keep the original backup files. The <a undefined>--move-back</a> option actually moves the backup files to the <a undefined>datadir</a>, so the original backup files are lost.

- First, [stop the MariaDB Server process](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

- Then, ensure that the <a undefined>datadir</a> is empty.

- Then, run Mariabackup with one of the options mentioned above:

```sql
$ mariabackup --copy-back \
   --target-dir=/var/mariadb/backup/
```

- Then, you may need to fix the file permissions.

When Mariabackup restores a database, it preserves the file and directory privileges of the backup.  However, it writes the files to disk as the user and group restoring the database.  As such, after restoring a backup, you may need to adjust the owner of the data directory to match the user and group for the MariaDB Server, typically `mysql` for both.  For example, to recursively change ownership of the files to the `mysql` user and group, you could execute:

```sql
$ chown -R mysql:mysql /var/lib/mysql/
```

- Finally, [start the MariaDB Server process](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).