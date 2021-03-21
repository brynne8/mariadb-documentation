# Moving from MySQL to MariaDB in Debian 9

[MariaDB 10.1](/kb/en/what-is-mariadb-101/) is now the default mysql server in Debian 9 "Stretch". This page provides information on this change and instructions to help with upgrading your Debian 8 "Jessie" version of MySQL or MariaDB to [MariaDB 10.1](/kb/en/what-is-mariadb-101/) in Debian 9 "Stretch".

## Background information

The version of MySQL in Debian 8 "Jessie" is 5.5. When installing, most users will install the `mysql-server` package, which depends on the `mysql-server-5.5 package`. In Debian 9 "Stretch" the `mysql-server` package depends on a new package called `default-mysql-server`. This package in turn depends on `mariadb-server-10.1`. There is no `default-mysql-server` package in Jessie.

In both Jessie and Stretch there is also a `mariadb-server` package which is a MariaDB-specific analog to the `mysql-server` package. In Jessie this package depends on `mariadb-server-10.0` and in Stretch this package depends on `mariadb-server-10.1` (the same as the `default-mysql-server` package).

So, the main repository difference in Debian 9 "Stretch" is that when you install the `mysql-server` package on Stretch you will get [MariaDB 10.1](/kb/en/what-is-mariadb-101/) instead of MySQL, like you would with previous versions of Debian. Note that `mysql-server` is just an empty transitional meta-package and users are encouraged to install MariaDB using the actual package `mariadb-server`.

All apps and tools, such as the popular LAMP stack, in the repositories that depend on the `mysql-server` package will continue to work using MariaDB as the database. For new installs there is nothing different that needs to be done when installing the mysql-server or mariadb-server packages.

## Before you upgrade

If you are currently running MySQL 5.5 on Debian 8 "Jessie" and are planning an upgrade to [MariaDB 10.1](/kb/en/what-is-mariadb-101/) on Debian 9 "Stretch", there are some things to keep in mind:

### Backup before you begin

This is a major upgrade, and so complete database backups are strongly suggested before you begin. [MariaDB 10.1](/kb/en/what-is-mariadb-101/) is compatible on disk and wire with MySQL 5.5, and the MariaDB developer team has done extensive development and testing to make upgrades as painless and trouble-free as possible. Even so, it's always a good idea to do regular backups, especially before an upgrade. As the database has to shutdown anyway for the upgrade, this is a good opportunity to do a backup!

### Changed, renamed, and removed options

Some default values have been changed, some have been renamed, and others have been removed between MySQL 5.5 and [MariaDB 10.1](/kb/en/what-is-mariadb-101/). The following sections detail them.

#### Options with changed default values

Most of the following options have increased a bit in value to give better
performance. They should not use much additional memory, but some of them do
use a bit more disk space.<span class="cstm-style" style="display:none;"> <sup class="reference" id="_ref-0">[[1](#_note-0)]</sup></span>

<table><tbody><tr><th>Option</th><th>Old default value</th><th>New default value</th></tr>
<tr><td><code><a href="/kb/en/aria-system-variables/#aria_sort_buffer_size">aria-sort-buffer-size</a></code></td><td><code>128M</code></td><td><code>256M</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#back_log">back_log</a></code></td><td><code>50</code></td><td><code>150</code></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_concurrency_tickets">innodb-concurrency-tickets</a></code></td><td><code>500</code></td><td><code>5000</code></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_log_file_size">innodb-log-file-size</a></code></td><td><code>5M</code></td><td><code>48M</code></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_log_compressed_pages">innodb_log_compressed_pages</a></code></td><td><code>ON</code></td><td><code>OFF</code></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_old_blocks_time">innodb-old-blocks-time</a></code></td><td><code>0</code></td><td><code>1000</code></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_open_files">innodb-open-files</a></code></td><td><code>300</code></td><td><code>400</code> <sup><a href="#_note-1">[2]</a></sup></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_purge_batch_size">innodb-purge-batch-size</a></code></td><td><code>20</code></td><td><code>300</code></td></tr>
<tr><td><code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_undo_logs">innodb-undo-logs</a></code></td><td><code>ON</code></td><td><code>20</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#join_buffer_size">join_buffer_size</a></code></td><td><code>128K</code></td><td><code>256K</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#max_allowed_packet">max_allowed_packet</a></code></td><td><code>1M</code></td><td><code>4M</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#max_connect_errors">max-connect-errors</a></code></td><td><code>10</code></td><td><code>100</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#max_relay_log_size">max-relay-log-size</a></code></td><td><code>0</code></td><td><code>1024M</code></td></tr>
<tr><td><code><a href="/kb/en/myisam-server-system-variables/#myisam_sort_buffer_size">myisam-sort-buffer-size</a></code></td><td><code>8M</code></td><td><code>128M</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#optimizer_switch">optimizer-switch</a></code></td><td>...</td><td>Added <code>extended_keys=on, exists_to_in=on</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#query_alloc_block_size">query_alloc_block_size</a></code></td><td><code>8192</code></td><td><code>16384</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#query_cache_size">query_cache_size</a></code></td><td><code>0</code></td><td><code>1M</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#query_cache_type">query_cache_type</a></code></td><td><code>ON</code></td><td><code>OFF</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#query_prealloc_size">query_prealloc_size</a></code></td><td><code>8192</code></td><td><code>24576</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#secure_auth">secure_auth</a></code></td><td><code>OFF</code></td><td><code>ON</code></td></tr>
<tr><td><code><a href="/kb/en/replication-and-binary-log-server-system-variables/#sql_log_bin">sql_log_bin</a></code></td><td></td><td>No longer affects replication of events in a Galera cluster.</td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#sql_mode">sql_mode</a></code></td><td><code>empty</code></td><td><code>NO_AUTO_CREATE_USER, NO_ENGINE_SUBSTITUTION</code></td></tr>
<tr><td><code><a href="/kb/en/replication-and-binary-log-server-system-variables/#sync_master_info">sync_master_info</a></code></td><td><code>0</code></td><td><code>10000</code></td></tr>
<tr><td><code><a href="/kb/en/replication-and-binary-log-server-system-variables/#sync_relay_log">sync_relay_log</a></code></td><td><code>0</code></td><td><code>10000</code></td></tr>
<tr><td><code><a href="/kb/en/replication-and-binary-log-server-system-variables/#sync_relay_log_info">sync_relay_log_info</a></code></td><td><code>0</code></td><td><code>10000</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#table_open_cache">table_open_cache</a></code></td><td><code>400</code></td><td><code>2000</code></td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#thread_pool_max_threads">thread_pool_max_threads</a></code></td><td><code>500</code></td><td><code>1000</code></td></tr>
</tbody></table>

#### Options that have been removed or renamed

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
<tr><td><code><a href="/kb/en/server-system-variables/#rpl_recovery_rank">rpl_recovery_rank</a></code></td><td>Unused in 10.0+</td></tr>
<tr><td><code>xtradb-admin-command</code></td><td>Removed by <a href="/kb/en/xtradb/">XtraDB</a></td></tr>
</tbody></table>

### Suggested upgrade procedure for replication

If you have a [master-slave setup](/replication/standard-replication/), the normal procedure is to first upgrade your slaves to MariaDB, then move one of your slaves to be the master and then upgrade your original master. In this scenario you can upgrade from MySQL to MariaDB or upgrade later to a new version of MariaDB without any downtime.

### Other resources to consult before beginning your upgrade

It may also be useful to check out the [Upgrading MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/) section. It contains several articles on upgrading from MySQL to MariaDB and from one version of MariaDB to another. For upgrade purposes, MySQL 5.5 and [MariaDB 5.5](/kb/en/what-is-mariadb-55/) are very similar. In particular, see the [Upgrading from MariaDB 5.5 to MariaDB 10.0](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-55-to-mariadb-100/) and [Upgrading from MariaDB 10.0 to MariaDB 10.1](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-100-to-mariadb-101/) articles.

If you need help with upgrading or setting up replication, you can always [contact the MariaDB corporation](https://mariadb.com/contact) to find experts to help you with this.

## Upgrading to [MariaDB 10.1](/kb/en/what-is-mariadb-101/) from MySQL 5.5

The suggested upgrade procedure is:

1 Set [innodb_fast_shutdown](/kb/en/xtradbinnodb-server-system-variables/#innodb_fast_shutdown) to `0`. This is to ensure that if you make a backup as part of the upgrade, all data is written to the InnoDB data files, which simplifies any restore in the future.
2 Shutdown MySQL 5.5
3 Take a [backup](/mariadb-administration/backing-up-and-restoring-databases/backup-and-restore-overview/)
<ul start="1"><li>when the server is shut down is the perfect time to take a backup of your databases
</li><li>store a copy of the backup on external media or a different machine for safety
</li></ul>
4 Perform the upgrade from Debian 8 to Debian 9
5 During the upgrade, the [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade/) script will be run automatically; this script does two things:
<ol start="1"><li>Upgrades the permission tables in the `mysql` database with some new fields
</li><li>Does a very quick check of all tables and marks them as compatible with [MariaDB 10.1](/kb/en/what-is-mariadb-101/)
<ul start="1"><li>In most cases this should be a fast operation (depending of course on the number of tables)
</li></ul>
</li></ol>
6 Add new options to [my.cnf](/kb/en/configuring-mariadb-with-mycnf/) to enable features
<ul start="1"><li>If you change <code class="highlight fixed" style="white-space:pre-wrap">my.cnf</code> then you need to restart `mysqld` with e.g. `sudo service mysql restart` or `sudo service mariadb restart`.
</li></ul>

## Upgrading to [MariaDB 10.1](/kb/en/what-is-mariadb-101/) from an older version of MariaDB

If you have installed [MariaDB 5.5](/kb/en/what-is-mariadb-55/) or [MariaDB 10.0](/kb/en/what-is-mariadb-100/) on your Debian 8 "Jessie" machine from the MariaDB repositories you will need to upgrade to [MariaDB 10.1](/kb/en/what-is-mariadb-101/) when upgrading to Debian 9 "Stretch". You can choose to continue using the MariaDB repositories or move to using the Debian repositories.

If you want to continue using the MariaDB repositories edit the MariaDB entry in your sources.list and change every instance of 5.5 or 10.0 to 10.1. Then upgrade as suggested [above](#upgrading-to-mariadb-101-from-mysql-55).

If you want to move to using [MariaDB 10.1](/kb/en/what-is-mariadb-101/) from the Debian repositories, delete or comment out the MariaDB entries in your sources.list file. Then upgrade as suggested [above](#upgrading-to-mariadb-101-from-mysql-55).

If you are already using [MariaDB 10.1](/kb/en/what-is-mariadb-101/) on your Debian 8 "Jessie" machine, you can choose to continue to use the MariaDB repositories or move to using the Debian repositories as with [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and 10.0. In either case, the upgrade will at most be just a minor upgrade from one version of [MariaDB 10.1](/kb/en/what-is-mariadb-101/) to a newer version. In the case that you are already on the current version of MariaDB that exists in the Debian repositories or a newer one) MariaDB will not be upgraded during the system upgrade but will be upgraded when future versions of MariaDB are released.

You should always perform a compete backup of your data prior to performing any major system upgrade, even if MariaDB itself is not being upgraded!

## MariaDB Galera Cluster

If you have been using MariaDB Galera Cluster 5.5 or 10.0 on Debian 8 "Jessie" it is worth mentioning that [Galera Cluster](/kb/en/galera/) is included by default in [MariaDB 10.1](/kb/en/what-is-mariadb-101/), there is no longer a need to install a separate `mariadb-galera-server` package.

## Configuration options for advanced database users

To get better performance from MariaDB used in production environments, here are some suggested additions to [your configuration file](/kb/en/configuring-mariadb-with-mycnf/) which in Debian is at `/etc/mysql/mariadb.d/my.cnf`:

```sql
[[mysqld]]
# Cache for disk based temporary files
aria_pagecache_buffer_size=128M
# If you are not using MyISAM tables directly (most people are using InnoDB)
key_buffer_size=64K
```

The reason for the above change is that MariaDB is using the newer [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine/) storage engine for disk based temporary files instead of MyISAM. The main benefit of Aria is that it can cache both indexes and rows and thus gives better performance than MyISAM for large queries.

## Secure passwordless root accounts only on new installs

Unlike the old MySQL packages in Debian, [MariaDB 10.0](/kb/en/what-is-mariadb-100/) onwards in Debian uses unix socket authentication on new installs to avoid root password management issues and thus be more secure and easier to use with provision systems of the cloud age.

This only affects new installs. Upgrades from old versions will continue to use whatever authentication and user accounts already existed. This is however good to know, because it can affect upgrades of dependant systems, typically e.g. require users to rewrite their Ansible scripts and similar tasks. The new feature is much easier than the old, so adjusting for it requires little work.

## See also

- [Differences in MariaDB in Debian (and Ubuntu)](/mariadb-administration/getting-installing-and-upgrading-mariadb/troubleshooting-installation-issues/installation-issues-on-debian-and-ubuntu/differences-in-mariadb-in-debian-and-ubuntu/)
- [Configuring MariaDB for optimal performance](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-performance-advanced-configurations/configuring-mariadb-for-optimal-performance/)
- [New features in MariaDB you should considering using](/kb/en/mariadb-vs-mysql-features/)
- [What is MariaDB 10.1](/kb/en/what-is-mariadb-101/)
- [General instructions for upgrading from MySQL to MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-upgrading-from-mysql-to-mariadb/upgrading-from-mysql-to-mariadb/)

## Comments and suggestions

If you have comments or suggestions on things we can add or change to improve this page. Please add them as comments below.

## Notes

1 [↑](#_ref-0) The `innodb-open-files` variable defaults to the value of `table-open-cache` (`400` is the default) if it is set to any value less than `10` so long as `innodb-file-per-table` is set to `1` or `TRUE` (the default). If `innodb_file_per_table` is set to `0` or `FALSE` <strong>and</strong> `innodb-open-files` is set to a value less than `10`, the default is `300`