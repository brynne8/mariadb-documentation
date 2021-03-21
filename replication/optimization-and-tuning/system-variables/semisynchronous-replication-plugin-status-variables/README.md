# Semisynchronous Replication Plugin Status Variables

This page documents status variables related to the [Semisynchronous Replication Plugin](/replication/standard-replication/semisynchronous-replication/) (which has been merged into the main server from [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)). See [Server Status Variables](/replication/optimization-and-tuning/system-variables/server-status-variables/) for a complete list of status variables that can be viewed with [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status/).

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `Rpl_semi_sync_master_clients`

- <strong>Description:</strong> Number of semisynchronous slaves.
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_master_net_avg_wait_time`

- <strong>Description:</strong> Average time the master waited for a slave to reply, in microseconds.
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_master_net_wait_time`

- <strong>Description:</strong> Total time the master waited for slave replies, in microseconds.
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_master_net_waits`

- <strong>Description:</strong> Total number of times the master waited for slave replies.
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_master_no_times`

- <strong>Description:</strong> Number of times the master turned off semisynchronous replication. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_master_no_tx`

- <strong>Description:</strong> Number of commits that have not been successfully acknowledged by a slave. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_master_status`

- <strong>Description:</strong> Whether or not semisynchronous replication is currently operating on the master. The value will be `ON` if both the plugin has been enabled and a commit acknowledgment has occurred. It will be `OFF` if either the plugin has not been enabled, or the master is replicating asynchronously due to a commit acknowledgment timeout.
- <strong>Data Type:</strong> `boolean`

---

#### `Rpl_semi_sync_master_timefunc_failures`

- <strong>Description:</strong> Number of times the master failed when calling a time function, such as gettimeofday(). The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_master_tx_avg_wait_time`

- <strong>Description:</strong> Average time the master waited for each transaction, in microseconds.
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_master_tx_wait_time`

- <strong>Description:</strong> Total time the master waited for transactions, in microseconds.
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_master_tx_waits`

- <strong>Description:</strong> Total number of times the master waited for transactions.
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_master_wait_pos_backtraverse`

- <strong>Description:</strong> Total number of times the master waited for an event that had binary coordinates lower than previous events waited for. Occurs if the order in which transactions start waiting for a reply is different from the order in which their binary log events were written. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_master_wait_sessions`

- <strong>Description:</strong> Number of sessions that are currently waiting for slave replies.
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_master_yes_tx`

- <strong>Description:</strong> Number of commits that have been successfully acknowledged by a slave. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Data Type:</strong> `numeric`

---

#### `Rpl_semi_sync_slave_status`

- <strong>Description:</strong>  Whether or not semisynchronous replication is currently operating on the slave. `ON` if both the plugin has been enabled and the slave I/O thread is running.
- <strong>Data Type:</strong> `boolean`

---