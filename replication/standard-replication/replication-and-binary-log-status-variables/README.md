# Replication and Binary Log Status Variables

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

The following status variables are useful in [binary logging](/mariadb-administration/server-monitoring-logs/binary-log) and [replication](/replication). See [Server Status Variables](/replication/optimization-and-tuning/system-variables/server-status-variables) for a complete list of status variables that can be viewed with [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status).

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables).

#### `Binlog_bytes_written`

- <strong>Description:</strong> The number of bytes written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Binlog_cache_disk_use`

- <strong>Description:</strong> Number of transactions which used a temporary disk cache because they could not fit in the regular [binary log](/mariadb-administration/server-monitoring-logs/binary-log) cache, being larger than [binlog_cache_size](/kb/en/server-system-variables/#binlog_cache_size). The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Binlog_cache_use`

- <strong>Description:</strong> Number of transaction which used the regular [binary log](/mariadb-administration/server-monitoring-logs/binary-log) cache, being smaller than [binlog_cache_size](/kb/en/server-system-variables/#binlog_cache_size). The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Binlog_commits`

- <strong>Description:</strong> Total number of transactions committed to the binary log.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Binlog_group_commit_trigger_count`

- <strong>Description:</strong> Total number of group commits triggered because of the number of binary log commits in the group reached the limit set by the variable [binlog_commit_wait_count](/kb/en/replication-and-binary-log-server-system-variables/#binlog_commit_wait_count). See [Group commit for the binary log](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/), [MariaDB 10.0.18](/kb/en/mariadb-10018-release-notes/)

---

#### `Binlog_group_commit_trigger_lock_wait`

- <strong>Description:</strong> Total number of group commits triggered because a binary log commit was being delayed because of a lock wait where the lock was held by a prior binary log commit. When this happens the later binary log commit is placed in the next group commit. See [Group commit for the binary log](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/), [MariaDB 10.0.18](/kb/en/mariadb-10018-release-notes/)

---

#### `Binlog_group_commit_trigger_timeout`

- <strong>Description:</strong> Total number of group commits triggered because of the time since the first binary log commit reached the limit set by the variable [binlog_commit_wait_usec](/kb/en/replication-and-binary-log-server-system-variables/#binlog_commit_wait_usec). See [Group commit for the binary log](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/), [MariaDB 10.0.18](/kb/en/mariadb-10018-release-notes/)

---

#### `Binlog_group_commits`

- <strong>Description:</strong> Total number of group commits done to the binary log. See [Group commit for the binary log](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Binlog_snapshot_file`

- <strong>Description:</strong> The binary log file. Unlike [SHOW MASTER STATUS](/kb/en/show-master-status/), can be queried in a transactionally consistent way, irrespective of which other transactions have been committed since the snapshot was taken. See [Enhancements for START TRANSACTION WITH CONSISTENT SNAPSHOT](/replication/standard-replication/enhancements-for-start-transaction-with-consistent-snapshot).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `string`

---

#### `Binlog_snapshot_position`

- <strong>Description:</strong> The binary log position. Unlike [SHOW MASTER STATUS](/kb/en/show-master-status/), can be queried in a transactionally consistent way, irrespective of which other transactions have been committed since the snapshot was taken. See [Enhancements for START TRANSACTION WITH CONSISTENT SNAPSHOT](/replication/standard-replication/enhancements-for-start-transaction-with-consistent-snapshot).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Binlog_stmt_cache_disk_use`

- <strong>Description:</strong> Number of non-transaction statements which used a temporary disk cache because they could not fit in the regular [binary log](/mariadb-administration/server-monitoring-logs/binary-log) cache, being larger than [binlog_stmt_cache_size](/kb/en/server-system-variables/#binlog_stmt_cache_size). The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Binlog_stmt_cache_use`

- <strong>Description:</strong> Number of non-transaction statement which used the regular [binary log](/mariadb-administration/server-monitoring-logs/binary-log) cache, being smaller than [binlog_stmt_cache_size](/kb/en/server-system-variables/#binlog_stmt_cache_size). The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Com_change_master`

- <strong>Description:</strong> Number of [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) statements executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_binlog_status`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Com_show_master_status`

- <strong>Description:</strong> Number of [SHOW MASTER STATUS](/kb/en/show-master-status/) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Com_show_new_master`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `Com_show_slave_hosts`

- <strong>Description:</strong> Number of [SHOW SLAVE HOSTS](/kb/en/show-slave-hosts/) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_slave_status`

- <strong>Description:</strong> Number of [SHOW SLAVE STATUS](/kb/en/show-slave-status/) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_slave_start`

- <strong>Description:</strong> Number of [START SLAVE](/kb/en/start-slave/) commands executed. Removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/), see [Com_start_slave](#com_start_slave).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> <a undefined>MariaDB 10.0</a>

---

#### `Com_slave_stop`

- <strong>Description:</strong> Number of [STOP SLAVE](/kb/en/stop-slave/) commands executed. Removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/), see [Com_stop_slave](#com_stop_slave).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> <a undefined>MariaDB 10.0</a>

---

#### `Com_start_all_slaves`

- <strong>Description:</strong> Number of [START ALL SLAVES](/kb/en/start-slave/) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Com_start_slave`

- <strong>Description:</strong> Number of [START SLAVE](/kb/en/start-slave/) commands executed. Replaces the old [Com_slave_start](#com_slave_start).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Com_stop_all_slaves`

- <strong>Description:</strong> Number of [STOP ALL SLAVES](/kb/en/stop-slave/) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Com_stop_slave`

- <strong>Description:</strong> Number of [STOP SLAVE](/kb/en/stop-slave/) commands executed. Replaces the old [Com_slave_stop](#com_slave_stop).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Master_gtid_wait_count`

- <strong>Description:</strong> Number of times MASTER_GTID_WAIT called.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Master_gtid_wait_time`

- <strong>Description:</strong> Total number of time spent in MASTER_GTID_WAIT.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Master_gtid_wait_timeouts`

- <strong>Description:</strong> Number of timeouts occurring in MASTER_GTID_WAIT.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Rpl_status`

- <strong>Description:</strong> For showing the status of fail-safe replication. Removed in MySQL 5.6, still present in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

---

#### `Rpl_transactions_multi_engine`

- <strong>Description:</strong> Number of replicated transactions that involved changes in multiple (transactional) storage engines, before considering the update of `mysql.gtid_slave_pos`. These are transactions that were already cross-engine, independent of the GTID position update introduced by replication. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)

---

#### `Slave_connections`

- <strong>Description:</strong> Number of REGISTER_SLAVE attempts.  In practice the number of times slaves has tried to connect to the master.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.11](/kb/en/mariadb-10111-release-notes/)

---

#### `Slave_heartbeat_period`

- <strong>Description:</strong> Time in seconds that a heartbeat packet is requested from the master by a slave.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Slave_open_temp_tables`

- <strong>Description:</strong> Number of temporary tables the slave has open.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Slave_received_heartbeats`

- <strong>Description:</strong> Number of heartbeats the slave has received from the master.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Slave_retried_transactions`

- <strong>Description:</strong> Number of times the slave has retried transactions since the server started. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Slave_running`

- <strong>Description:</strong> Whether the slave is running (both I/O and SQL threads running) or not.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Slave_skipped_errors`

- <strong>Description:</strong> The number of times a slave has skipped errors defined by [slave-skip-errors](/kb/en/replication-and-binary-log-server-system-variables/#slave_skip_errors).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.18](/kb/en/mariadb-10018-release-notes/)

---

#### `Slaves_connected`

- <strong>Description:</strong> Number of slaves connected.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.11](/kb/en/mariadb-10111-release-notes/)

---

#### `Slaves_running`

- <strong>Description:</strong> Number of slave SQL threads running.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.11](/kb/en/mariadb-10111-release-notes/)

---

#### `Transactions_gtid_foreign_engine`

- <strong>Description:</strong> Number of replicated transactions where the update of the `gtid_slave_pos` table had to choose a storage engine that did not otherwise participate in the transaction. This can indicate that setting [gtid_pos_auto_engines](/kb/en/global-transaction-id/#gtid_pos_auto_engines) might be useful. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)

---

#### `Transactions_multi_engine`

- <strong>Description:</strong> Number of transactions that changed data in multiple (transactional) storage engines. If this is significantly larger than [Rpl_transactions_multi_engine](#rpl_transactions_multi_engine), it indicates that setting [gtid_pos_auto_engines](gtid_pos_auto_engines) could reduce the need for cross-engine transactions. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)

---