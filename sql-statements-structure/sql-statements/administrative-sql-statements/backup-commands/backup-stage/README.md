# BACKUP STAGE

##### MariaDB starting with [10.4.1](/kb/en/mariadb-1041-release-notes/)

The `BACKUP STAGE` commands were introduced in [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/).

The `BACKUP STAGE` commands are a set of commands to make it possible to make an efficient external backup tool.

## Syntax

```sql
BACKUP STAGE [START | FLUSH | BLOCK_DDL | BLOCK_COMMIT | END ]
```

In the following text, a transactional table means InnoDB or "InnoDB-like engine with redo log that can lock redo purges and can be copied without locks by an outside process".

## Goals with BACKUP STAGE Commands

- To be able to do a majority of the backup with the minimum possible server locks. Especially for transactional tables (InnoDB, MyRocks etc) there is only need for a very short block of new commits while copying statistics and log tables.
- DDL are only needed to be blocked for a very short duration of the backup while [mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup) is copying the tables affected by DDL during the initial part of the backup.
- Most non transactional tables (those that are not in use) will be copied during `BACKUP STAGE START`.  The exceptions are system statistic and log tables that are not blocked during the backup until `BLOCK_COMMIT`.
- Should work efficiently with backup tools that use disk snapshots.
- Should work as efficiently as possible for all table types that store data on the local disks.
- As little copying as possible under higher level stages/locks. For example, .frm (dictionary) and .trn (trigger) files should be copying while copying the table data.

## `BACKUP STAGE` Commands

### `BACKUP STAGE START`

The `START` stage is designed for the following tasks:

- Blocks purge of redo files for storage engines that needs this (Aria)
- Start logging of DDL commands into 'datadir'/ddl.log. This may take a short time as the command has to wait until there are no active DDL commands.

### `BACKUP STAGE FLUSH`

The `FLUSH` stage is designed for the following tasks:

- FLUSH all changes for inactive non-transactional tables, except for statistics and log tables.
- Close all tables that are not in use, to ensure they are marked as closed for the backup.
- BLOCK all new write locks for all non transactional tables (except statistics and log tables).  The command will not wait for tables that are in use by read-only transactions.

DDLs don't have to be blocked at this stage as they can't cause the table to be in an inconsistent state. This is true also for non-transactional tables.

### `BACKUP STAGE BLOCK_DDL`

The `BLOCK_DDL` stage is designed for the following tasks:

- Wait for all statements using write locked non-transactional tables to end.
- Blocks [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table), [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table), [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table), and [RENAME TABLE](/sql-statements-structure/sql-statements/data-definition/rename-table).
- Blocks also start off a <strong>new</strong> [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) and the <strong>final rename phase</strong> of [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table). Running ALTER TABLES are not blocked.

### `BACKUP STAGE BLOCK_COMMIT`

The `BLOCK_COMMIT` stage is designed for the following tasks:

- Lock the binary log and commit/rollback to ensure that no changes are committed to any tables. If there are active commits or data to be copied to the binary log this will be allowed to finish.
- This doesn't lock temporary tables that are not used by replication. However these will be blocked when it's time to write to the binary log.
- Lock system log tables and statistics tables, flush them and mark them closed.

When the `BLOCK_COMMIT`'s stages return, this is the 'backup time'. Everything committed will be in the backup and everything not committed will roll back.

Transactional engines will continue to do changes to the redo log during the `BLOCK COMMIT` stage, but this is not important as all of these will roll back later as the changes will not be committed.

### `BACKUP STAGE END`

The `END` stage is designed for the following tasks:

- End DDL logging
- Free resources

## Using `BACKUP STAGE` Commands with Backup Tools

### Using `BACKUP STAGE` Commands with Mariabackup

The `BACKUP STAGE` commands are a set of commands to make it possible to make an efficient external backup tool. How [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup) uses these commands depends on whether you are using the version that is bundled with MariaDB Community Server or the version that is bundled with [MariaDB Enterprise Server](https://mariadb.com/docs/products/mariadb-enterprise-server/). See [Mariabackup and BACKUP STAGE Commands](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/mariabackup-and-backup-stage-commands) for some examples on how [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup) uses these commands.

If you would like to use a version of [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup) that uses the [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) commands in an efficient way, then one option is to use [MariaDB Enterprise Backup](https://mariadb.com/docs/usage/mariadb-enterprise-backup/) that is bundled with [MariaDB Enterprise Server](https://mariadb.com/docs/products/mariadb-enterprise-server/).

### Using `BACKUP STAGE` Commands with Storage Snapshots

The `BACKUP STAGE` commands are a set of commands to make it possible to make an efficient external backup tool. These commands could even be used by tools that perform backups by taking a snapshot of a file system, SAN, or some other kind of storage device. See [Storage Snapshots and BACKUP STAGE Commands](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/storage-snapshots-and-backup-stage-commands) for some examples on how to use each `BACKUP STAGE` command in an efficient way.

## Privileges

`BACKUP STAGE` requires the [RELOAD](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) privilege.

## Notes

- Only one connection can run `BACKUP STAGE START`. If a second connection tries, it will wait until the first one has executed `BACKUP STAGE END`.
- If the user skips a `BACKUP STAGE`, then all intermediate backup stages will automatically be run. This will allow us to add new stages within the `BACKUP STAGE` hierarchy in the future with even more precise locks without causing problems for tools using an earlier version of the `BACKUP STAGE` implementation.
- One can use the <a undefined>max_statement_time</a> or <a undefined>lock_wait_timeout</a> system variables to ensure that a `BACKUP STAGE` command doesn't block the server too long.
- DDL logging will only be available in [MariaDB Enterprise Server](https://mariadb.com/docs/products/mariadb-enterprise-server/) 10.2 and later.

## See Also

- [BACKUP LOCK](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-lock) Locking a table from DDL's.
- [MDEV-5336](https://jira.mariadb.org/browse/MDEV-5336). Implement BACKUP STAGE for safe external backups.