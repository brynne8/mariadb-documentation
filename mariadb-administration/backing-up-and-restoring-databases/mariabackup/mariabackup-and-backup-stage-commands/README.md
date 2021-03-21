# Mariabackup and BACKUP STAGE Commands

##### MariaDB starting with [10.4.1](/kb/en/mariadb-1041-release-notes/)

The [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) commands were introduced in [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/).

The [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) commands are a set of commands to make it possible to make an efficient external backup tool. How Mariabackup uses these commands depends on whether you are using the version that is bundled with MariaDB Community Server or the version that is bundled with [MariaDB Enterprise Server](https://mariadb.com/docs/products/mariadb-enterprise-server/).

## Mariabackup and `BACKUP STAGE` Commands in MariaDB Community Server

##### MariaDB starting with [10.4.1](/kb/en/mariadb-1041-release-notes/)

In MariaDB Community Server, Mariabackup first supported [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) commands in [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/).

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, the [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) commands are <strong>not</strong> supported, so Mariabackup executes the [FLUSH TABLES WITH READ LOCK](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) command to lock the database. When the backup is complete, it executes the [UNLOCK TABLES](/sql-statements-structure/sql-statements/transactions/transactions-unlock-tables) command to unlock the database.

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, the [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) commands are supported. However, the version of Mariabackup that is bundled with MariaDB Community Server does not yet use the [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) commands in the most efficient way. Mariabackup simply executes the following [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) commands to lock the database:

```sql
BACKUP STAGE START;
BACKUP STAGE BLOCK_COMMIT;
```

When the backup is complete, it executes the following [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) command to unlock the database:

```sql
BACKUP STAGE END;
```

If you would like to use a version of Mariabackup that uses the [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) commands in the most efficient way, then your best option is to use [MariaDB Enterprise Backup](https://mariadb.com/docs/usage/mariadb-enterprise-backup/) that is bundled with [MariaDB Enterprise Server](https://mariadb.com/docs/products/mariadb-enterprise-server/).

### Tasks Performed Prior to `BACKUP STAGE` in MariaDB Community Server

- Copy some transactional tables.
<ul start="1"><li>[InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) (i.e. `ibdataN` and file extensions `.ibd` and `.isl`)
</li></ul>
- Copy the tail of some transaction logs.
<ul start="1"><li>The tail of the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log) (i.e. `ib_logfileN` files) will be copied for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.
</li></ul>

### `BACKUP STAGE START` in MariaDB Community Server

Mariabackup from MariaDB Community Server does not currently perform any tasks in the `START` stage.

### `BACKUP STAGE FLUSH` in MariaDB Community Server

Mariabackup from MariaDB Community Server does not currently perform any tasks in the `FLUSH` stage.

### `BACKUP STAGE BLOCK_DDL` in MariaDB Community Server

Mariabackup from MariaDB Community Server does not currently perform any tasks in the `BLOCK_DDL` stage.

### `BACKUP STAGE BLOCK_COMMIT` in MariaDB Community Server

Mariabackup from MariaDB Community Server performs the following tasks in the `BLOCK_COMMIT` stage:

- Copy other files.
<ul start="1"><li>i.e. file extensions `.frm`, `.isl`, `.TRG`, `.TRN`, `.opt`, `.par`
</li></ul>
- Copy some transactional tables.
<ul start="1"><li>[Aria](/columns-storage-engines-and-plugins/storage-engines/aria) (i.e. `aria_log_control` and file extensions `.MAD` and `.MAI`)
</li></ul>
- Copy the non-transactional tables.
<ul start="1"><li>[MyISAM](/kb/en/myisam/) (i.e. file extensions `.MYD` and `.MYI`)
</li><li>[MERGE](/columns-storage-engines-and-plugins/storage-engines/merge) (i.e. file extensions `.MRG`)
</li><li>[ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive) (i.e. file extensions `.ARM` and `.ARZ`)
</li><li>[CSV](/columns-storage-engines-and-plugins/storage-engines/csv) (i.e. file extensions `.CSM` and `.CSV`)
</li></ul>
- Create a [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks) checkpoint using the <a undefined>rocksdb_create_checkpoint</a> system variable.
- Copy the tail of some transaction logs.
<ul start="1"><li>The tail of the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log) (i.e. `ib_logfileN` files) will be copied for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.
</li></ul>
- Save the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) position to <a undefined>xtrabackup_binlog_info</a>.
- Save the [Galera Cluster](/kb/en/galera/) state information to <a undefined>xtrabackup_galera_info</a>.

### `BACKUP STAGE END` in MariaDB Community Server

Mariabackup from MariaDB Community Server performs the following tasks in the `END` stage:

- Copy the [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks) checkpoint into the backup.

## Mariabackup and `BACKUP STAGE` Commands in MariaDB Enterprise Server

##### MariaDB starting with [10.2.25](/kb/en/mariadb-10225-release-notes/)

[MariaDB Enterprise Backup](https://mariadb.com/docs/usage/mariadb-enterprise-backup/) first supported [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) commands in [MariaDB Enterprise Server 10.4.6-1](https://mariadb.com/docs/appendix/release-notes/mariadb-enterprise-server-10-4-6-1-release-notes/), [MariaDB Enterprise Server 10.3.16-1](https://mariadb.com/docs/appendix/release-notes/mariadb-enterprise-server-10-3-16-1-release-notes/), and [MariaDB Enterprise Server 10.2.25-1](https://mariadb.com/docs/appendix/release-notes/mariadb-enterprise-server-10-2-25-1-release-notes/).

The following sections describe how the [MariaDB Enterprise Backup](https://mariadb.com/docs/usage/mariadb-enterprise-backup/) version of Mariabackup that is bundled with [MariaDB Enterprise Server](https://mariadb.com/docs/products/mariadb-enterprise-server/) uses each [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) command in an efficient way.

### `BACKUP STAGE START` in MariaDB Enterprise Server

Mariabackup from MariaDB Enterprise Server performs the following tasks in the `START` stage:

- Copy all transactional tables.
<ul start="1"><li>[InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) (i.e. `ibdataN` and file extensions `.ibd` and `.isl`)
</li><li>[Aria](/columns-storage-engines-and-plugins/storage-engines/aria) (i.e. `aria_log_control` and file extensions `.MAD` and `.MAI`)
</li></ul>
- Copy the tail of all transaction logs.
<ul start="1"><li>The tail of the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log) (i.e. `ib_logfileN` files) will be copied for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.
</li><li>The tail of the Aria redo log (i.e. `aria_log.N` files) will be copied for [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) tables.
</li></ul>

### `BACKUP STAGE FLUSH` in MariaDB Enterprise Server

Mariabackup from MariaDB Enterprise Server performs the following tasks in the `FLUSH` stage:

- Copy all non-transactional tables that are not in use. This list of used tables is found with [SHOW OPEN TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-open-tables).
<ul start="1"><li>[MyISAM](/kb/en/myisam/) (i.e. file extensions `.MYD` and `.MYI`)
</li><li>[MERGE](/columns-storage-engines-and-plugins/storage-engines/merge) (i.e. file extensions `.MRG`)
</li><li>[ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive) (i.e. file extensions `.ARM` and `.ARZ`)
</li><li>[CSV](/columns-storage-engines-and-plugins/storage-engines/csv) (i.e. file extensions `.CSM` and `.CSV`)
</li></ul>
- Copy the tail of all transaction logs.
<ul start="1"><li>The tail of the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log) (i.e. `ib_logfileN` files) will be copied for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.
</li><li>The tail of the Aria redo log (i.e. `aria_log.N` files) will be copied for [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) tables.
</li></ul>

### `BACKUP STAGE BLOCK_DDL` in MariaDB Enterprise Server

Mariabackup from MariaDB Enterprise Server performs the following tasks in the `BLOCK_DDL` stage:

- Copy other files.
<ul start="1"><li>i.e. file extensions `.frm`, `.isl`, `.TRG`, `.TRN`, `.opt`, `.par`
</li></ul>
- Copy the non-transactional tables that were in use during `BACKUP STAGE FLUSH`.
<ul start="1"><li>[MyISAM](/kb/en/myisam/) (i.e. file extensions `.MYD` and `.MYI`)
</li><li>[MERGE](/columns-storage-engines-and-plugins/storage-engines/merge) (i.e. file extensions `.MRG`)
</li><li>[ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive) (i.e. file extensions `.ARM` and `.ARZ`)
</li><li>[CSV](/columns-storage-engines-and-plugins/storage-engines/csv) (i.e. file extensions `.CSM` and `.CSV`)
</li></ul>
- Check `ddl.log` for DDL executed before the `BLOCK DDL` stage.
<ul start="1"><li>The file names of newly created tables can be read from `ddl.log`.
</li><li>The file names of dropped tables can also be read from `ddl.log`.
</li><li>The file names of renamed tables can also be read from `ddl.log`, so the files can be renamed instead of re-copying them.
</li></ul>
- Copy changes to system log tables.
<ul start="1"><li><a undefined>mysql.general_log</a>
</li><li>[mysql.slow_log](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlslow_log-table)
</li><li>This is easy as these are append only.
</li></ul>
- Copy the tail of all transaction logs.
<ul start="1"><li>The tail of the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log) (i.e. `ib_logfileN` files) will be copied for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.
</li><li>The tail of the Aria redo log (i.e. `aria_log.N` files) will be copied for [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) tables.
</li></ul>

### `BACKUP STAGE BLOCK_COMMIT` in MariaDB Enterprise Server

Mariabackup from MariaDB Enterprise Server performs the following tasks in the `BLOCK_COMMIT` stage:

- Create a [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks) checkpoint using the <a undefined>rocksdb_create_checkpoint</a> system variable.
- Copy changes to system log tables.
<ul start="1"><li><a undefined>mysql.general_log</a>
</li><li>[mysql.slow_log](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlslow_log-table)
</li><li>This is easy as these are append only.
</li></ul>
- Copy changes to statistics tables.
<ul start="1"><li><a undefined>mysql.table_stats</a>
</li><li><a undefined>mysql.column_stats</a>
</li><li><a undefined>mysql.index_stats</a>
</li></ul>
- Copy the tail of all transaction logs.
<ul start="1"><li>The tail of the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log) (i.e. `ib_logfileN` files) will be copied for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.
</li><li>The tail of the Aria redo log (i.e. `aria_log.N` files) will be copied for [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) tables.
</li></ul>
- Save the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) position to <a undefined>xtrabackup_binlog_info</a>.
- Save the [Galera Cluster](/kb/en/galera/) state information to <a undefined>xtrabackup_galera_info</a>.

### `BACKUP STAGE END` in MariaDB Enterprise Server

Mariabackup from MariaDB Enterprise Server performs the following tasks in the `END` stage:

- Copy the [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks) checkpoint into the backup.