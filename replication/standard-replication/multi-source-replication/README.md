# Multi-Source Replication

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

##### MariaDB starting with [10.0.1](/kb/en/mariadb-1001-release-notes/)

Multi-source replication means that one server has many masters from which it
replicates. This feature was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

<img src="/kb/en/multi-source-replication/+image/multi_source_replication_small" alt="multi_source_replication_small" title="multi_source_replication_small">

## New Syntax

You specify which master connection you want to work with by either specifying
the connection name in the command or setting
[default_master_connection](/kb/en/replication-and-binary-log-server-system-variables/#default_master_connection) to the connection you want to
work with.

The connection name may include any characters and should be less than 64
characters. Connection names are compared without regard to case (case
insensitive). You should preferably keep the connection name short as it will
be used as a suffix for relay logs and master info index files.

The new syntax introduced to handle many connections:

- <code class="highlight fixed" style="white-space:pre-wrap">[CHANGE MASTER ['connection_name'] TO ...](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/)</code>.  This creates or modifies a connection to a master.
- <code class="highlight fixed" style="white-space:pre-wrap">FLUSH RELAY LOGS ['connection_name']</code>
- <code class="highlight fixed" style="white-space:pre-wrap">[MASTER_POS_WAIT(....,['connection_name'])](/built-in-functions/secondary-functions/miscellaneous-functions/master_pos_wait/)</code>
- <code class="highlight fixed" style="white-space:pre-wrap">[RESET SLAVE ['connection_name'] [ALL](/kb/en/reset-slave/)]</code>.  This is used to reset slave replication position or to remove a slave permanently.
- <code class="highlight fixed" style="white-space:pre-wrap">[SHOW RELAYLOG ['connection_name'] EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-relaylog-events/)</code>
- <code class="highlight fixed" style="white-space:pre-wrap">[SHOW SLAVE ['connection_name'] STATUS](/kb/en/show-slave-status/)</code>
- <code class="highlight fixed" style="white-space:pre-wrap">[SHOW ALL SLAVES STATUS](/kb/en/show-slave-status/)</code>
- <code class="highlight fixed" style="white-space:pre-wrap">[START SLAVE ['connection_name'](/kb/en/start-slave/)...]]</code>
- <code class="highlight fixed" style="white-space:pre-wrap">[START ALL SLAVES ...](/kb/en/start-slave/)</code>
- <code class="highlight fixed" style="white-space:pre-wrap">[STOP SLAVE ['connection_name'] ...](/kb/en/stop-slave/)</code>
- <code class="highlight fixed" style="white-space:pre-wrap">[STOP ALL SLAVES ...](/kb/en/stop-slave/)</code>

The original old-style connection is an empty string <code class="highlight fixed" style="white-space:pre-wrap">''</code>.
You don't have to use this connection if you don't want to.

You create new master connections with [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/).<br>
You delete the connection permanently with [RESET SLAVE 'connection_name' ALL](/kb/en/reset-slave/).

## Replication Variables for Multi-Source

The new replication variable [default_master_connection](/kb/en/replication-and-binary-log-server-system-variables/#default_master_connection)
specifies which connection will be used for commands and variables if you don't
specify a connection. By default this is <code class="highlight fixed" style="white-space:pre-wrap">''</code> (the default
connection name).

The following replication variables are local for the connection.  (In other
words, they show the value for the
<code class="highlight fixed" style="white-space:pre-wrap">@@default_master_connection</code> connection). We are working on
making all the important ones local for the connection.

<table><tbody><tr><th>Type</th><th>Name</th><th>Description</th></tr>
<tr><td>Variable</td><td>&nbsp;<a href="/kb/en/replication-and-binary-log-server-system-variables/#max_relay_log_size">max_relay_log_size</a></td><td>Max size of relay log. Is set at startup to <code>max_binlog_size</code> if 0</td></tr>
<tr><td>Variable</td><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_do_db">replicate_do_db</a></td><td>Tell the slave to restrict replication to updates of tables whose names appear in the comma-separated list. For statement-based replication, only the default database (that is, the one selected by USE) is considered, not any explicitly mentioned tables in the query. For row-based replication, the actual names of table(s) being updated are checked.</td></tr>
<tr><td>Variable</td><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_do_table">replicate_do_table</a></td><td>Tells the slave to restrict replication to tables in the comma-separated list</td></tr>
<tr><td>Variable</td><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_ignore_db">replicate_ignore_db</a></td><td>Tell the slave to restrict replication to updates of tables whose names do not appear in the comma-separated list. For statement-based replication, only the default database (that is, the one selected by USE) is considered, not any explicitly mentioned tables in the query. For row-based replication, the actual names of table(s) being updated are checked.</td></tr>
<tr><td>Variable</td><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_ignore_table">replicate_ignore_table</a></td><td>Tells the slave thread to not replicate any statement that updates the specified table, even if any other tables might be updated by the same statement.</td></tr>
<tr><td>Variable</td><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_wild_do_table">replicate_wild_do_table</a></td><td>Tells the slave thread to restrict replication to statements where any of the updated tables match the specified database and table name patterns.</td></tr>
<tr><td>Variable</td><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_wild_ignore_table">replicate_wild_ignore_table</a></td><td>Tells the slave thread to not replicate to the tables that match the given wildcard pattern.</td></tr>
<tr><td>Status</td><td><a href="/kb/en/replication-and-binary-log-status-variables/#slave_heartbeat_period">Slave_heartbeat_period</a></td><td>How often to request a heartbeat packet from the master (in seconds).</td></tr>
<tr><td>Status</td><td><a href="/kb/en/replication-and-binary-log-status-variables/#slave_received_heartbeats">Slave_received_heartbeats</a></td><td>How many heartbeats we have got from the master.</td></tr>
<tr><td>Status</td><td><a href="/kb/en/replication-and-binary-log-status-variables/#slave_running">Slave_running</a></td><td>Shows if the slave is running. YES means that the sql thread and the IO thread are active. No means either one is not running. '' means that <code>@@default_master_connection</code> doesn't exist.</td></tr>
<tr><td>Variable</td><td>&nbsp;<a href="/kb/en/replication-and-binary-log-server-system-variables/#sql_slave_skip_counter">Sql_slave_skip_counter</a></td><td>How many entries in the replication log that should be skipped (mainly used in case of errors in the log).</td></tr>
</tbody></table>

You can access all of the above variables with either
<code class="highlight fixed" style="white-space:pre-wrap">SESSION</code> or <code class="highlight fixed" style="white-space:pre-wrap">GLOBAL</code>.

Note that the <code class="highlight fixed" style="white-space:pre-wrap">replicate_...</code> variables were added in [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/)

Note that in contrast to MySQL, all variables always show the correct active
value!

Example:

```sql
set @@default_master_connection='';
show status like 'Slave_running';
set @@default_master_connection='other_connection';
show status like 'Slave_running';
```

If <code class="highlight fixed" style="white-space:pre-wrap">@@default_master_connection</code> contains a non existing name,
you will get a warning.

All other master-related variables are global and affect either only the ''
connections or all connections.  For example,
[Slave_retried_transactions](/kb/en/replication-and-binary-log-status-variables/#slave_retried_transactions) now shows the total number of retried transactions over all slaves.

If you need to set [gtid_slave_pos](https://mariadb.com/kb/en/global-transaction-id/#gtid_slave_pos) you need to set this for all masters at the same time.

New status variables:

<table><tbody><tr><th>Name</th><th>Description</th></tr>
<tr><td><code><a href="/kb/en/replication-and-binary-log-server-status-variables/#com_start_all_slaves">Com_start_all_slaves</a></code></td><td>Number of executed <code>START ALL SLAVES</code> commands.</td></tr>
<tr><td><code><a href="/kb/en/replication-and-binary-log-server-status-variables/#com_start_slave">Com_start_slave</a></code></td><td>Number of executed <code>START SLAVE</code> commands. This replaces <code>Com_slave_start</code>.</td></tr>
<tr><td><code><a href="/kb/en/replication-and-binary-log-server-status-variables/#com_stop_slave">Com_stop_slave</a></code></td><td>Number of executed <code>STOP SLAVE</code> commands. This replaces <code>Com_slave_stop</code>.</td></tr>
<tr><td><code><a href="/kb/en/replication-and-binary-log-server-status-variables/#com_stop_all_slaves">Com_stop_all_slaves</a></code></td><td>Number of executed <code>STOP ALL SLAVES</code> commands.</td></tr>
</tbody></table>

<code class="highlight fixed" style="white-space:pre-wrap">[SHOW ALL SLAVES STATUS](/kb/en/show-slave-status/)</code> has the following new columns:

<table><tbody><tr><th>Name</th><th>Description</th></tr>
<tr><td><code>Connection_name</code></td><td>Name of the master connection. This is the first variable.</td></tr>
<tr><td><code>Slave_SQL_State</code></td><td>State of SQL thread.</td></tr>
<tr><td><code>Retried_transactions</code></td><td>Number of retried transactions for this connection.</td></tr>
<tr><td><code>Max_relay_log_size</code></td><td>Max relay log size for this connection.</td></tr>
<tr><td><code>Executed_log_entries</code></td><td>How many log entries the slave has executed.</td></tr>
<tr><td><code>Slave_received_heartbeats</code></td><td>How many heartbeats we have got from the master.</td></tr>
<tr><td><code>Slave_heartbeat_period</code></td><td>How often to request a heartbeat packet from the master (in seconds).</td></tr>
</tbody></table>

## New Files

The basic principle of the new files used by multi source replication is that
they have the same name as the original relay log files suffixed with
<code class="highlight fixed" style="white-space:pre-wrap">connection_name</code> before the extension. The main exception is
the file that holds all connection is named as the normal
<code class="highlight fixed" style="white-space:pre-wrap">master-info-file</code> with a <code class="highlight fixed" style="white-space:pre-wrap">multi-</code> prefix.

When you are using multi source, the following new files are created:

<table><tbody><tr><th>Name</th><th>Description</th></tr>
<tr><td><code>multi-master-info-file</code></td><td>The <code>master-info-file</code> (normally <code>master.info</code>) with a <code>multi-</code> prefix. This contains all master connections in use.</td></tr>
<tr><td><code>master-info-file</code>-connection_name<code>.extension</code></td><td>Contains the current master position for what's applied to in the slave. Extension is normally <code>.info</code></td></tr>
<tr><td><code>relay-log</code>-connection_name<code>.xxxxx</code></td><td>The relay-log name with a connection_name suffix. The xxxxx is the relay log number. This contains the replication data read from the master.</td></tr>
<tr><td><code>relay-log-index</code>-connection_name<code>.extension</code>&nbsp;</td><td>&nbsp;Contains the name of the active <code>relay-log</code>-connection_name<code>.xxxxx</code> files. Extension is normally <code>.index</code></td></tr>
<tr><td><code>relay-log-info-file</code>-connection_name<code>.extension</code></td><td>Contains the current master position for the relay log. Extension is normally <code>.info</code></td></tr>
</tbody></table>

When creating the file, the connection name is converted to lower case and all
special characters in the connection name are converted, the same way as MySQL
table names are converted. This is done to make the file name portable across
different systems.

Hint:

Instead of specifying names for <code class="highlight fixed" style="white-space:pre-wrap">mysqld</code> with [--relay-log](/kb/en/replication-and-binary-log-system-variables/#relay_log), [--relay-log-index](/kb/en/replication-and-binary-log-system-variables/#relay_log_index), [--general-log-file](/kb/en/server-system-variables/#general_log_file), [--slow-query-log-file](/kb/en/server-system-variables/#slow_query_log_file),
[--log-bin](/kb/en/replication-and-binary-log-system-variables/#log_bin) and [--log-bin-index](/kb/en/replication-and-binary-log-system-variables/#log_bin_index), you can just
specify [--log-basename](/kb/en/mysqld-options/#-log-basename) and all the other variables are set
with this as a prefix.

## Other Things

- All error messages from a slave with a connection name, that are written to the error log, are prefixed with <code class="highlight fixed" style="white-space:pre-wrap">Master 'connection_name':</code>. This makes it easy to see from where an error originated.
- Errors <code class="highlight fixed" style="white-space:pre-wrap">ER_MASTER_INFO</code> and <code class="highlight fixed" style="white-space:pre-wrap">WARN_NO_MASTER_INFO</code> now includes connection_name.
- There is no conflict resolution. The assumption is that there are no conflicts in data between the different masters.
- All executed commands are stored in the normal binary log (nothing new here).
- If the server variable <code class="highlight fixed" style="white-space:pre-wrap">log_warnings</code> &gt; 1 then you will get some information in the log about how the multi-master-info file is updated (mainly for debugging).
- [SHOW [FULL] SLAVE STATUS](/kb/en/show-slave-status/) has one line per connection and more columns than before. <strong>Note that the first column is the <code class="highlight fixed" style="white-space:pre-wrap">connection_name</code>!</strong>
- <code class="highlight fixed" style="white-space:pre-wrap">[RESET SLAVE](/kb/en/reset-slave/)</code> now deletes all relay-log files.

## replicate-... Variables

- The support for <code class="highlight fixed" style="white-space:pre-wrap">replicate-...</code> variables was added in [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/)
- One can set the values for the <code class="highlight fixed" style="white-space:pre-wrap">replicate-...</code> variables from the command line or in <code class="highlight fixed" style="white-space:pre-wrap">my.cnf</code> for a given connection by prefixing the variable with the connection name.
- If one doesn't use any connection name prefix for a <code class="highlight fixed" style="white-space:pre-wrap">replicate..</code> variable, then the value will be used as the default value for all connections that don't have a value set for this variable.

Example:

```sql
mysqld --main_connection.replicate_do_db=main_database --replicate_do_db=other_database
```

The have sets the <code class="highlight fixed" style="white-space:pre-wrap">replicate_do_db</code> variable to <code class="highlight fixed" style="white-space:pre-wrap">main_database</code> for the connection named <code class="highlight fixed" style="white-space:pre-wrap">main_connection</code>. All other connections will use the value <code class="highlight fixed" style="white-space:pre-wrap">other_database</code>.

One can also use this syntax to set <code class="highlight fixed" style="white-space:pre-wrap">replicate-rewrite-db</code> for a given connection.

## Typical Use Cases

- You are partitioning your data over many masters and would like to get it all together on one machine to do analytical queries on all data.
- You have many databases spread over many MariaDB/MySQL servers and would like to have all of them on one machine as an extra backup.
- In a Galera cluster the default replication filter rules like <code class="highlight fixed" style="white-space:pre-wrap">replicate-do-db</code> do not apply to replication connections, but also to Galera write set applier threads. By using a named multi-master replication connection instead, even when replicating from just one master into the cluster, the master-slave replication rules can be kept separate from the Galera intra-node replication traffic.

## Limitations

- Each active connection will create 2 threads (as is normal for MariaDB replication).
- You should ensure that all master have different <code class="highlight fixed" style="white-space:pre-wrap">server-id</code>'s. If you don't do this, you will get into trouble if you try to replicate from the multi-source slave back to your masters.
- One can change [max_relay_log_size](/kb/en/replication-and-binary-log-server-system-variables/#max_relay_log_size) for any active connection, but new connections will always use the server startup value for <code class="highlight fixed" style="white-space:pre-wrap">max_relay_log_size</code>, which can't be changed at runtime.
- Option [innodb-recovery-update-relay-log](/kb/en/xtradbinnodb-server-system-variables/#innodb_recovery_update_relay_log) (xtradb feature to store and restore relay log position for slaves) only works for the default connection ''.  As this option is not really safe and can easily cause loss of data if you use storage engines other than InnoDB, we don't recommend this option to be used.
- [slave_net_timeout](/kb/en/replication-and-binary-log-server-system-variables/#slave_net_timeout) affects all connections. We don't check anymore if it's less than [Slave_heartbeat_period](/kb/en/replication-and-binary-log-server-status-variables/#slave_heartbeat_period), as this doesn't make sense in a multi-source setup.

## Incompatibilities with MariaDB/MySQL 5.5

- [max_relay_log_size](/kb/en/replication-and-binary-log-server-system-variables/#max_relay_log_size) is now (almost) a normal variable and not automatically changed if [max_binlog_size](/kb/en/replication-and-binary-log-server-system-variables/#max_binlog_size) is changed. To keep things compatible with old config files, we set it to <code class="highlight fixed" style="white-space:pre-wrap">max_binlog_size</code> at startup if its value is 0.
- You can now access replication variables that depend on the active connection with either <code class="highlight fixed" style="white-space:pre-wrap">GLOBAL</code> or <code class="highlight fixed" style="white-space:pre-wrap">SESSION</code>.
- We only write information about relay log positions for recovery if [innodb-recovery-update-relay-log](/kb/en/xtradbinnodb-server-system-variables/#innodb_recovery_update_relay_log) is set.
- [Slave_retried_transactions](/kb/en/replication-and-binary-log-status-variables/#slave_retried_transactions) now shows the total count of retried transactions over all slaves.
- The status variable <code class="highlight fixed" style="white-space:pre-wrap">Com_slave_start</code> is replaced with [Com_start_slave](/kb/en/replication-and-binary-log-status-variables/#com_start_slave).
- The status variable <code class="highlight fixed" style="white-space:pre-wrap">Com_slave_stop</code> is replaced with [Com_stop_slave](/kb/en/replication-and-binary-log-status-variables/#com_stop_slave).
- <code class="highlight fixed" style="white-space:pre-wrap">FLUSH RELAY LOGS</code> are not replicated anymore. This is not safe as connection names may be different on the slave.

## See Also

- Using multi-source with [global transaction id](https://mariadb.com/kb/en/global-transaction-id/#use-with-multi-source-replication-and-other-multi-master-setups)
- The work in MariaDB is based on the project description at [MDEV-253](https://jira.mariadb.org/browse/MDEV-253).
- The original code base comes from [Taobao, developed by Peng Lixun](http://mysql.taobao.org/index.php/Patch_source_code#Multi-master_replication). A big thanks to them for this important feature!