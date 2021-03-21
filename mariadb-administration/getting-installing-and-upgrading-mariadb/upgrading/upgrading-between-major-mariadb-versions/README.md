# Upgrading Between Major MariaDB Versions

MariaDB is designed to allow easy upgrades. You should be able to trivially upgrade from ANY earlier
MariaDB version to the latest one (for example [MariaDB 5.5](/kb/en/what-is-mariadb-55/).x to [MariaDB 10.5](/kb/en/what-is-mariadb-105/).x), usually in a few seconds. This is also mainly true for any MySQL version &lt; 8.0 to [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and up.

Upgrades are normally easy because:

- All MariaDB table data files are backward compatible
- The MariaDB connection protocol is backward compatible. You don't normally need to upgrade any of your old clients to be able to connect to a newer MariaDB version.
- The MariaDB slave can be of any newer version than the master.

MariaDB Corporation regularly runs tests to check that one can upgrade from [MariaDB 5.5](/kb/en/what-is-mariadb-55/) to the latest MariaDB version without any trouble. All older versions should work too (as long as the storage engines you were using are still around).

Note that if you are using [MariaDB Galera Cluster](/replication/galera-cluster), you have to follow the [Galera upgrading instructions](/replication/galera-cluster/upgrading-galera-cluster)!

## Requirements for Doing an Upgrade Between Major Versions

- Go through the individual version upgrade notes (listed below) to look for any major changes or configuration options that have changed.
- Ensure that the target MariaDB version supports the storage engines you are using. For example, in 10.5 [TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb) is not supported.
- Backup of the database (just in case). At least, take a copy of the <code class="fixed" style="white-space:pre-wrap">mysql</code> data directory with [mysqldump --add-drop-table mysql](/clients-utilities/backup-restore-and-import-clients/mysqldump) as most of the upgrade changes are done there (adding new fields and new system tables etc).
- Ensure that the [innodb_fast_shutdown](/kb/en/innodb-system-variables/#innodb_fast_shutdown) variable is not 2 (fast crash shutdown). The default of this variable is 1. The most safe option for upgrades is 0, but the shutdown time may be notable larger with 0 than for 1 as there is a lot more cleanups done for 0.
- Clean shutdown of the server. This is necessary because even if data files are compatible between versions, recovery logs may not be.

Note that rpms don't support upgrading between major versions, only minor like 10.4.1 to 10.4.2. If you are using rpms, you should de-install the old MariaDB rpms and install the new MariaDB rpms before running [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade). Note that when installing the new rpms, [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade) may be run automatically. There is no problem with running [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade) many times.

## Recommended Steps

- If you have a [master-slave setup](/replication/standard-replication), first upgrade one slave and when you have verified that the slave works well, upgrade the rest of the slaves (if any). Then [upgrade one slave to master](/replication/standard-replication/changing-a-slave-to-become-the-master), upgrade the master, and change the master to a slave.

- If you don't have a master-slave setup, then [take a backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup), [shutdown MariaDB](/clients-utilities/mysqladmin) and do the upgrade.

### Step by step instructions for upgrades

- Upgrade MariaDB binaries and libraries, preferably without starting MariaDB.
- If the MariaDB server process, [mysqld or mariadbd](/mysqld-options) was not started as part of the upgrade, start it by executing <code class="fixed" style="white-space:pre-wrap">mysqld --skip-grant-tables</code>. This may produce some warnings about some system tables not being up to date, but you can ignore these for now as [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade) will fix that.
- Run [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade)
- Restart MariaDB server.

## Work Done by [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade)

The main work done when upgrading is done by running [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade). The main things it does are:

- Updating the system tables in the <code class="fixed" style="white-space:pre-wrap">mysql</code> database to the newest version. This is very quick.
- [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade) also runs [mysqlcheck --check-upgrade](mysql_check) to check if there have been any collation changes between the major versions. This recreates indexes in old tables that are using any of the changed collations. This can take a bit of time if there are a lot of tables or there are many tables which used the changed collation. The last time a collation changed was in MariaDB/MySQL 5.1.23.

## Post Upgrade Work

Check the [MariaDB error log](/mariadb-administration/server-monitoring-logs/error-log) for any problems during upgrade. If there are any warnings in the log files, do you best to get rid of them!

The common warnings/errors are:

- Using obsolete options.  If this is the case, remove them from your [my.cnf files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

- Check the manual for [new features](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading) that have been added since your last MariaDB version.
- Test that your application works as before. The main difference from before is that because of optimizer improvements your application should work better than before, but in some rare cases the optimizer may get something wrong. In this case, you can try to use [explain](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain), [optimizer trace](/kb/en/mariadb-internals-documentation-optimizer-trace/) or [optimizer_switch](/replication/optimization-and-tuning/query-optimizations/optimizer-switch) to fix the queries.

## If Something Goes Wrong

- First, check the [MariaDB error log](/mariadb-administration/server-monitoring-logs/error-log) to see if you are using configure options that are not supported anymore.
- Check the upgrade notices for the MariaDB release that you are upgrading to.
- File an issue in the [MariaDB bug tracker](/kb/en/bug-tracking/) so that we know about the issue and can provide a fix to make upgrades even better.
- Add a comment to this manual entry for how we can improve it.

### Disaster Recovery

In the unlikely event something goes wrong, you can try the following:

- Remove the InnoDB tables from the <code class="fixed" style="white-space:pre-wrap">mysql</code> data directory. They are:
<ul start="1"><li>gtid_slave_pos
</li><li>innodb_table_stats
</li><li>innodb_index_stats
</li><li>transaction_registry
</li></ul>
- Move the <code class="fixed" style="white-space:pre-wrap">mysql</code> data directory to <code class="fixed" style="white-space:pre-wrap">mysql-old</code> and run [mysql_install_db](/clients-utilities/mysql_install_db) to generate a new one.
- After the above, you have to add back your old users.
- When done, delete the <code class="fixed" style="white-space:pre-wrap">mysql-old</code> data directory.

## Downgrading

MariaDB server is not designed for downgrading. That said, in most cases, as long as you haven't run any [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) or [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) statements and you have a [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump) of your old <code class="fixed" style="white-space:pre-wrap">mysql</code> database , you should be able to downgrade to your previous version by doing the following:

- Do a clean shutdown. For this special case you have to set [innodb_fast_shutdown](/kb/en/innodb-system-variables/#innodb_fast_shutdown) to 0,before taking down the new MariaDB server,  to ensure there are no redo or undo logs that need to be applied on the downgraded server.
- Delete the tables in the <code class="fixed" style="white-space:pre-wrap">mysql</code> database (if you didn't use the option <code class="fixed" style="white-space:pre-wrap">--add-drop-table</code> to [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump))
- Delete the new MariaDB installation
- Install the old MariaDB version
- Start the server with [mysqld --skip-grant-tables](/kb/en/mysqld-options/#-skip-grant-tables)
- Install the old <code class="fixed" style="white-space:pre-wrap">mysql</code> database
- Execute in the [mysql client](/clients-utilities/mysql-client) <code class="fixed" style="white-space:pre-wrap">FLUSH PRIVILEGES</code>

## See Also

- [Upgrading from MySQL to MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-upgrading-from-mysql-to-mariadb/upgrading-from-mysql-to-mariadb)
- [Upgrading from MariaDB 10.4 to MariaDB 10.5](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-104-to-mariadb-105)
- [Upgrading from MariaDB 10.3 to MariaDB 10.4](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-103-to-mariadb-104)
- [Upgrading from MariaDB 10.2 to MariaDB 10.3](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-102-to-mariadb-103)
- [Upgrading from MariaDB 10.1 to MariaDB 10.2](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-101-to-mariadb-102)
- [Upgrading from MariaDB 10.0 to MariaDB 10.1](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-100-to-mariadb-101)
- [Upgrading from MariaDB 5.5 to MariaDB 10.0](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-55-to-mariadb-100)
- [Galera upgrading instructions](/replication/galera-cluster/upgrading-galera-cluster)
- [innodb_fast_shutdown](/kb/en/innodb-system-variables/#innodb_fast_shutdown)