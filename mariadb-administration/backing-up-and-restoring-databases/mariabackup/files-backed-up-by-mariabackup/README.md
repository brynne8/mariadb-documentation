# Files Backed Up By Mariabackup

## Files Included in Backup

Mariabackup backs up the files listed below.

### InnoDB Data Files

Mariabackup backs up the following InnoDB data files:

- [InnoDB system tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/)
- [InnoDB file-per-table tablespaces](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/)

### MyRocks Data Files

Starting with [MariaDB 10.2.16](/kb/en/mariadb-10216-release-notes/) and [MariaDB 10.3.8](/kb/en/mariadb-1038-release-notes/), Mariabackup will back up tables that use the [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/) storage engine. This data data is located in the directory defined by the <a undefined>rocksdb_datadir</a> system variable. Mariabackup backs this data up by performing a checkpoint using the <a undefined>rocksdb_create_checkpoint</a> system variable.

Mariabackup does not currently support [partial backups](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup/) for MyRocks.

### Other Data Files

Mariabackup also backs up files with the following extensions:

- `frm`
- `isl`
- `MYD`
- `MYI`
- `MAD`
- `MAI`
- `MRG`
- `TRG`
- `TRN`
- `ARM`
- `ARZ`
- `CSM`
- `CSV`
- `opt`
- `par`

## Files Excluded From Backup

Mariabackup does <strong>not</strong> back up the files listed below.

- [InnoDB Temporary Tablespaces](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-temporary-tablespaces/)
- [Binary logs](/mariadb-administration/server-monitoring-logs/binary-log/)
- [Relay logs](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/)