# Storage Snapshots and BACKUP STAGE Commands

##### MariaDB starting with [10.4.1](/kb/en/mariadb-1041-release-notes/)

The `BACKUP STAGE` commands were introduced in [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/).

The [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) commands are a set of commands to make it possible to make an efficient external backup tool. These commands could even be used by tools that perform backups by taking a snapshot of a file system, SAN, or some other kind of storage device.

## Generic Backup Process with Storage Snapshots

A tool that backs up MariaDB by taking a snapshot of a file system, SAN, or some other kind of storage device could use each `BACKUP STAGE` command in the following way:

- First, execute the following:

```sql
BACKUP STAGE START
BACKUP STAGE BLOCK_COMMIT
```

- Then, take the snapshot.

- Then, execute the following:

```sql
BACKUP STAGE END
```

The above ensures that all non-transactional tables are properly flushed to disk before the snapshot is done.
Using `BACKUP STAGE` commands is also more efficient than using the [FLUSH TABLES WITH READ LOCK](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) command as the above set of commands will not block or be blocked by write operations to transactional tables.

Note that when the backup is completed, one should delete all files with the "#sql" prefix, as these are files used by concurrent running `ALTER TABLE`.  Note that InnoDB will on server restart automatically delete any tables with the "#sql" prefix.