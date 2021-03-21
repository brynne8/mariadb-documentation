# Semisynchronous Replication

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

Semisynchronous replication is no longer a plugin in [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/) and later. This removes some overhead and improves performance. See [MDEV-13073](https://jira.mariadb.org/browse/MDEV-13073) for more information.

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

Semisynchronous replication was significantly enhanced in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/). See [MDEV-162](https://jira.mariadb.org/browse/MDEV-162) for more information.

## Description

[Standard MariaDB replication](/replication/standard-replication) is asynchronous, but MariaDB also provides a semisynchronous replication option.

With regular asynchronous replication, slaves request events from the master's binary log whenever the slaves are ready. The master does not wait for a slave to confirm that an event has been received.

With fully synchronous replication, all slaves are required to respond that they have received the events. See [Galera Cluster](/replication/galera-cluster).

Semisynchronous replication waits for just one slave to acknowledge that it has received and logged the events.

Semisynchronous replication therefore comes with some negative performance impact, but increased data integrity. Since the delay is based on the roundtrip time to the slave and back, this delay is minimized for servers in close proximity over fast networks.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and later, semisynchronous replication is built into the server, so it can be enabled immediately in those versions.

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and before, semisynchronous replication requires the user to install a plugin on both the master and the slave before it can be enabled.

## Installing the Plugin

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

In [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/) and later, the Semisynchronous Replication feature is built into MariaDB server and is no longer provided by a plugin. <strong>This means that installing the plugin is not supported on those versions.</strong> In [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/) and later, you can skip right to [Enabling Semisynchronous Replication](#enabling-semisynchronous-replication).

The semisynchronous replication plugin is actually two different plugins--one for the master, and one for the slave. Shared libraries for both plugins are included with MariaDB. Although the plugins' shared libraries distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default prior to [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/). There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin).

For example, if it's a master:

```sql
INSTALL SONAME 'semisync_master';
```

Or if it's a slave:

```sql
INSTALL SONAME 'semisync_slave';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

For example, if it's a master:

```sql
[mariadb]
...
plugin_load_add = semisync_master
```

Or if it's a slave:

```sql
[mariadb]
...
plugin_load_add = semisync_slave
```

## Uninstalling the Plugin

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

In [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/) and later, the Semisynchronous Replication feature is built into MariaDB server and is no longer provided by a plugin. <strong>This means that uninstalling the plugin is not supported on those versions.</strong>

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin).

For example, if it's a master:

```sql
UNINSTALL SONAME 'semisync_master';
```

Or if it's a slave:

```sql
UNINSTALL SONAME 'semisync_slave';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Enabling Semisynchronous Replication

Semisynchronous replication can be enabled by setting the relevant system variables on the master and the slave.

If a server needs to be able to switch between acting as a master and a slave, then you can enable both the master and slave system variables on the server. For example, you might need to do this if [MariaDB MaxScale](/mariadb-platform-x3/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/mariadb-enterprise/maxscale) is being used to enable [auto-failover or switchover](/kb/en/mariadb-maxscale-23-mariadb-monitor/#cluster-manipulation-operations) with [MariaDB Monitor](/kb/en/mariadb-maxscale-23-mariadb-monitor/).

### Enabling Semisynchronous Replication on the Master

Semisynchronous replication can be enabled on the master by setting the <a undefined>rpl_semi_sync_master_enabled</a> system variable to `ON`. It can be set dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL rpl_semi_sync_master_enabled=ON;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) prior to starting up the server. For example:

```sql
[mariadb]
...
rpl_semi_sync_master_enabled=ON
```

### Enabling Semisynchronous Replication on the Slave

Semisynchronous replication can be enabled on the slave by setting the <a undefined>rpl_semi_sync_slave_enabled</a> system variable to `ON`. It can be set dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL rpl_semi_sync_slave_enabled=ON;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) prior to starting up the server. For example:

```sql
[mariadb]
...
rpl_semi_sync_slave_enabled=ON
```

If semisynchronous replication is enabled on a server when [slave threads](/kb/en/replication-threads/#threads-on-the-slave) were already running, the slave I/O thread will need to be restarted to enable the slave to register as a semisynchronous slave when it connects to the master. For example:

```sql
STOP SLAVE IO_THREAD;
START SLAVE IO_THREAD;
```

If this is not done, and the slave thread is already running, then it will continue to use asynchronous replication.

## Configuring the Master Timeout

In semisynchronous replication, only after the events have been written to the relay log and flushed does the slave acknowledge receipt of a transaction's events. If the slave does not acknowledge the transaction before a certain amount of time has passed, then a timeout occurs and the master switches to asynchronous replication. This will be reflected in the master's [error log](/mariadb-administration/server-monitoring-logs/error-log) with messages like the following:

```sql
[Warning] Timeout waiting for reply of binlog (file: mariadb-1-bin.000002, pos: 538), semi-sync up to file , position 0.
[Note] Semi-sync replication switched OFF.
```

When this occurs, the <a undefined>Rpl_semi_sync_master_status</a> status variable will be switched to `OFF`.

When at least one semisynchronous slave catches up, semisynchronous replication is resumed. This will be reflected in the master's [error log](/mariadb-administration/server-monitoring-logs/error-log) with messages like the following:

```sql
[Note] Semi-sync replication switched ON with slave (server_id: 184137206) at (mariadb-1-bin.000002, 215076)
```

When this occurs, the <a undefined>Rpl_semi_sync_master_status</a> status variable will be switched to `ON`.

The number of times that semisynchronous replication has been switched off can be checked by looking at the value of the <a undefined>Rpl_semi_sync_master_no_times</a> status variable.

If you see a lot of timeouts like this in your environment, then you may want to change the timeout period. The timeout period can be changed by setting the <a undefined>rpl_semi_sync_master_timeout</a> system variable. It can be set dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL rpl_semi_sync_master_timeout=20000;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) prior to starting up the server. For example:

```sql
[mariadb]
...
rpl_semi_sync_master_timeout=20000
```

To determine a good value for the <a undefined>rpl_semi_sync_master_timeout</a> system variable, you may want to look at the values of the <a undefined>Rpl_semi_sync_master_net_avg_wait_time</a> and <a undefined>Rpl_semi_sync_master_tx_avg_wait_time</a> status variables.

## Configuring the Master Wait Point

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

The <a undefined>rpl_semi_sync_master_wait_point</a> system variable was added in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/).

##### MariaDB until [10.1.2](/kb/en/mariadb-1012-release-notes/)

In [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/) and before, the <a undefined>rpl_semi_sync_master_wait_point</a> system variable does not exist. However, in those versions, the master waits for the acknowledgement from slaves at a point that is equivalent to `AFTER_COMMIT`.

In semisynchronous replication, there are two potential points at which the master can wait for the slave acknowledge the receipt of a transaction's events. These two wait points have different advantages and disadvantages.

The wait point is configured by the <a undefined>rpl_semi_sync_master_wait_point</a> system variable. The supported values are:

- `AFTER_SYNC`
- `AFTER_COMMIT`

It can be set dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL rpl_semi_sync_master_wait_point='AFTER_SYNC';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) prior to starting up the server. For example:

```sql
[mariadb]
...
rpl_semi_sync_master_wait_point=AFTER_SYNC
```

When this variable is set to `AFTER_SYNC`, the master performs the following steps:

1 Prepares the transaction in the storage engine.
2 Syncs the transaction to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).
3 Waits for acknowledgement from the slave.
4 Commits the transaction to the storage engine.
5 Returns an acknowledgement to the client.

The effects of the `AFTER_SYNC` wait point are:

- All clients see the same data on the master at the same time; after acknowledgement by the slave and after being committed to the storage engine on the master.

- If the master crashes, then failover should be lossless, because all transactions committed on the master would have been replicated to the slave.

- However, if the master crashes, then its [binary log](/mariadb-administration/server-monitoring-logs/binary-log) may also contain events for transactions that were prepared by the storage engine and written to the binary log, but that were never actually committed by the storage engine. As part of the server's [automatic crash recovery](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log/heuristic-recovery-with-the-transaction-coordinator-log) process, the server may recover these prepared transactions when the server is restarted. This could cause the crashed master to become inconsistent with the slaves. Therefore, the crashed master may need to be rebuilt. See [MDEV-19733](https://jira.mariadb.org/browse/MDEV-19733) for more information.

When this variable is set to `AFTER_COMMIT`, the master performs the following steps:

1 Prepares the transaction in the storage engine.
2 Syncs the transaction to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).
3 Commits the transaction to the storage engine.
4 Waits for acknowledgement from the slave.
5 Returns an acknowledgement to the client.

The effects of the `AFTER_COMMIT` wait point are:

- Other clients may see the committed transaction before the committing client.

- If the master crashes, then failover may involve some data loss, because the master may have committed transactions that had not yet been acknowledged by the slaves.

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>N/A</td><td>N/A</td><td><a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a></td></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a></td></tr>
<tr><td>1.0</td><td>Gamma</td><td><a href="/kb/en/mariadb-10013-release-notes/">MariaDB 10.0.13</a></td></tr>
<tr><td>1.0</td><td>Unknown</td><td><a href="/kb/en/mariadb-10011-release-notes/">MariaDB 10.0.11</a></td></tr>
<tr><td>1.0</td><td>N/A</td><td><a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a></td></tr>
</tbody></table>

## System Variables

#### `rpl_semi_sync_master_enabled`

- <strong>Description:</strong> Set to `ON` to enable semi-synchronous replication master. Disabled by default.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rpl-semi-sync-master-enabled[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rpl_semi_sync_master_timeout`

- <strong>Description:</strong> The timeout value, in milliseconds, for semi-synchronous replication in the master. If this timeout is exceeded in waiting on a commit for acknowledgement from a slave, the master will revert to asynchronous replication.
<ul start="1"><li>When a timeout occurs, the <a undefined>Rpl_semi_sync_master_status</a> status variable will also be switched to `OFF`.
</li><li>See [Configuring the Master Timeout](#configuring-the-master-timeout) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rpl-semi-sync-master-timeout[=#]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10000` (10 seconds)
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rpl_semi_sync_master_trace_level`

- <strong>Description:</strong> The tracing level for semi-sync replication. Four levels are defined:
<ul start="1"><li>`1`: General level, including for example time function failures.
</li><li>`16`: More detailed level, with more verbose information.
</li><li>`32`: Net wait level, including more information about network waits.
</li><li>`64`: Function level, including information about function entries and exits. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rpl-semi-sync-master-trace-level[=#]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `32`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rpl_semi_sync_master_wait_no_slave`

- <strong>Description:</strong> If set to `ON`, the default, the slave count (recorded by [Rpl_semi_sync_master_clients](/kb/en/semisynchronous-replication-plugin-status-variables/#rpl_semi_sync_master_clients)) may drop to zero, and the master will still wait for the timeout period. If set to `OFF`, the master will revert to asynchronous replication as soon as the slave count drops to zero.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rpl-semi-sync-master-wait-no-slave[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rpl_semi_sync_master_wait_point`

- <strong>Description:</strong> Whether the transaction should wait for semi-sync acknowledgement after having synced the binlog (`AFTER_SYNC`), or after having committed in storage engine (`AFTER_COMMIT`, the default).
<ul start="1"><li>When this variable is set to `AFTER_SYNC`, the master performs the following steps:
<ol start="1"><li>Prepares the transaction in the storage engine.
</li><li>Syncs the transaction to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).
</li><li>Waits for acknowledgement from the slave.
</li><li>Commits the transaction to the storage engine.
</li><li>Returns an acknowledgement to the client.
</li></ol>
</li><li>When this variable is set to `AFTER_COMMIT`, the master performs the following steps:
<ol start="1"><li>Prepares the transaction in the storage engine.
</li><li>Syncs the transaction to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).
</li><li>Commits the transaction to the storage engine.
</li><li>Waits for acknowledgement from the slave.
</li><li>Returns an acknowledgement to the client.
</li></ol>
</li><li>In [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/) and before, this system variable does not exist. However, in those versions, the master waits for the acknowledgement from slaves at a point that is equivalent to `AFTER_COMMIT`. 
</li><li>See [Configuring the Master Wait Point](#configuring-the-master-wait-point) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rpl-semi-sync-master-wait-point=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `AFTER_COMMIT`
- <strong>Valid Values:</strong> `AFTER_SYNC`, `AFTER_COMMIT`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `rpl_semi_sync_slave_delay_master`

- <strong>Description:</strong> Only write master info file when ack is needed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rpl-semi-sync-slave-delay-master[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `rpl_semi_sync_slave_enabled`

- <strong>Description:</strong> Set to `ON` to enable semi-synchronous replication slave. Disabled by default.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rpl-semi-sync-slave-enabled[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rpl_semi_sync_slave_kill_conn_timeout`

- <strong>Description:</strong> Timeout for the mysql connection used to kill the slave io_thread's connection on master. This timeout comes into play when stop slave is executed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rpl-semi-sync-slave-kill-conn-timeout[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `5`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `rpl_semi_sync_slave_trace_level`

- <strong>Description:</strong> The tracing level for semi-sync replication. The levels are the same as for [rpl_semi_sync_master_trace_level](#rpl_semi_sync_master_trace_level).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rpl-semi-sync-slave-trace_level[=#]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `32`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

## Options

### `rpl_semi_sync_master`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the <a undefined>mysql.plugins</a> table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rpl-semi-sync-master=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`
- <strong>Removed:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

### `rpl_semi_sync_slave`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the <a undefined>mysql.plugins</a> table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rpl-semi-sync-slave=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`
- <strong>Removed:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

## Status Variables

For a list of status variables added when the plugin is installed, see [Semisynchronous Replication Plugin Status Variables](/replication/optimization-and-tuning/system-variables/semisynchronous-replication-plugin-status-variables).