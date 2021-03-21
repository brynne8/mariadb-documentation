# Replication Threads

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

MariaDB's [replication](/kb/en/high-availability-performance-tuning-mariadb-replication/) implementation requires several types of threads.

## Threads on the Master

The master usually only has one type of replication-related thread: the binary log dump thread.

If [semisynchronous replication](/replication/standard-replication/semisynchronous-replication/) is enabled, then the master also has an ACK receiver thread.

### Binary Log Dump Thread

The binary log dump thread runs on the master and dumps the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) to the slave. This thread can be identified by running the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) statement and finding the thread where the [thread command](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-command-values/) is `"Binlog Dump"`.

The master creates a separate binary log dump thread for each slave connected to the master. You can identify which slaves are connected to the master by executing the <a undefined>SHOW SLAVE HOSTS</a> statement.

#### Binary Log Dump Threads and the Shutdown Process

When a master server is shutdown and it goes through the normal shutdown process, the master kills client threads in random order. By default, the master also considers its binary log dump threads to be regular client threads. As a consequence, the binary log dump threads can be killed while client threads still exist, and this means that data can be written on the master during a normal shutdown that won't be replicated. This is true even if [semi-synchronous replication](/replication/standard-replication/semisynchronous-replication/) is being used.

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this problem can be solved by shutting down the server using either the [mysqladmin](/clients-utilities/mysqladmin/) utility or the [SHUTDOWN](/sql-statements-structure/sql-statements/administrative-sql-statements/shutdown/) command, and providing a special option.

For example, this problem can be solved by shutting down the server with the [mysqladmin](/clients-utilities/mysqladmin/) utility and by providing the `--wait-for-all-slaves` option to the utility and by executing the `shutdown` command with the utility:

```sql
mysqladmin --wait-for-all-slaves shutdown
```

Or this problem can be solved by shutting down the server with the [SHUTDOWN](/sql-statements-structure/sql-statements/administrative-sql-statements/shutdown/) command and by providing the `WAIT FOR ALL SLAVES` option to the command:

```sql
SHUTDOWN WAIT FOR ALL SLAVES;
```

When one of these special options is provided, the server only kills its binary log dump threads after all client threads have been killed, and it only completes the shutdown after the last [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) has been sent to all connected slaves.

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, it is still not possible to enable this behavior by default. This means that this behavior is currently inaccessible when shutting down the server using tools like [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) or [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit/).

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, it is recommended to manually switchover slaves to a new master before shutting down the old master.

### ACK Receiver Thread

When [semisynchronous replication](/replication/standard-replication/semisynchronous-replication/) is enabled, semisynchronous slaves send acknowledgements (ACKs) to their master confirm that they have received some transaction. The master creates an ACK receiver thread to receive these ACKs.

## Threads on the Slave

The slave has three types of replication-related threads: the slave I/O thread, the slave SQL thread, and worker threads, which are only applicable when [parallel replication](/replication/standard-replication/parallel-replication/) is in use.

When [multi-source replication](/replication/standard-replication/multi-source-replication/) is in use, each independent replication connection has its own slave threads of each type.

### Slave I/O Thread

The slave's I/O thread receives the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) events from the master and writes them to its [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/).

#### Binary Log Position

The [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position of the slave's I/O thread can be checked by executing the <a undefined>SHOW SLAVE STATUS</a> statement. It will be shown as the `Master_Log_File` and `Read_Master_Log_Pos` columns.

The [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position of the slave's I/O thread can be set by setting the <a undefined>MASTER_LOG_FILE</a> and <a undefined>MASTER_LOG_POS</a> options with the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) statement.

The [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position of the slave's I/O thread and the values of most other [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) options are written to either the default `master.info` file or the file that is configured by the <a undefined>master_info_file</a> option. The slave's I/O thread keeps this [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position updated as it downloads events. See [CHANGE MASTER TO: Option Persistence](/kb/en/change-master-to/#option-persistence) for more information

### Slave SQL Thread

The slave's SQL thread reads events from the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/). What it does with them depends on whether [parallel replication](/replication/standard-replication/parallel-replication/) is in use. If [parallel replication](/replication/standard-replication/parallel-replication/) is not in use, then the SQL thread applies the events to its local copy of the data. If [parallel replication](/replication/standard-replication/parallel-replication/) is in use, then the SQL thread hands off the events to its worker threads to apply in parallel.

#### Relay Log Position

The [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) position of the slave's SQL thread can be checked by executing the <a undefined>SHOW SLAVE STATUS</a> statement. It will be shown as the `Relay_Log_File` and `Relay_Log_Pos` columns.

The [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) position of the slave's SQL thread can be set by setting the <a undefined>RELAY_LOG_FILE</a> and <a undefined>RELAY_LOG_POS</a> options with the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) statement.

The [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) position of the slave's SQL thread is written to either the default `relay-log.info` file or the file that is configured by the <a undefined>relay_log_info_file</a> system variable. The slave's SQL thread keeps this [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) position updated as it applies events. See [CHANGE MASTER TO: Option Persistence](/kb/en/change-master-to/#option-persistence) for more information.

#### Binary Log Position

The corresponding [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position of the current [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) position of the slave's SQL thread can be checked by executing the <a undefined>SHOW SLAVE STATUS</a> statement. It will be shown as the `Relay_Master_Log_File` and `Exec_Master_Log_Pos` columns.

#### GTID Position

If the slave is replicating [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) events that contain [GTIDs](/replication/standard-replication/gtid/), then the [slave's SQL thread](/kb/en/replication-threads/#slave-sql-thread) will write every GTID that it applies to the <a undefined>mysql.gtid_slave_pos</a> table. This GTID can be inspected and modified through the <a undefined>gtid_slave_pos</a> system variable.

If the slave has the <a undefined>log_slave_updates</a> system variable enabled and if the slave has the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) enabled, then every write by the [slave's SQL thread](/kb/en/replication-threads/#slave-sql-thread) will also go into the slave's [binary log](/mariadb-administration/server-monitoring-logs/binary-log/). This means that [GTIDs](/replication/standard-replication/gtid/) of replicated transactions would be reflected in the value of the <a undefined>gtid_binlog_pos</a> system variable.

See [CHANGE MASTER TO: GTID Persistence](/kb/en/change-master-to/#gtid-persistence) for more information.

### Worker Threads

When [parallel replication](/replication/standard-replication/parallel-replication/) is in use, then the SQL thread hands off the events to its worker threads to apply in parallel.