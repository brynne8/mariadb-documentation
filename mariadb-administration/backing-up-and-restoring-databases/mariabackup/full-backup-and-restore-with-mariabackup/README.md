# Full Backup and Restore with Mariabackup

When using Mariabackup, you have the option of performing a full or an incremental backup. Full backups create a complete backup of the database server in an empty directory while incremental backups update a previous backup with whatever changes to the data have occurred since the backup. This page documents how to perform full backups.

## Backing up the Database Server

In order to back up the database, you need to run Mariabackup with the <a undefined>--backup</a> option to tell it to perform a backup and with the <a undefined>--target-dir</a> option to tell it where to place the backup files. When taking a full backup, the target directory must be empty or it must not exist.

To take a backup, run the following command:

```sql
$ mariabackup --backup \
   --target-dir=/var/mariadb/backup/ \
   --user=mariabackup --password=mypassword
```

The time the backup takes depends on the size of the databases or tables you're backing up. You can cancel the backup if you need to, as the backup process does not modify the database.

Mariabackup writes the backup files the target directory. If the target directory doesn't exist, then it creates it. If the target directory exists and contains files, then it raises an error and aborts.

Here is an example backup directory:

```sql
$ ls /var/mariadb/backup/

aria_log.0000001  mysql                   xtrabackup_checkpoints
aria_log_control  performance_schema      xtrabackup_info
backup-my.cnf     test                    xtrabackup_logfile
ibdata1           xtrabackup_binlog_info
```

## Preparing the Backup for Restoration

The data files that Mariabackup creates in the target directory are not point-in-time consistent, given that the data files are copied at different times during the backup operation. If you try to restore from these files, InnoDB notices the inconsistencies and crashes to protect you from corruption

Before you can restore from a backup, you first need to <strong>prepare</strong> it to make the data files consistent. You can do so with the <a undefined>--prepare</a> option.

```sql
$ mariabackup --prepare \
   --target-dir=/var/mariadb/backup/
```

## Restoring the Backup

Once the backup is complete and you have prepared the backup for restoration (previous step), you can restore the backup using either the <a undefined>--copy-back</a> or the <a undefined>--move-back</a> options. The <a undefined>--copy-back</a> option allows you to keep the original backup files. The <a undefined>--move-back</a> option actually moves the backup files to the <a undefined>datadir</a>, so the original backup files are lost.

- First, [stop the MariaDB Server process](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb).

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

- Finally, [start the MariaDB Server process](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb).

### Restoring with Other Tools

Once a full backup is prepared, it is a fully functional MariaDB data directory. Therefore, as long as the MariaDB Server process is stopped on the target server, you can technically restore the backup using any file copying tool, such as `cp` or `rysnc`. For example, you could also execute the following to restore the backup:

```sql
$ rsync -avrP /var/mariadb/backup /var/lib/mysql/
$ chown -R mysql:mysql /var/lib/mysql/
$ rm /var/lib/mysql/ib_logfile*
```

When using Mariabackup from versions prior to [MariaDB 10.2.10](/kb/en/mariadb-10210-release-notes/), you would also have to remove any pre-existing  [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) files. For example:

```sql
$ rm /var/lib/mysql/ib_logfile*
```

See [Mariabackup Internal Details: Redo Log Files](/kb/en/mariabackup-overview/#redo-log-files) for more information.