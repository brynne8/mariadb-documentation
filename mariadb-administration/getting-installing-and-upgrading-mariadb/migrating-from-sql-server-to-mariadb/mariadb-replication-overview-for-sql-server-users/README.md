# MariaDB Replication Overview for SQL Server Users

MariaDB supports the following types of replication:

- Asynchronous replication.
- Semi-synchronous replication.
- Galera Cluster.

##### MariaDB starting with [10.5.1](/kb/en/mariadb-1051-release-notes/)

Note: in the snippets in this page, several SQL statements use the keyword `SLAVE`. This word is considered inappropriate by some persons or cultures, so from [MariaDB 10.5](/kb/en/what-is-mariadb-105/) it is possible to use the `REPLICA` keyword, as a synonym.

Similar synonyms will be created in the future for status variables and system variables. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to track the status of these changes.

## Asynchronous Replication

The original MariaDB replication system is asynchronous master-slave replication.

A master needs to have the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) enabled. The master logs all data changes in the binary log. Every <em>event</em> (a binary log entry) is sent to all the slaves.

For a high-level description of the binary log for SQL Server users, see [Understanding MariaDB Architecture](/kb/en/understanding-mariadb-architecture/#the-binary-log).

The events can be written in two formats: as an SQL statement (<em>statement-based replication</em>, or SBR), or as a binary representation of the change (<em>row-based replication</em>, or RBR). The former is generally slower, because the statement needs to be re-executed by the slaves. It is also less reliable, because some SQL statements are [not deterministic](/replication/standard-replication/unsafe-statements-for-statement-based-replication), so they could produce different results on the slaves. On the other hand row-based replication could make the binary log much bigger, and require more network traffic. For this reason, DML statements are always logged in statement format.

For more details on replication formats, see [binary log formats](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats).

The slaves have an [I/O thread](/kb/en/replication-threads/#slave-io-thread) that receives the binary log events and writes them to the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log). These events are then read by the [SQL thread](/kb/en/replication-threads/#slave-sql-thread). This thread could directly apply the changes to the local databases, and this was the only option before [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/). If [parallel replication](#parallel-replication-and-group-commit) is enabled, the SQL thread hands the events to the worker thread, that apply them to the databases. The latter method is recommended for performance reasons.

When a slave cannot apply an event to the local data, the SQL thread stops. This happens, for example, if the event is a row deletion but that row doesn't exist on the slave. There can be several reasons for this, for example non-deterministic statements, or a user deleted the row in the slave. To reduce the risk, it is recommended to set [read_only](/kb/en/server-system-variables/#read_only) to 1 in the slaves.

[SHOW SLAVE STATUS](/kb/en/show-slave-status/) has columns named `Slave_SQL_State` and `Slave_IO_State` that show, respectively, if the SQL thread and the IO thread are running. If they are not, the column `Last_IO_Errno` and `Last_IO_Error` (for the IO thread) or `Last_SQL_Errno` and `Last_SQL_Error` (for the SQL thread) show what the problem is.

In a replication chain, every server must have a unique [server_id](/kb/en/replication-and-binary-log-system-variables/#server_id).

For more information on replication, see [standard replication](/replication/standard-replication).

### Binary Log Coordinates, Relay Log Coordinates and GTID

The binary log coordinates provide a way to identify a certain data change made by a server. Coordinates consist of a file name and the position of the latest event, expressed as an integer. The last event coordinates can be seen with the [SHOW MASTER STATUS](/kb/en/show-master-status/) columns `File` and `Position`. [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump) includes them in a dump if the `--master-data` option is used.

A slave uses master binary log coordinates to identify the last event it read. This can be seen with the [SHOW SLAVE STATUS](/kb/en/show-slave-status/),columns `Master_Log_File` and `Read_Master_Log_Pos`.

The columns `Relay_Master_Log_File` and `Exec_Master_Log_Pos` identify the master event that corresponds to the last event applied by the SQL thread.

The slave relay log also has coordinates. The coordinates of the last applied event can be seen with the `SHOW SLAVE STATUS` columns `Relay_Log_File` and `Relay_Log_Pos`.

To easily find out how far the slave is lagging behind the master, we can look at `Seconds_Behind_Master`.

Coordinates represented in this way have a problem: they are different on each server. Each server can use files with different (or the same) names, depending on its configuration. And files can be rotated at different times, including when a user runs [FLUSH LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush). By enabling the GTID (global transaction id) an event will have the same id on the master and on all the slaves.

When [GTID](/replication/standard-replication/gtid) is enabled, `SHOW SLAVE STATUS` shows two GTIDs: `Gtid_IO_Pos` is the last event written into the relay log, and `Gtid_Slave_Pos` is the last event applied by the SQL thread. There is no need for a column identifying the same event in the master, because the id is the same.

### Provisioning a Slave

MariaDB does not have an equivalent to SQL Server's snapshot replication.

To setup a slave, it is necessary to manually provision it. It can be provisioned from the master in this way:

- A backup from the master must be restored on the new slave;
- The binary log coordinates at the moment of the backup should be set as replication coordinates in the slave, via [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to).

However, if there is at least one existing slave, it is better to use it to provision the new slave:

- A backup from the existing slave must be restored in the new slave;
- The backup should include the system tables. In this way it will not be necessary to set the correct coordinates manually.

For more information see [Setting Up Replication](/replication/standard-replication/setting-up-replication) and [Setting up a Replication Slave with Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/setting-up-a-replication-slave-with-mariabackup).

### Replication and Permissions

A slave connects to a master using its credentials. See [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to).

The appropriate account must be created in the master, and it needs to have the `REPLICATION SLAVE` permission.

See [Setting Up Replication](/replication/standard-replication/setting-up-replication) for more information.

### Parallel Replication and Group Commit

MariaDB uses [group commit](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log), which means that a group of events are physically written in the binary log altogether. This reduces the number of IOPS (input/output operations per second). Group commit cannot be disabled, but it can be tuned with variables like [binlog_commit_wait_count](/kb/en/replication-and-binary-log-system-variables/#binlog_commit_wait_count) and [binlog_commit_wait_usec](/kb/en/replication-and-binary-log-system-variables/#binlog_commit_wait_usec).

Slaves can apply the changes using multiple threads. This is known as [parallel replication](/replication/standard-replication/parallel-replication). Before [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/) only one thread was used to apply changes. Since a master can use many threads to write data, mono-thread replication is a well-known bottleneck. Parallel replication is not enabled by default. To use it, set the [slave_parallel_threads](/kb/en/replication-and-binary-log-system-variables/#slave_parallel_threads) variable to a number greater than 1. If replication is running, the slave threads must be stopped in order to change this value:

```sql
STOP SLAVE SQL_THREAD;
SET GLOBAL slave_parallel_threads = 4;
START SLAVE SQL_THREAD;
```

There are different parallel replication styles available: in-order and out-of-order. The exact mode in use is determined by the [slave_parallel_mode](/kb/en/replication-and-binary-log-system-variables/#slave_parallel_mode) system variable. In parallel replication, the events are not replicated exactly in the same order as they occurred in the master. But with an in-order replication mode the commit phase is always applied simultaneously. In this way data in the slave always reflect data as they have been in the master at a certain point in time. Out-of-order replication is faster because there is less queuing, but it's not completely consistent with the master. If two transactions modified different sets of rows in the master, they could become visible in the slave in a different order.

`conservative` relies on master group commit: events in different groups are executed in a parallel way.

`optimistic` does not try to find out which transaction can be executed in a parallel way - except for transactions that conflicted on the master. Instead, it always tries to apply many events together, and rolls transactions back when there is a conflict.

`aggressive` is similar to optimistic, but it does not take into account which transactions conflicted in the master.

`minimal` applies commits together, but all other events are applied in order.

Out-of-order replication cannot be enabled automatically by changing a variable in the slave. Instead, it must be enabled by the applications that run transactions in the master. They can do this if the GTID is enabled. They can set different values for the [gtid_domain_id](/kb/en/replication-and-binary-log-system-variables/#gtid_domain_id) variable in different transactions. This shifts a lot of responsibility to the application layer; however, if the application is aware of which transactions are not going to conflict and this information allows one to sensibly increase the parallelism, and using out-of-order replication can be a good idea.

Even if out-of-order replication is not normally used, it can be a good idea to use it for long running transactions or [ALTER TABLEs](/sql-statements-structure/sql-statements/data-definition/alter/alter-table), so they can be applied at the same time as normal operations that are not conflicting.

The impact of the number of threads and mode on performance can be partly seen with [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist), which shows the state of all threads. This includes the replication worker threads, and shows if they are blocking each other.

### Differences Between the Master and the Slaves

As a general rule, we want the master and the slaves to contain exactly the same data. In this way, no conflicts are possible. Conflicts are the most likely cause of replication outages.

To reduce the possible causes of conflicts, the following best practices are recommended:

- Users must not change data in the slaves directly. Set [read_only](/kb/en/server-system-variables/#read_only) to 1. Note that this won't prevent root from making changes.
- Use the same table definitions in the master and in the slave.
- Use `ROW` binary log format on the master.

Another cause of inconsistencies include MariaDB bugs and failover in case the master crashes.

An open source third party tool is available to check if the master and a slave are consistent. It is called pt-table-checksum. Another tool, pt-table-sync, can be used to eliminate the differences. Both are part of Percona Toolkit. The advice is to run pt-table-checksum periodically, and use pt-table-sync if inconsistencies are found.

If a replication outage occurs because an inconsistency is found, sometimes we want to quickly bring the slave up again as quickly as possible, and solve the core problem later. If GTID is not used, a way to do this is to run [SET GLOBAL SQL_SLAVE_SKIP_COUNTER = 1](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/set-global-sql_slave_skip_counter), which skips the problematic replication event.

If GTID is used, the [gtid_slave_pos](/kb/en/gtid/#gtid_slave_pos) variable can be used instead. See the link for an explanation of how it works.

There are ways to have different data on the slaves. For example:

- Multi-source replication is possible. In this way, a slave will replicate data from multiple masters. This feature is described below.
- [Replication filters](/replication/standard-replication/replication-filters) are supported. This allows one to exclude or include in replication specific tables, entire databases, or tables whose name matches a certain pattern. This allows one to avoid replicating data that is present in the master but can always be rebuilt.
- [Differences in table definitions](/replication/standard-replication/replication-when-the-master-and-slave-have-different-table-definitions) are also possible. For example, a slave could have [more columns or less columns](/kb/en/replication-when-the-master-and-slave-have-different-table-definitions/#different-number-or-order-of-columns) compared to the master. In this way we can avoid replicating columns whose values can be rebuilt. Or we can add columns for analytics purposes, without having them in the master. Be sure to understand the limitations and risks of this technique.

### Delayed Replication

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

Delayed replication was introduced in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

MariaDB supports delayed replication. This is the equivalent of setting a `pollinginterval` in SQL Server.

To delay replication in a MariaDB slave, use [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) to specify a delay in seconds.

For more information, see [Delayed Replication](/replication/standard-replication/delayed-replication).

### Multi-Source Replication

[Multi-source replication](/replication/standard-replication/multi-source-replication) is an equivalent to peer-to-peer replication, available in SQL Server Enterprise Edition.

A MariaDB slave can replicate from any number of masters. It is very important that different masters don't have the same tables. Otherwise there could be conflicts between data changes made on different masters, and this will result in a replication outage.

In multi-source replication different channels exist, one for each master.

This changed the way [SQL replication statements](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands) work. [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist) returns a different row for each channel. Several statements, like [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to), [START SLAVE](/kb/en/start-slave/) or [STOP SLAVE](/kb/en/stop-slave/). accept a parameter which specifies which replication channel they should affect. For example, to stop a channel called `wp1`:

```sql
STOP SLAVE "wp1";
```

Furthermore, variables that affect parallel replication can be prefixed with a channel name. This allow one to only use parallel replication for certain channels, or to tune it differently for each channel. For example, to enable parallel replication on a channel called `wp1`:

```sql
SET GLOBAL wp1.slave_parallel_threads = 4;
```

### Dual Master

It is possible to configure two servers in a way that each of them acts as a master for the other server.

In this way, data could theoretically be inserted into any of these servers, and will then be replicated to the other server. However, in such a configuration conflicts are very likely. So it is impractical to use this technique to scale writes.

A dual master (or master-master) configuration however can be useful for failover. In this case we talk about an <em>active master</em> that receives reads and writes from the clients, and a <em>passive master</em> that is not used until the active master crashes.

Several problems should be considered in this scenario:

- If the active master crashes, it is very possible that the passive master did not receive all events yet, because replication is asynchronous. If the master data are lost (for example because the disk is damaged), some data are also lost.
- If data is not lost, when we bring the master up again, the latest events will be replicated by the other server. There could be conflicts that will break replication.
- When is the active master considered down? Even if a server cannot reach it, the active master could be running and it could be able to communicate with the passive master. Switching the clients to the passive master could lead to unnecessary problems. It is a good idea to always check `SHOW SLAVE STATUS` to be sure that the two masters are not communicating.
- If we want to have more slaves, we should attach some of them to the active master, and some of them to the passive master. The reason is that when a server crashes, its slaves stop receiving any data. Failover is still possible, but it's better to have some servers that will not need any failover.

A safe master-master configuration where both servers accept writes, however, is possible. This is the case is data never conflicts. For example, the two servers could accept writes on different databases. We will have to decide what should happens in case of a server crash:

- Writes can be stopped until the server is up again. Reads can be sent to the other server, but keep in mind that the most recently written data could be missing.
- Both writes and reads can failover to the other server. All the problems mentioned above may apply to this situation.

See Sveta Smirnova's slides at MariaDB Day 2020: "[How Safe is Asynchronous Master-Master Setup?](https://www.slideshare.net/SvetaSmirnova/how-safe-is-asynchronous-mastermaster-setup)".

## Semi-Synchronous Replication

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

Semi-synchronous replication was initially implemented as a plugin, in MySQL. Two different plugins needed to be used, one on the master and the other on the slaves. Starting from [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/) it is built-in, which improved its performance.

The problem with standard replication is that there is no guarantee that it will not lag, even by long amounts of time. [Semi-synchronous replication](/replication/standard-replication/semisynchronous-replication) reduces this problem, at the cost of reducing the speed of the master.

In semi-synchronous replication, when a transaction is committed on the master, the master does not immediately return control to the client. Instead, it sends the event to the slaves. After one slave reported that the commit was executed with success, the master reports success to the client.

Semi-synchronous replication is useful for failover, therefore a dual master setup is not needed in this case. If the master crashes, the most up-to-date slave can be promoted to master without losing any data.

### Enabling Semi-Synchronous Replication

Semi-synchronous replication can be enabled at runtime in this way on the master:

```sql
SET GLOBAL rpl_semi_sync_master_enabled = ON;
```

Semi-synchronous replication is not used until it has been enabled on the slaves also. If the slaves are already replicating, the io_thread needs to be stopped and restarted. This can be done as follows:

```sql
SET GLOBAL rpl_semi_sync_slave_enabled = ON;
STOP SLAVE IO_THREAD;
START SLAVE IO_THREAD;
```

### Tuning the Wait Point and the Master Timeout

The most important aspects to tune are the wait point and the master timeout.

When the binary log is enabled, transactions must be committed both in the [storage engine](/kb/en/understanding-mariadb-architecture/#storage-engines) (usually [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb)) and in the [binary log](/kb/en/understanding-mariadb-architecture/#the-binary-log). Semi-synchronous replication requires that the transaction is also acknowledged by at least one slave before the master can report success to the client.

The wait point determines at which point the master must stop and wait for a confirmation from a slave. This is an important decision from disaster recovery standpoint, in case the master crashes when a transaction is not fully committed. The [rpl_semi_sync_master_wait_point](/kb/en/semisynchronous-replication/#rpl_semi_sync_master_wait_point) is used to set the wait point, Its allowed values are:

- `AFTER_SYNC`: After committing the transaction in the binary log, but before committing it to the storage engine. After a crash, a transaction may be present in the binary log even if it was not committed.
- `AFTER_COMMIT`. After committing a transaction both in the binary log and in the storage engine. In case of a crash, a transaction could possibly be committed in the master but not replicated in the slaves. This is the default.

Master timeout is meant to avoid that a master remains stuck for a long time, or virtually forever, because no slave acknowledges a transaction. If master timeout is reached, the master switches to asynchronous replication. Before doing that, the master writes an error in the [error log](/mariadb-administration/server-monitoring-logs/error-log) and increments the [Rpl_semi_sync_master_no_times](/kb/en/semisynchronous-replication-plugin-status-variables/#rpl_semi_sync_master_no_times) status variable.

The timeout is set via the [rpl_semi_sync_master_timeout](/kb/en/semisynchronous-replication/#rpl_semi_sync_master_timeout) variable.

## Galera Cluster

[Galera](/replication/galera-cluster) is a technology that implements virtually synchronous, master-master replication for a cluster of MariaDB servers.

### Raft and the Primary Cluster

Nodes of the cluster communicate using the Raft protocol. In case the cluster is partitioned or some nodes crash, a cluster knows that it's still the <em>primary cluster</em> if it has the <em>quorum</em>: half of the nodes + 1. Only the primary cluster accepts reads and writes.

For this reason a cluster should consist of an odd number of nodes. Imagine for example that a cluster consists of two nodes: if one of them crashes of the connection between them is interrupted, there will be no primary cluster.

### Transaction Certification

A transaction can be executed against any node. The node will use a 2-phase commit. After running the transaction locally, the node will ask other nodes to <em>certify</em> it. This means that other nodes will receive it, and will try to apply it, and will report success or a failure. The node that received the transaction will not wait for an answer from all the nodes. Once it succeeded on more than half of the nodes (the quorum) the node will run the final commit and data becomes visible.

It is desirable to write data on only one node (unless it fails), or write different databases on different nodes. This will minimize the risk of conflicts.

### Galera Cache and SST

Data changes applied are recorded for some time in the <em>Galera cache</em>. This is an on-disk cache, written in a circularly written file.

The size of Galera cache can be tuned using the [wsrep_provider_options](/replication/galera-cluster/wsrep_provider_options) system variable, which contains many flags. We need to tune [gcache.size](/kb/en/wsrep_provider_options/#gcachesize). To tune it, add a line similar to the following to a configuration file:

```sql
wsrep_provider_options = 'gcache.size=2G';
```

If a single transaction is bigger than half of the Galera cache, it needs to be written in a separate file, as <em>on-demand pages</em>. On-demand pages are regularly replaced. Whether a new page replaces an old one depends on another wsrep_provider_options flag: `wsrep_provider_options#gcachekeep_pages_size|gcache.keep_pages_size`, which limits the total size of on-demand pages.

When a node is restarted (after a crash or for maintenance reasons), it will need to receive all the changes that were written by other nodes since the moment it was unreachable. A node is therefore chosen as a donor, possibly using the [gcssync_donor](/kb/en/wsrep_provider_options/#gcssync_donor) wsrep_provider_options flag.

If possible, the donor will send all the recent changes, reading them from the Galera cache and on-demand pages. However, sometimes the Galera cache is not big enough to contain all the needed changes, or the on-demand pages have been overwritten because `gcache.keep_pages_size` is not big enough. In these cases, a [State Snapshot Transfer (SST)](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster) needs to be sent. This means that the donor will send the whole dataset to the restarted node. Most commonly, this happens using the [mariabackup method](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/mariabackup-sst-method).

### Flow Control

While transaction certification is synchronous, certified transactions are applied locally in asynchronous fashion. However, a node should never lag too much behind others. To avoid that, a node may occasionally trigger a mechanism called <em>flow control</em> to ask other nodes to stop replication until its situation improves. Several [wsrep_provider_options](/replication/galera-cluster/wsrep_provider_options) flags affect flow control.

[gcs.fc_master_slave](/kb/en/wsrep_provider_options/#gcsfc_master_slave) should normally be set to 1 if all writes are set to a single node.

[gcs.fc_limit](/kb/en/wsrep_provider_options/#gcsfc_limit) is tuned automatically, unless [gcs.fc_master_slave](/kb/en/wsrep_provider_options/#gcsfc_master_slave) is set to 0. The <em>receive queue</em> (the transactions received and not yet applied) should not exceed this limit. When this happens, flow control is triggered by the node to pause other node's replication.

Once flow control is activated, [gcs.fc_factor](/kb/en/wsrep_provider_options/#gcsfc_factor) determines when it is released. It is a number from 0 to 1, and it represents a fraction. When the receive queue is below this fraction, the flow control is released.

Flow control and the receive queue can and should be monitored. The most useful metrics are:

- [wsrep_flow_control_paused](/kb/en/galera-cluster-status-variables/#wsrep_flow_control_paused) indicates how many times the replication has been paused as requested by other nodes, since the last `FLUSH STATUS`.
- [wsrep_flow_control_sent](/kb/en/galera-cluster-status-variables/#wsrep_flow_control_sent) indicates how many times this node requested other nodes to pause replication.
- [wsrep_local_recv_queue](/kb/en/galera-cluster-status-variables/#wsrep_local_recv_queue) is the size of the receive queue.

### Configuration

Galera is implemented as a plugin. Starting from version 10.1, MariaDB comes with Galera pre-installed, but not in use by default. To enable it one has to set the [wsrep_on](/kb/en/galera-cluster-system-variables/#wsrep_on) system variable.

Like asynchronous replication, Galera uses the binary log. It also requires that data changes are logged in the `ROW` format.

For other required settings, see [Mandatory Options](/kb/en/configuring-mariadb-galera-cluster/#mandatory-options).

## Galera Limitations

Galera is not suitable for all databases and workloads.

- Galera only replicates [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables. Other storage engines should not be used.
- For performance reasons, it is highly desirable that all tables have a primary key.
- Long transactions will damage performance.
- Some applications use an integer [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment) primary key. In case of failover from a crashed node to another, Galera does not guarantee that `AUTO_INCREMENT` follows a chronological order. Therere, applications should use [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp) columns for chronological order instead.