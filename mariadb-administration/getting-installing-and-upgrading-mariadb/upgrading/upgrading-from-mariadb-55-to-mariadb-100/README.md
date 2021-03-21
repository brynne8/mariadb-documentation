# Upgrading from MariaDB 5.5 to MariaDB 10.0

## What You Need to Know

There are no changes in table or index formats between [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB
10.0](/kb/en/what-is-mariadb-100/), so on most servers the upgrade should be painless.

### How to Upgrade

For Windows, see [Upgrading MariaDB on Windows](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-on-windows) instead.

For MariaDB Galera Cluster, see [Upgrading from MariaDB Galera Cluster 5.5 to MariaDB Galera Cluster 10.0](/replication/galera-cluster/upgrading-galera-cluster/upgrading-from-mariadb-galera-cluster-55-to-mariadb-galera-cluster-100) instead.

Before you upgrade, it would be best to take a backup of your database. This is always a good idea to do before an upgrade. We would recommend [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/).

The suggested upgrade procedure is:

1 Modify the repository configuration, so the system's package manager installs [MariaDB 10.0](/kb/en/what-is-mariadb-100/). For example,
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, see [Updating the MariaDB APT repository to a New Major Release](/kb/en/installing-mariadb-deb-files/#updating-the-mariadb-apt-repository-to-a-new-major-release) for more information.
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, see [Updating the MariaDB YUM repository to a New Major Release](/kb/en/yum/#updating-the-mariadb-yum-repository-to-a-new-major-release) for more information.
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, see [Updating the MariaDB ZYpp repository to a New Major Release](/kb/en/installing-mariadb-with-zypper/#updating-the-mariadb-zypp-repository-to-a-new-major-release) for more information.
</li></ul>
2 Set <a undefined>innodb_fast_shutdown</a> to `0`. It can be changed dynamically with <a undefined>SET GLOBAL</a>. For example: <br>
<code class="fixed" style="white-space:pre-wrap">SET GLOBAL innodb_fast_shutdown=0;</code>
3 [Stop MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/starting-and-stopping-mariadb-automatically).
4 Uninstall the old version of MariaDB.
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, execute the following: <br>
<code class="fixed" style="white-space:pre-wrap">sudo apt-get remove mariadb-server</code>
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, execute the following: <br>
<code class="fixed" style="white-space:pre-wrap">sudo yum remove MariaDB-server</code>
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, execute the following: <br>
<code class="fixed" style="white-space:pre-wrap">sudo zypper remove MariaDB-server</code>
</li></ul>
5 Install the new version of MariaDB.
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, see [Installing MariaDB Packages with APT](/kb/en/installing-mariadb-deb-files/#installing-mariadb-packages-with-apt) for more information.
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, see [Installing MariaDB Packages with YUM](/kb/en/yum/#installing-mariadb-packages-with-yum) for more information.
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, see [Installing MariaDB Packages with ZYpp](/kb/en/installing-mariadb-with-zypper/#installing-mariadb-packages-with-zypp) for more information.
</li></ul>
6 Make any desired changes to configuration options in [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), such as `my.cnf`. This includes removing any options that are no longer supported.
7 [Start MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/starting-and-stopping-mariadb-automatically).
8 Run [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade).
<ul start="1"><li>`mysql_upgrade` does two things:
<ol start="1"><li>Ensures that the system tables in the [mysq](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables)l database are fully compatible with the new version.
</li><li>Does a very quick check of all tables and marks them as compatible with the new version of MariaDB .
</li></ol>
</li></ul>

### Incompatible Changes Between 5.5 and 10.0

As mentioned previously, on most servers upgrading from 5.5 should be painless.
However, there are some things that have changed which could affect an upgrade:

#### Options That Have Changed Default Values

Most of the following options have increased a bit in value to give better
performance. They should not use much additional memory, but some of them a do
use a bit more disk space.<span class="cstm-style" style="display:none;"> <sup class="reference" id="_ref-0">[[1](#_note-0)]</sup></span>

<table><tbody><tr><th>Option</th><th>Old default value</th><th>New default value</th></tr>
<tr><td><code><a href="/kb/en/aria-system-variables/#aria_sort_buffer_size">aria-sort-buffer-size</a></code></td><td><code>128M</code></td><td><code>256M</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#back_log">back_log</a></code></td><td><code>50</code></td><td><code>150</code></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_instances">innodb-buffer-pool-instances</a></code></td><td><code>1</code></td><td><code>8</code> (except on 32-bit Windows)</td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_concurrency_tickets">innodb-concurrency-tickets</a></code></td><td><code>500</code></td><td><code>5000</code></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_log_file_size">innodb-log-file-size</a></code></td><td><code>5M</code></td><td><code>48M</code></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_old_blocks_time">innodb-old-blocks-time</a></code></td><td><code>0</code></td><td><code>1000</code></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_open_files">innodb-open-files</a></code></td><td><code>300</code></td><td><code>400</code> <sup><a href="#_note-1">[2]</a></sup></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_purge_batch_size">innodb-purge-batch-size</a></code></td><td><code>20</code></td><td><code>300</code></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_undo_logs">innodb-undo-logs</a></code></td><td><code>ON</code></td><td><code>20</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#max_connect_errors">max-connect-errors</a></code></td><td><code>10</code></td><td><code>100</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#max_relay_log_size">max-relay-log-size</a></code></td><td><code>0</code></td><td><code>1024M</code></td></tr>
<tr><td><code><a href="/kb/en/myisam-server-system-variables/#myisam_sort_buffer_size">myisam-sort-buffer-size</a></code></td><td><code>8M</code></td><td><code>128M</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#optimizer_switch">optimizer-switch</a></code></td><td>...</td><td>Added <code>extended_keys=on, exists_to_in=on</code></td></tr>
</tbody></table>

#### Options That Have Been Removed or Renamed

The following options should be removed or renamed if you use them in your
config files:

<table><tbody><tr><th>Option</th><th>Reason</th></tr>
<tr><td><code>engine-condition-pushdown</code></td><td>Replaced with <code><a href="/kb/en/index-condition-pushdown/">set optimizer_switch='engine_condition_pushdown=on'</a></code></td></tr>
<tr><td><code>innodb-adaptive-flushing-method</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-autoextend-increment</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-blocking-buffer-pool-restore</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-buffer-pool-pages</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-buffer-pool-pages-blob</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-buffer-pool-pages-index</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-buffer-pool-restore-at-startup</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-buffer-pool-shm-checksum</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-buffer-pool-shm-key</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-checkpoint-age-target</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-dict-size-limit</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-doublewrite-file</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-fast-checksum</code></td><td>Renamed to <a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_checksum_algorithm">innodb-checksum-algorithm</a></td></tr>
<tr><td><code>innodb-flush-neighbor-pages</code></td><td>Renamed to <a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_flush_neighbors">innodb-flush-neighbors</a></td></tr>
<tr><td><code>innodb-ibuf-accel-rate</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-ibuf-active-contract</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-ibuf-max-size</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-import-table-from-xtrabackup</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-index-stats</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-lazy-drop-table</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-merge-sort-block-size</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-persistent-stats-root-page</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-read-ahead</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-recovery-stats</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-recovery-update-relay-log</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-stats-auto-update</code></td><td>Renamed to <code>innodb-stats-auto-recalc</code></td></tr>
<tr><td><code>innodb-stats-update-need-lock</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-sys-stats</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-table-stats</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-thread-concurrency-timer-based</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>innodb-use-sys-stats-table</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
<tr><td><code>xtradb-admin-command</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
</tbody></table>

#### Reserved Words

New reserved word: RETURNING. This can no longer be used as an identifier without being quoted.

#### Other

The `SET OPTION` syntax is deprecated in [MariaDB 10.0](/kb/en/what-is-mariadb-100/). Use [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set) instead.

### New Major Features To Consider

You should consider using the following new major features in [MariaDB 10.0](/kb/en/what-is-mariadb-100/):

#### For Master / Slave Setups

- [Global transaction id](/kb/en/global-transaction-id/) is enabled by default. This makes it easier to change a slave to a master.
- [MariaDB 10.0](/kb/en/what-is-mariadb-100/) supports [parallel applying of queries on slaves](/replication/standard-replication/parallel-replication). You can enable this with [slave-parallel-threads=#](/kb/en/replication-and-binary-log-server-system-variables/#slave_parallel_threads).  Note that this works only if your master is [MariaDB 10.0](/kb/en/what-is-mariadb-100/) or later.
- [Multi source replication](/replication/standard-replication/multi-source-replication)

#### Galera

See [Upgrading from MariaDB Galera Cluster 5.5 to MariaDB Galera Cluster 10.0](/replication/galera-cluster/upgrading-galera-cluster/upgrading-from-mariadb-galera-cluster-55-to-mariadb-galera-cluster-100) for more details on [Galera](/replication/galera-cluster/what-is-mariadb-galera-cluster) upgrades.

#### Variables

- [System Variables Added in MariaDB 10.0](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/system-variables-added-in-mariadb-100)
- [Status Variables Added in MariaDB 10.0](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/status-variables-added-in-mariadb-100)

## Notes

1 [↑](#_ref-0) The `innodb-open-files` variable defaults to the value of `table-open-cache` (`400` is the default) if it is set to any value less than `10` so long as `innodb-file-per-table` is set to `1` or `TRUE` (the default). If `innodb_file_per_table` is set to `0` or `FALSE` <strong>and</strong> `innodb-open-files` is set to a value less than `10`, the default is `300`

## See Also

- [The features in MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- [Upgrading from MariaDB Galera Cluster 5.5 to MariaDB Galera Cluster 10.0](/replication/galera-cluster/upgrading-galera-cluster/upgrading-from-mariadb-galera-cluster-55-to-mariadb-galera-cluster-100)