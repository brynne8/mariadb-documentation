# DROP TABLE

## Syntax

```sql
DROP [TEMPORARY] TABLE [IF EXISTS] [/*COMMENT TO SAVE*/]
    tbl_name [, tbl_name] ...
    [WAIT n|NOWAIT]
    [RESTRICT | CASCADE]
```

## Description

`DROP TABLE` removes one or more tables. You must have the `DROP` privilege
for each table. All table data and the table definition are removed, as well as [triggers](/programming-customizing-mariadb/triggers-events/triggers) associated to the table, so be
careful with this statement! If any of the tables named in the argument list do
not exist, MariaDB returns an error indicating by name which non-existing tables
it was unable to drop, but it also drops all of the tables in the list that do
exist.

<strong>Important</strong>: When a table is dropped, user privileges on the table are not
automatically dropped. See [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant).

If another thread is using the table in an explicit transaction or an autocommit transaction, then the thread acquires a [metadata lock (MDL)](/sql-statements-structure/sql-statements/transactions/metadata-locking) on the table. The `DROP TABLE` statement will wait in the "Waiting for table metadata lock" [thread state](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-states) until the MDL is released. MDLs are released in the following cases:

- If an MDL is acquired in an explicit transaction, then the MDL will be released when the transaction ends.
- If an MDL is acquired in an autocommit transaction, then the MDL will be released when the statement ends.
- Transactional and non-transactional tables are handled the same.

Note that for a partitioned table, `DROP TABLE` permanently removes the table
definition, all of its partitions, and all of the data which was stored in
those partitions. It also removes the partitioning definition (.par) file
associated with the dropped table.

For each referenced table, `DROP TABLE` drops a temporary table with that name, if it exists. If it does not exist, and the `TEMPORARY` keyword is not used, it drops a non-temporary table with the same name, if it exists. The `TEMPORARY` keyword ensures that a non-temporary table will not accidentally be dropped.

Use `IF EXISTS` to prevent an error from occurring for tables that do not
exist. A `NOTE` is generated for each non-existent table when using
`IF EXISTS`. See [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings).

If a [foreign key](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys) references this table, the table cannot be dropped. In this case, it is necessary to drop the foreign key first.

`RESTRICT` and `CASCADE` are allowed to make porting from other database systems easier. In MariaDB, they do nothing.

The comment before the table names (`/*COMMENT TO SAVE*/`) is stored in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log). That feature can be used by replication tools to send their internal messages.

It is possible to specify table names as `db_name`.`tab_name`. This is useful to delete tables from multiple databases with one statement. See [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers) for details.

The [DROP privilege](/kb/en/grant/#table-privileges) is required to use `DROP TABLE` on non-temporary tables. For temporary tables, no privilege is required, because such tables are only visible for the current session.

<strong>Note:</strong> `DROP TABLE` automatically commits the current active transaction,
unless you use the `TEMPORARY` keyword.

##### MariaDB starting with [10.5.4](/kb/en/mariadb-1054-release-notes/)

From [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/), `DROP TABLE` reliably deletes table remnants inside a storage engine even if the `.frm` file is missing. Before then, a missing `.frm` file would result in the statement failing.

##### MariaDB starting with [10.3.1](/kb/en/mariadb-1031-release-notes/)

### WAIT/NOWAIT

Set the lock wait timeout. See [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait).

## DROP TABLE in replication

`DROP TABLE` has the following characteristics in [replication](/replication):

- <code class="highlight fixed" style="white-space:pre-wrap">DROP TABLE IF EXISTS</code> are always logged.
- <code class="highlight fixed" style="white-space:pre-wrap">DROP TABLE</code> without <code class="highlight fixed" style="white-space:pre-wrap">IF EXISTS</code> for tables that don't exist are not written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).
- Dropping of <code class="highlight fixed" style="white-space:pre-wrap">TEMPORARY</code> tables are prefixed in the log with <code class="highlight fixed" style="white-space:pre-wrap">TEMPORARY</code>. These drops are only logged when running [statement](/kb/en/binary-log-formats/#statement-based) or [mixed mode](/kb/en/binary-log-formats/#mixed) replication.
- One <code class="highlight fixed" style="white-space:pre-wrap">DROP TABLE</code> statement can be logged with up to 3 different <code class="highlight fixed" style="white-space:pre-wrap">DROP</code> statements:
<ul start="1"><li><code class="highlight fixed" style="white-space:pre-wrap">DROP TEMPORARY TABLE list_of_non_transactional_temporary_tables</code>
</li><li><code class="highlight fixed" style="white-space:pre-wrap">DROP TEMPORARY TABLE list_of_transactional_temporary_tables</code>
</li><li><code class="highlight fixed" style="white-space:pre-wrap">DROP TABLE list_of_normal_tables</code>
</li></ul>

Starting from [MariaDB 10.0.8](/kb/en/mariadb-1008-release-notes/), <code class="highlight fixed" style="white-space:pre-wrap">DROP TABLE</code> on the master is treated on the slave as <code class="highlight fixed" style="white-space:pre-wrap">DROP TABLE IF EXISTS</code>. You can change that by setting [slave-ddl-exec-mode](/kb/en/replication-and-binary-log-server-system-variables/#slave_ddl_exec_mode) to <code class="highlight fixed" style="white-space:pre-wrap">STRICT</code>.

## Dropping an Internal #sql-... Table

If the mysqld process is killed during an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) you may find a table named #sql-... in your data directory. In [MariaDB 10.3](/kb/en/what-is-mariadb-103/), InnoDB tables with this prefix will de deleted automatically during startup.
In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) we will ensure that these temporary tables will always be deleted automatically.

If you want to delete one of these tables explicitly you can do so by using the following syntax:

```sql
DROP TABLE `#mysql50##sql-...`;
```

When running an `ALTER TABLE…ALGORITHM=INPLACE` that rebuilds the table, InnoDB will create an internal `#sql-ib` table. For these tables, the `.frm` file will be called something else. In order to drop such a table after a server crash, you must rename the `#sql*.frm` file to match the `#sql-ib*.ibd` file.

## Dropping All Tables in a Database

The best way to drop all tables in a database is by executing [DROP DATABASE](/sql-statements-structure/sql-statements/data-definition/drop/drop-database), which will drop the database itself, and all tables in it.

However, if you want to drop all tables in the database, but you also want to keep the database itself and any other non-table objects in it, then you would need to execute `DROP TABLE` to drop each individual table. You can construct these `DROP TABLE` commands by querying the <a undefined>TABLES</a> table in the [information_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables) database. For example:

```sql
SELECT CONCAT('DROP TABLE IF EXISTS `', TABLE_SCHEMA, '`.`', TABLE_NAME, '`;')
FROM information_schema.TABLES
WHERE TABLE_SCHEMA = 'mydb';
```

## Examples

```sql
DROP TABLE Employees, Customers;
```

## Notes

Beware that `DROP TABLE` can drop both tables and [sequences](/sql-statements-structure/sequences/create-sequence). This is mainly done to allow old tools like [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump) to work with sequences.

## See Also

- [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table)
- [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table)
- [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table)
- [DROP SEQUENCE](/sql-statements-structure/sequences/drop-sequence)
- Variable [slave-ddl-exec-mode](/kb/en/replication-and-binary-log-server-system-variables/#slave_ddl_exec_mode).