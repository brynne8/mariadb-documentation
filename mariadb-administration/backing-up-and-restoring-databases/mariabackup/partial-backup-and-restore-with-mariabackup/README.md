# Partial Backup and Restore with Mariabackup

When using Mariabackup, you have the option of performing partial backups. Partial backups allow you to choose which databases or tables to backup, as long as the table or partition involved is in an [InnoDB file-per-table tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/).This page documents how to perform partial backups.

## Backing up the Database Server

Just like with [full backups](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup/), in order to back up the database, you need to run Mariabackup with the <a undefined>--backup</a> option to tell it to perform a backup and with the <a undefined>--target-dir</a> option to tell it where to place the backup files. The target directory must be empty or not exist.

For a partial backup, there are a few other arguments that you can provide as well:

- To tell it which databases to backup, you can provide the <a undefined>--databases</a> option.
- To tell it which databases to exclude from the backup, you can provide the <a undefined>--databases-exclude</a> option.
- To tell it to check a file for the databases to backup, you can provide the <a undefined>--databases-file</a> option.
- To tell it which tables to backup, you can use the <a undefined>--tables</a> option.
- To tell it which tables to exclude from the backup, you can provide the <a undefined>--tables-exclude</a> option.
- To tell it to check a file for specific tables to backup, you can provide the <a undefined>--tables-file</a> option.

The non-file partial backup options support regex in the database and table names.

For example, to take a backup of any database that starts with the string `app1_` and any table in those databases that start with the string `tab_`, run the following command:

```sql
$ mariabackup --backup \
   --target-dir=/var/mariadb/backup/ \
   --databases='app1_*' --tables='tab_*' \
   --user=mariabackup --password=mypassword
```

Mariabackup cannot currently backup a subset of partitions from a partitioned table. Backing up a partitioned table is currently an all-or-nothing selection. See [MDEV-17132](https://jira.mariadb.org/browse/MDEV-17132) about that. If you need to backup a subset of partitions, then one possibility is that instead of using Mariabackup, you can [export the file-per-table tablespaces of the partitions](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces).

The time the backup takes depends on the size of the databases or tables you're backing up. You can cancel the backup if you need to, as the backup process does not modify the database.

Mariabackup writes the backup files the target directory. If the target directory doesn't exist, then it creates it. If the target directory exists and contains files, then it raises an error and aborts.

## Preparing the Backup

Just like with [full backups](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup/), the data files that Mariabackup creates in the target directory are not point-in-time consistent, given that the data files are copied at different times during the backup operation. If you try to restore from these files, InnoDB notices the inconsistencies and crashes to protect you from corruption. In fact, for partial backups, the backup is not even a completely functional MariaDB data directory, so InnoDB would raise more errors than it would for full backups. This point will also be very important to keep in mind during the restore process.

Before you can restore from a backup, you first need to <strong>prepare</strong> it to make the data files consistent. You can do so with the <a undefined>--prepare</a> command option.

Partial backups rely on [InnoDB's transportable tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces). For MariaDB to import tablespaces like these, [InnoDB](/kb/en/xtradb-and-innodb/) looks for a file with a `.cfg` extension. For Mariabackup to create these files, you also need to add the <a undefined>--export</a> option during the prepare step.

For example, you might execute the following command:

```sql
$ mariabackup --prepare --export \
   --target-dir=/var/mariadb/backup/
```

If this operation completes without error, then the backup is ready to be restored.

##### MariaDB until [10.2.8](/kb/en/mariadb-1028-release-notes/)

In [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/) and before, Mariabackup did not support the <a undefined>--export</a> option. See [MDEV-13466](https://jira.mariadb.org/browse/MDEV-13466) about that. In these versions of MariaDB, this means that Mariabackup could not create `.cfg` files for [InnoDB file-per-table tablespaces](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/) during the `--prepare` stage. You can still [import file-per-table tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces) without the `.cfg` files in many cases, so it may still be possible in those versions to [restore partial backups](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup/) or to [restore individual tables and partitions](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/restoring-individual-tables-and-partitions-with-mariabackup/) with just the `.ibd` files. If you have a [full backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup/) and you need to create `.cfg` files for [InnoDB file-per-table tablespaces](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/), then you can do so by preparing the backup as usual without the `--export` option, and then restoring the backup, and then starting the server. At that point, you can use the server's built-in features to [copy the transportable tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces).

## Restoring the Backup

The restore process for partial backups is quite different than the process for [full backups](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup/). A partial backup is not a completely functional data directory. The data dictionary in the [InnoDB system tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/) will still contain entries for the databases and tables that were not included in the backup.

Rather than using the <a undefined>--copy-back</a> or the <a undefined>--move-back</a>, each individual [InnoDB file-per-table tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/) file will have to be manually imported into the target server. The process that is used to import the file will depend on whether partitioning is involved.

### Restoring Individual Non-Partitioned Tables

To restore individual non-partitioned tables from a backup, find the `.ibd` and `.cfg` files for the table in the backup, and then import them using the [Importing Transportable Tablespaces for Non-partitioned Tables](/kb/en/innodb-file-per-table-tablespaces/#importing-transportable-tablespaces-for-non-partitioned-tables) process.

### Restoring Individual Partitions and Partitioned Tables

To restore individual partitions or partitioned tables from a backup, find the `.ibd` and `.cfg` files for the partition(s) in the backup, and then import them using the [Importing Transportable Tablespaces for Partitioned Tables](/kb/en/innodb-file-per-table-tablespaces/#importing-transportable-tablespaces-for-partitioned-tables) process.