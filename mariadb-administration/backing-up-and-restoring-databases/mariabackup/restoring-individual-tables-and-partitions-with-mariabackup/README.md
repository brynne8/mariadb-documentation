# Restoring Individual Tables and Partitions with Mariabackup

When using Mariabackup, you don't necessarily need to restore every table and/or partition that was backed up. Even if you're starting from a [full backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup/), it is certainly possible to restore only certain tables and/or partitions from the backup, as long as the table or partition involved is in an [InnoDB file-per-table tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/). This page documents how to restore individual tables and partitions.

## Preparing the Backup

Before you can restore from a backup, you first need to <strong>prepare</strong> it to make the data files consistent. You can do so with the <a undefined>--prepare</a> command option.

The ability to restore individual tables and partitions relies on [InnoDB's transportable tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces). For MariaDB to import tablespaces like these, [InnoDB](/kb/en/xtradb-and-innodb/) looks for a file with a `.cfg` extension. For Mariabackup to create these files, you also need to add the <a undefined>--export</a> option during the prepare step.

For example, you might execute the following command:

```sql
$ mariabackup --prepare --export \
   --target-dir=/var/mariadb/backup/ \
   --user=mariabackup --password=mypassword
```

If this operation completes without error, then the backup is ready to be restored.

##### MariaDB until [10.2.8](/kb/en/mariadb-1028-release-notes/)

Before [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), Mariabackup did not support the <a undefined>--export</a> option. See [MDEV-13466](https://jira.mariadb.org/browse/MDEV-13466) about that. In earlier versions of MariaDB, this means that Mariabackup could not create `.cfg` files for [InnoDB file-per-table tablespaces](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/) during the `--prepare` stage. You can still [import file-per-table tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces) without the `.cfg` files in many cases, so it may still be possible in those versions to [restore partial backups](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup/) or to [restore individual tables and partitions](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/restoring-individual-tables-and-partitions-with-mariabackup/) with just the `.ibd` files. If you have a [full backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup/) and you need to create `.cfg` files for [InnoDB file-per-table tablespaces](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/), then you can do so by preparing the backup as usual without the `--export` option, and then restoring the backup, and then starting the server. At that point, you can use the server's built-in features to [copy the transportable tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces).

## Restoring the Backup

The restore process for restoring individual tables and/or partitions is quite different than the process for [full backups](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup/).

Rather than using the <a undefined>--copy-back</a> or the <a undefined>--move-back</a>, each individual [InnoDB file-per-table tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/) file will have to be manually imported into the target server. The process that is used to restore the backup will depend on whether partitioning is involved.

### Restoring Individual Non-Partitioned Tables

To restore individual non-partitioned tables from a backup, find the `.ibd` and `.cfg` files for the table in the backup, and then import them using the [Importing Transportable Tablespaces for Non-partitioned Tables](/kb/en/innodb-file-per-table-tablespaces/#importing-transportable-tablespaces-for-non-partitioned-tables) process.

### Restoring Individual Partitions and Partitioned Tables

To restore individual partitions or partitioned tables from a backup, find the `.ibd` and `.cfg` files for the partition(s) in the backup, and then import them using the [Importing Transportable Tablespaces for Partitioned Tables](/kb/en/innodb-file-per-table-tablespaces/#importing-transportable-tablespaces-for-partitioned-tables) process.