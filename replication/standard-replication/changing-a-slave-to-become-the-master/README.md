# Changing a Slave to Become the Master

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

This article describes how to change a slave to become a master and optionally to set the old master as a slave for the new master.

A typical scenario of when this is useful is if you have set up a new
version of MariaDB as a slave, for example for testing, and want to
upgrade your master to the new version.

In MariaDB replication, a slave should be of a version same or newer than the master. Because of this, one should first upgrades all slaves to the latest version before changing a slave to be a master.  In some cases one can have a slave to be of an older version than the master, as long as one doesn't execute on the master any SQL commands that the slave doesn't understand. This is however not guaranteed between all major MariaDB versions.

Note that in the examples below, <code class="fixed" style="white-space:pre-wrap">[connection_name]</code> is used as the [name of the connection](/replication/standard-replication/multi-source-replication).  If you are not using named connections you can ignore this.

### Stopping the Original Master.

First one needs to take down the original master in such a way that the slave
has all information on the master.

If you are using [Semisynchronous Replication](/replication/standard-replication/semisynchronous-replication) you can just stop the server with the [SHUTDOWN](/sql-statements-structure/sql-statements/administrative-sql-statements/shutdown) command as the slaves should be automatically up to date.

If you are using [MariaDB MaxScale proxy](/mariadb-platform-x3/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/mariadb-enterprise/maxscale), then you [can use MaxScale](https://mariadb.com/resources/blog/mariadb-maxscale-2-2-introducing-failover-switchover-and-automatic-rejoin) to handle the whole process of taking down the master and replacing it with one of the slaves.

If neither of the above is true, you have to do this step manually:

#### Manually Take Down the Master

First we have to set the master to read only to ensure that there are no new updates on the master:

```sql
FLUSH TABLES WITH READ LOCK;
```

Note that you should not disconnect this session as otherwise the read lock will disappear and you have to start from the beginning.

Then you should check the current position of the master:

```sql
SHOW MASTER STATUS;
+--------------------+----------+--------------+------------------+
| File               | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+--------------------+----------+--------------+------------------+
| mariadb-bin.000003 |      343 |              |                  |
+--------------------+----------+--------------+------------------+
SELECT @@global.gtid_binlog_pos;
+--------------------------+
| @@global.gtid_binlog_pos |
+--------------------------+
| 0-1-2                    |
+--------------------------+
```

And wait until you have the same position on the slave:
(The following should be excepted on the slave)

```sql
SHOW SLAVE [connection_name] STATUS;
+-------------------+-------------------+
Master_Log_File     | narttu-bin.000003 +
Read_Master_Log_Pos | 343               +
Exec_Master_Log_Pos | 343               +
...
Gtid_IO_Pos          0-1-2              +
+-------------------+-------------------+
```

The most important information to watch are `Master_Log_File` and
`Exec_Master_Log_Pos` as when this matches the master, it signals
that all transactions has been committed on the slave.

Note that `Gtid_IO_Pos` on slave can contain many different positions
separated with ',' if the slave has been connected to many different
masters. What is important is that all the sequences that are on the
master is also on the slave.

When slave is up to date, you can then take the <strong>MASTER</strong> down. This should be on the same connection where you executed [FLUSH TABLES WITH READ LOCK](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).

```sql
SHUTDOWN;
```

### Preparing the Slave to be a Master

Stop all old connections to the old master(s) and reset <strong>read only
mode</strong>, if you had it enabled. You also want to save the values of
<a undefined>SHOW MASTER STATUS</a> and `gtid_binlog_pos`, as
you may need these to setup new slaves.

```sql
STOP ALL SLAVES;
RESET SLAVE ALL;
SHOW MASTER STATUS;
SELECT @@global.gtid_binlog_pos;
SET @@global.read_only=0;
```

### Reconnect Other Slaves to the New Master

On the other slaves you have point them to the new master (the slave you promoted to a master).

```sql
STOP SLAVE [connection_name];
CHANGE MASTER [connection_name] TO MASTER_HOST="new_master_name",
MASTER_PORT=3306, MASTER_USER='root', MASTER_USE_GTID=current_pos,
MASTER_LOG_FILE="XXX", MASTER_LOG_POS=XXX;
START SLAVE;
```

The <code class="fixed" style="white-space:pre-wrap">XXX</code> values for `MASTER_LOG_FILE` and `MASTER_LOG_POS` should be the values you got from the `SHOW MASTER STATUS` command you did when you finished setting up the slave.

### Changing the Old Master to be a Slave

Now you can upgrade the new master to a newer version of MariaDB and then
follow the same procedure to connect it as a slave.

When starting the original master, it's good to start the `mysqld`
executable with the `--with-skip-slave-start` and `--read-only`
options to ensure that no old slave configurations could cause any
conflicts.

For the same reason it's also good to execute the following commands
on the old master (same as for other slaves, but with some extra
security).  The `read_only` option below is there to ensure that old
applications doesn't by accident try to update the old master by mistake.
It only affects normal connections to the slave, not changes from the
new master.

```sql
set @@global.read_only=1;
STOP ALL SLAVES;
RESET MASTER;
RESET SLAVE ALL;
CHANGE MASTER [connection_name] TO MASTER_HOST="new_master_name",
MASTER_PORT=3306, MASTER_USER='root', MASTER_USE_GTID=current_pos,
MASTER_LOG_FILE="XXX", MASTER_LOG_POS=XXX;
START SLAVE;
```

### Moving Applications to Use New Master

You should now point your applications to use the new master.
If you are using the [MariaDB MaxScale proxy](/mariadb-platform-x3/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/mariadb-enterprise/maxscale), then you don't
have to do this step as MaxScale will take care of sending write request
to the new master.

### See Also

- [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) command
- [MaxScale Blog about using Switchover to swap a master and slave](https://mariadb.com/resources/blog/mariadb-maxscale-2-2-introducing-failover-switchover-and-automatic-rejoin)
- [Percona blog about how to upgrade slave to master](https://www.percona.com/blog/2015/12/01/upgrade-master-server-minimal-downtime)