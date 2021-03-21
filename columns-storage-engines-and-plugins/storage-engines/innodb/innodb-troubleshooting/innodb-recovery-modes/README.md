# InnoDB Recovery Modes

The XtraDB/InnoDB recovery mode is a mode used for recovering from emergency situations. You should ensure you have a backup of your database before making changes in case you need to restore it. The [innodb_force_recovery](/kb/en/xtradbinnodb-server-system-variables/#innodb_force_recovery) server system variable sets the recovery mode. A mode of 0 is normal use, while the higher the mode, the more stringent the restrictions. Higher modes incorporate all limitations of the lower modes.

The recovery mode should never be set to a value other than zero except in an emergency situation.

Generally, it is best to start with a recovery mode of 1, and increase in single increments if needs be. With a recovery mode &lt; 4, only corrupted pages should be lost. With 4, secondary indexes could be corrupted. With 5, results could be inconsistent and secondary indexes could be corrupted (even if they were not with 4). A value of 6 leaves pages in an obsolete state, which might cause more corruption.

Until [MariaDB 10.2.7](/kb/en/mariadb-1027-release-notes/), mode `0` was the only mode permitting changes to the data. From [MariaDB 10.2.7](/kb/en/mariadb-1027-release-notes/), write transactions are permitted with mode `3` or less.

To recover the tables, you can execute [SELECTs](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) to dump data, and [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table) (when write transactions are permitted) to remove corrupted tables.

The following modes are available:

## Recovery Modes

<table><tbody><tr><th>Mode</th><th>Description</th></tr>
<tr><td>0</td><td>The default mode while XtraDB/InnoDB is running normally. Until <a href="/kb/en/mariadb-1027-release-notes/">MariaDB 10.2.7</a>, it was the only mode permitting changes to the data. From <a href="/kb/en/mariadb-1027-release-notes/">MariaDB 10.2.7</a>, write transactions are permitted with innodb_force_recovery&lt;=3.</td></tr>
<tr><td>1</td><td>(SRV_FORCE_IGNORE_CORRUPT) allows the the server to keep running even if corrupt pages are detected. It does so by making redo log based recovery ignore certain errors, such as missing data files or corrupted data pages. Any redo log for affected files or pages will be skipped. You can facilitate dumping tables by getting the SELECT * FROM table_name statement to jump over corrupt indexes and pages.</td></tr>
<tr><td>2</td><td>(SRV_FORCE_NO_BACKGROUND) stops the master thread from running, preventing a crash that occurs during a purge. No purge will be performed, so the undo logs will keep growing.</td></tr>
<tr><td>3</td><td>(SRV_FORCE_NO_TRX_UNDO) does not roll back transactions after the crash recovery. Does not affect rollback of currently active transactions. Starting with <a href="/kb/en/mariadb-1027-release-notes/">MariaDB 10.2.7</a>, will also prevent some undo-generating background tasks from running. These tasks could hit a lock wait due to the recovered incomplete transactions whose rollback is being prevented.</td></tr>
<tr><td>4</td><td>(SRV_FORCE_NO_IBUF_MERGE) does not calculate tables statistics and prevents insert buffer merges.</td></tr>
<tr><td>5</td><td>(SRV_FORCE_NO_UNDO_LOG_SCAN) treats incomplete transactions as committed, and does not look at the <a href="/kb/en/undo-log/">undo logs</a> when starting.</td></tr>
<tr><td>6</td><td>(SRV_FORCE_NO_LOG_REDO) does not perform redo log roll-forward as part of recovery. Running queries that require indexes are likely to fail with this mode active. However, if a table dump still causes a crash, you can try using a <code>SELECT * FROM tab ORDER BY primary_key DESC</code> to dump all the data portion after the corrupted part.</td></tr>
</tbody></table>

Note also that XtraDB (&lt;= [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)) by default will crash the server when it detects corrupted data in a single-table tablespace. This behaviour can be changed - see the [innodb_corrupt_table_action](/kb/en/xtradbinnodb-server-system-variables/#innodb_corrupt_table_action) system variable.

## Fixing Things

Try to set innodb_force_recovery to 1 and start mariadb.  If that fails, try a value of "2".  If a value of 2 works, then there is a chance the only corruption you have experienced is within the innodb "undo logs".  If that gets mariadb started, you should be able to dump your database with mysqldump.  You can verify any other issues with any tables by running "mysqlcheck --all-databases".

If you were able to successfully dump your databases, or had previously known good backups, drop your database(s) from the mariadb command line like "[DROP DATABASE](/sql-statements-structure/sql-statements/data-definition/drop/drop-database) yourdatabase".  Stop mariadb.  Go to /var/lib/mysql (or whereever your mysql data directory is located) and "rm -i ib*".  Start mariadb, create the database(s) you dropped ("[CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database) yourdatabase"), and then import your most recent dumps: "mysql &lt; mydatabasedump.sql"