# Upgrading from MariaDB 10.0 to MariaDB 10.1

## What You Need to Know

There are no changes in table or index formats between [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and [MariaDB
10.1](/kb/en/what-is-mariadb-101/), so on most servers the upgrade should be painless.

### How to Upgrade

For Windows, see [Upgrading MariaDB on Windows](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-on-windows) instead.

For MariaDB Galera Cluster, see [Upgrading from MariaDB Galera Cluster 10.0 to MariaDB 10.1 with Galera Cluster](/replication/galera-cluster/upgrading-galera-cluster/upgrading-galera-cluster-upgrading-from-mariadb-galera-cluster-100-to-maria) instead.

Before you upgrade, it would be best to take a backup of your database. This is always a good idea to do before an upgrade. We would recommend [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/).

The suggested upgrade procedure is:

1 Modify the repository configuration, so the system's package manager installs [MariaDB 10.1](/kb/en/what-is-mariadb-101/). For example,
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

### Incompatible Changes Between 10.0 and 10.1

As mentioned previously, on most servers upgrading from 10.0 should be painless.
However, there are some things that have changed which could affect an upgrade:

#### Storage Engines

- The [ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive) storage engine is no longer enabled by default, and the plugin needs to be specifically enabled.
- The [BLACKHOLE](/columns-storage-engines-and-plugins/storage-engines/blackhole) storage engine is no longer enabled by default, and the plugin needs to be specifically enabled.

#### Replication

- [MariaDB 10.1](/kb/en/what-is-mariadb-101/) introduces new, standards-compliant behavior for dealing with [primary keys over nullable columns](/replication/optimization-and-tuning/optimization-and-indexes/primary-keys-with-nullable-columns). In certain edge cases this could cause replication issues when replicating from a [MariaDB 10.0](/kb/en/what-is-mariadb-100/) master to a [MariaDB 10.1](/kb/en/what-is-mariadb-101/) slave using [statement-based replication](/kb/en/binary-log-formats/#statement-based). See [MDEV-12248](https://jira.mariadb.org/browse/MDEV-12248).

#### Options That Have Changed Default Values

Most of the following options have increased in value to give better performance.

<table><tbody><tr><th>Option</th><th>Old default value</th><th>New default value</th></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_log_compressed_pages">innodb_log_compressed_pages</a></td><td><code>ON</code></td><td><code>OFF</code></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#join_buffer_size">join_buffer_size</a></td><td><code>128K</code></td><td><code>256K</code></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#max_allowed_packet">max_allowed_packet</a></td><td><code>1M</code></td><td><code>4M</code></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#query_alloc_block_size">query_alloc_block_size</a></td><td><code>8192</code></td><td><code>16384</code></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#query_cache_size">query_cache_size</a></td><td><code>0</code></td><td><code>1M</code></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#query_cache_type">query_cache_type</a></td><td><code>ON</code></td><td><code>OFF</code></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#sync_master_info">sync_master_info</a></td><td><code>0</code></td><td><code>10000</code></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#sync_relay_log">sync_relay_log</a></td><td><code>0</code></td><td><code>10000</code></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#sync_relay_log_info">sync_relay_log_info</a></td><td><code>0</code></td><td><code>10000</code></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#query_prealloc_size">query_prealloc_size</a></td><td><code>8192</code></td><td><code>24576</code></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#secure_auth">secure_auth</a></td><td><code>OFF</code></td><td><code>ON</code></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#sql_log_bin">sql_log_bin</a></td><td></td><td>No longer affects replication of events in a Galera cluster.</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#sql_mode">sql_mode</a></td><td><code>empty</code></td><td><code>NO_AUTO_CREATE_USER, NO_ENGINE_SUBSTITUTION</code></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#table_open_cache">table_open_cache</a></td><td><code>400</code></td><td><code>2000</code></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#thread_pool_max_threads">thread_pool_max_threads</a></td><td><code>500</code></td><td><code>1000</code></td></tr>
</tbody></table>

#### Options That Have Been Removed or Renamed

The following options should be removed or renamed if you use them in your config files:

<table><tbody><tr><th>Option</th><th>Reason</th></tr>
<tr><td><a href="/kb/en/server-system-variables/#rpl_recovery_rank">rpl_recovery_rank</a></td><td>Unused in 10.0</td></tr>
</tbody></table>

#### Other Issues

Note that explicit or implicit casts from MAX(string) to INT, DOUBLE or DECIMAL now produce warnings ([MDEV-8852](https://jira.mariadb.org/browse/MDEV-8852)).

### Major New Features To Consider

You might consider using the following major new features in [MariaDB 10.1](/kb/en/what-is-mariadb-101/):

- [Galera Cluster](/kb/en/galera/) is now included by default.
- [Encryption](/kb/en/data-at-rest-encryption/)
- [InnoDB/XtraDB Page Compression](/kb/en/compression/)

## Notes



## See Also

- [The features in MariaDB 10.1](/kb/en/what-is-mariadb-101/)
- [Upgrading from MariaDB Galera Cluster 10.0 to MariaDB 10.1 with Galera Cluster](/replication/galera-cluster/upgrading-galera-cluster/upgrading-galera-cluster-upgrading-from-mariadb-galera-cluster-100-to-maria)
- [Upgrading from MariaDB 10.1 to MariaDB 10.2](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-101-to-mariadb-102)
- [Upgrading from MariaDB 5.5 to MariaDB 10.0](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-55-to-mariadb-100)