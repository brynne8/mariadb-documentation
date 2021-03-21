# Setting Up Replication

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

Getting [replication](/replication) working involves steps on both the master server/s and steps on the slave server/s.

[MariaDB 10.0](/kb/en/what-is-mariadb-100/) introduced replication with [global transaction IDs](/kb/en/global-transaction-id/). These have a number of benefits, and it is generally recommended to use this feature from [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

## Setting up a Replication Slave with Mariabackup

If you would like to use [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup) to set up a replication slave, then you might find the information at [Setting up a Replication Slave with Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/setting-up-a-replication-slave-with-mariabackup) helpful.

## Versions

In general, when replicating across different versions of MariaDB, it is best that the master is an older version than the slave. MariaDB versions are usually backward compatible, while of course older versions cannot always be forward compatible. See also  [Replicating from MySQL Master to MariaDB Slave](#replicating-from-mysql-master-to-mariadb-slave).

## Configuring the Master

- Enable binary logging if it's not already enabled. See [Activating the Binary Log](/mariadb-administration/server-monitoring-logs/binary-log/activating-the-binary-log) and [Binary log formats](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats) for details.
- Give the master a unique [server_id](/kb/en/server-system-variables/#server_id). All slaves must also be given a server_id. This can be a number from 1 to 2<sup>32</sup>-1, and must be unique for each server in the replicating group.
- Specify a unique name for your replication logs with [--log-basename](/kb/en/mysqld-options-full-list/#-log-basename). If this is not specified your host name will be used and there will be problems if the hostname ever changes.
- Slaves will need permission to connect and start replicating from a server. Usually this is done by creating a dedicated slave user, and granting that user permission only to replicate (REPLICATION SLAVE permission).

### Example Enabling Replication for MariaDB

Add the following into your [my.cnf](/kb/en/configuring-mariadb-with-mycnf/) file and restart the database.

```sql
[mariadb]
log-bin
server_id=1
log-basename=master1
binlog-format=mixed
```

The server id is a unique number for each MariaDB/MySQL server in your network.
[binlog-format](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats) specifies how your statements are logged. This mainly affects the size of the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) that is sent between the Master and the Slaves.

Then execute the following SQL with the [`mysql`](/clients-utilities/mysql-client/mysql-command-line-client) command line client:

```sql
CREATE USER 'replication_user'@'%' IDENTIFIED BY 'bigs3cret';
GRANT REPLICATION SLAVE ON *.* TO 'replication_user'@'%';
```

### Example Enabling Replication for MySQL

If you want to enable replication from MySQL to MariaDB, you can do it in almost the same way as between MariaDB servers. The main difference is that MySQL doesn't support `log-basename`.

```sql
[mysqld]
log-bin
server_id=1
```

## Settings to Check

There are a number of options that may impact or break replication. Check the following settings to avoid problems.

- [skip-networking](/kb/en/server-system-variables/#skip_networking). If `skip-networking=1`, the server will limit connections to localhost only, and prevent all remote slaves from connecting.
- [bind-address](/kb/en/server-system-variables/#bind_address). Similarly, if the address the server listens for TCP/IP connections is 127.0.0.1 (localhost), remote slaves connections will fail.

## Configuring the Slave

- Give the slave a unique [server_id](/kb/en/replication-and-binary-log-server-system-variables/#server_id). All servers, whether masters or slaves, are given a server_id. This can be a number from 1 to 2<sup>32</sup>-1, and must be unique for each server in the replicating group. The server will need to be restarted in order for a change in this option to take effect.

## Getting the Master's Binary Log Co-ordinates

Now you need prevent any changes to the data while you view the binary log position. You'll use this to tell the slave at exactly which point it should start replicating from.

- On the master, flush and lock all tables by running <code class="fixed" style="white-space:pre-wrap">FLUSH TABLES WITH READ LOCK</code>. Keep this session running - exiting it will release the lock.
- Get the current position in the binary log by running <a undefined>SHOW MASTER STATUS</a>:

```sql
SHOW MASTER STATUS;
+--------------------+----------+--------------+------------------+
| File               | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+--------------------+----------+--------------+------------------+
| master1-bin.000096 |      568 |              |                  |
+--------------------+----------+--------------+------------------+
```

- Record the <em>File</em> and <em>Position</em> details. If binary logging has just been enabled, these will be blank.
- Now, with the lock still in place, copy the data from the master to the slave. See [Backup, Restore and Import](/kb/en/backup-restore-and-import/) for details on how to do this.
- Note for live databases: You just need to make a local copy of the data, you don't need to keep the master locked until the slave has imported the data.
- Once the data has been copied, you can release the lock on the master by running [UNLOCK TABLES](/kb/en/transactions-lock/).

```sql
UNLOCK TABLES;
```

## Start the Slave

- Once the data has been imported, you are ready to start replicating. Begin by running a [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to), making sure that <em>MASTER_LOG_FILE</em> matches the file and <em>MASTER_LOG_POS</em> the position returned by the earlier SHOW MASTER STATUS. For example:

```sql
CHANGE MASTER TO
  MASTER_HOST='master.domain.com',
  MASTER_USER='replication_user',
  MASTER_PASSWORD='bigs3cret',
  MASTER_PORT=3306,
  MASTER_LOG_FILE='master1-bin.000096',
  MASTER_LOG_POS=568,
  MASTER_CONNECT_RETRY=10;
```

If you are starting a slave against a fresh master that was configured for replication from the start, then you don't have to specify `MASTER_LOG_FILE` and `MASTER_LOG_POS`.

### Use Global Transaction Id (GTID)

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

[MariaDB 10.0](/kb/en/what-is-mariadb-100/) introduced global transaction IDs (GTIDs) for replication. It is generally recommended to use (GTIDs) from [MariaDB 10.0](/kb/en/what-is-mariadb-100/), as this has a number of benefits. All that is needed is to add the `MASTER_USE_GTID` option to the `CHANGE MASTER` statement, for example:

```sql
CHANGE MASTER TO MASTER_USE_GTID = slave_pos
```

See [Global Transaction ID](/kb/en/global-transaction-id/) for a full description.

- Now start the slave with the [`START SLAVE`](/kb/en/start-slave/) command:

```sql
START SLAVE;
```

- Check that the replication is working by executing the [`SHOW SLAVE STATUS`](/kb/en/show-slave-status/) command:

```sql
SHOW SLAVE STATUS \G
```

- If replication is working correctly, both the values of `Slave_IO_Running` and `Slave_SQL_Running` should be `Yes`:

```sql
Slave_IO_Running: Yes
Slave_SQL_Running: Yes
```

## Replicating from MySQL Master to MariaDB Slave

- Replicating from MySQL 5.5 to [MariaDB 5.5](/kb/en/what-is-mariadb-55/)+ should just work. When using a [MariaDB 10.2](/kb/en/what-is-mariadb-102/)+ as a slave, it may be necessary to set [binlog_checksum](/kb/en/replication-and-binary-log-server-system-variables/#binlog_checksum) to NONE.
- Replicating from MySQL 5.6 without GTID to MariaDB 10+ should work.
- Replication from MySQL 5.6 with GTID, binlog_rows_query_log_events and ignorable events works starting from [MariaDB 10.0.22](/kb/en/mariadb-10022-release-notes/) and [MariaDB 10.1.8](/kb/en/mariadb-1018-release-notes/). In this case MariaDB will remove the MySQL GTIDs and other unneeded events and instead adds its own GTIDs.

## See Also

- [Differences between Statement-based, mixed and row logging](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats)
- [Replication and Foreign Keys](/replication/standard-replication/replication-and-foreign-keys)
- [Replication as a Backup Solution](/mariadb-administration/backing-up-and-restoring-databases/replication-as-a-backup-solution)
- [Multi-source Replication](/replication/standard-replication/multi-source-replication)
- [Global Transaction ID](/kb/en/global-transaction-id/)
- [Parallel Replication](/replication/standard-replication/parallel-replication)
- [Replication and Binary Log System Variables](/kb/en/replication-and-binary-log-server-system-variables/)
- [Replication and Binary Log Status Variables](/replication/standard-replication/replication-and-binary-log-status-variables)
- [Semisynchronous Replication](/replication/standard-replication/semisynchronous-replication)
- [Delayed Replication](/replication/standard-replication/delayed-replication)
- [Replication Compatibility](/kb/en/mariadb-vs-mysql-compatibility/#replication-compatibility)