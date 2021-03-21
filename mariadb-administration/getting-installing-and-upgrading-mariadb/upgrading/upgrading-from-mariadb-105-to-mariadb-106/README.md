# Upgrading from MariaDB 10.5 to MariaDB 10.6

[MariaDB 10.6](/kb/en/what-is-mariadb-106/) is still under active development, so details on this page will change frequently.

### How to Upgrade

For Windows, see [Upgrading MariaDB on Windows](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-on-windows/).

For MariaDB Galera Cluster, see [Upgrading from MariaDB 10.5 to MariaDB 10.6 with Galera Cluster](upgrading-from-mariadb-105-to-mariadb-106-with-galera-cluster) instead.

Before you upgrade, it would be best to take a backup of your database. This is always a good idea to do before an upgrade. We would recommend [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/).

The suggested upgrade procedure is:

1 Modify the repository configuration, so the system's package manager installs [MariaDB 10.6](/kb/en/what-is-mariadb-106/). For example,
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, see [Updating the MariaDB APT repository to a New Major Release](/kb/en/installing-mariadb-deb-files/#updating-the-mariadb-apt-repository-to-a-new-major-release) for more information.
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, see [Updating the MariaDB YUM repository to a New Major Release](/kb/en/yum/#updating-the-mariadb-yum-repository-to-a-new-major-release) for more information.
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, see [Updating the MariaDB ZYpp repository to a New Major Release](/kb/en/installing-mariadb-with-zypper/#updating-the-mariadb-zypp-repository-to-a-new-major-release) for more information.
</li></ul>
2 [Stop MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/starting-and-stopping-mariadb-automatically/).
3 Uninstall the old version of MariaDB.
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, execute the following: <br>
<code class="fixed" style="white-space:pre-wrap">sudo apt-get remove mariadb-server</code>
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, execute the following: <br>
<code class="fixed" style="white-space:pre-wrap">sudo yum remove MariaDB-server</code>
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, execute the following: <br>
<code class="fixed" style="white-space:pre-wrap">sudo zypper remove MariaDB-server</code>
</li></ul>
4 Install the new version of MariaDB.
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, see [Installing MariaDB Packages with APT](/kb/en/installing-mariadb-deb-files/#installing-mariadb-packages-with-apt) for more information.
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, see [Installing MariaDB Packages with YUM](/kb/en/yum/#installing-mariadb-packages-with-yum) for more information.
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, see [Installing MariaDB Packages with ZYpp](/kb/en/installing-mariadb-with-zypper/#installing-mariadb-packages-with-zypp) for more information.
</li></ul>
5 Make any desired changes to configuration options in [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), such as `my.cnf`. This includes removing any options that are no longer supported.
6 [Start MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/starting-and-stopping-mariadb-automatically/).
7 Run [mariadb-upgrade](/clients-utilities/mariadb-upgrade/).
<ul start="1"><li>`mariadb-upgrade` does two things:
<ol start="1"><li>Ensures that the system tables in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/) database are fully compatible with the new version.
</li><li>Does a very quick check of all tables and marks them as compatible with the new version of MariaDB .
</li></ol>
</li></ul>

### Incompatible Changes Between 10.5 and 10.6

On most servers upgrading from 10.5 should be painless. However, there are some things that have changed which could affect an upgrade:

#### InnoDB COMPRESSED Row Format

From [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/), tables that are of the [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) row format are read-only by default. This is the first step towards removing write support and deprecating the feature.

Set the  [innodb_read_only_compressed](/kb/en/innodb-system-variables/#innodb_read_only_compressed) variable to `OFF` to make the tables writable.

#### Options That Have Changed Default Values

<table><tbody><tr><th>Option</th><th>Old default value</th><th>New default value</th></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_flush_method">innodb_flush_method</a></td><td>fsync</td><td>O_DIRECT</td></tr>
</tbody></table>

#### Options That Have Been Removed or Renamed

The following options should be removed or renamed if you use them in your [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

<table><tbody><tr><th>Option</th><th>Reason</th></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_adaptive_max_sleep_delay">innodb_adaptive_max_sleep_delay</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_background_scrub_data_check_interval">innodb_background_scrub_data_check_interval</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_background_scrub_data_compressed">innodb_background_scrub_data_compressed</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_background_scrub_data_interval">innodb_background_scrub_data_interval</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_background_scrub_data_uncompressed">innodb_background_scrub_data_uncompressed</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_buffer_pool_instances">innodb_buffer_pool_instances</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_checksum_algorithm">innodb_checksum_algorithm</a></td><td>The variable is still present, but the <code>*innodb</code> and <code>*none</code> options have been removed as the <code>crc32</code> algorithm only is supported from <a href="/kb/en/what-is-mariadb-106/">MariaDB 10.6</a>.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_commit_concurrency">innodb_commit_concurrency</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_concurrency_tickets">innodb_concurrency_tickets</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_file_format">innodb_file_format</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_large_prefix">innodb_large_prefix</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_log_checksums">innodb_log_checksums</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_log_compressed_pages">innodb_log_compressed_pages</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_log_files_in_group">innodb_log_files_in_group</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_log_optimize_ddl">innodb_log_optimize_ddl</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_page_cleaners">innodb_page_cleaners</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_replication_delay">innodb_replication_delay</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_scrub_log">innodb_scrub_log</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_scrub_log_speed">innodb_scrub_log_speed</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_thread_concurrency">innodb_thread_concurrency</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_thread_sleep_delay">innodb_thread_sleep_delay</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_undo_logs">innodb_undo_logs</a></td></tr>
</tbody></table>

#### Deprecated Options

The following options have been deprecated. They have not yet been removed, but will be in a future version, and should ideally no longer be used.

<table><tbody><tr><th>Option</th><th>Reason</th></tr>
<tr><td><a href="/kb/en/galera-cluster-system-variables/#wsrep_replicate_myisam">wsrep_replicate_myisam</a></td><td>Use <a href="/kb/en/galera-cluster-system-variables/#wsrep_mode">wsrep_mode</a> instead.</td></tr>
<tr><td><a href="/kb/en/galera-cluster-system-variables/#wsrep_strict_ddl">wsrep_strict_ddl</a></td><td>Use <a href="/kb/en/galera-cluster-system-variables/#wsrep_mode">wsrep_mode</a> instead.</td></tr>
</tbody></table>

### Major New Features To Consider

- See also [System Variables Added in MariaDB 10.6](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/system-variables-added-in-mariadb-106/).

### See Also

- [The features in MariaDB 10.6](/kb/en/what-is-mariadb-106/)

- [Upgrading from MariaDB 10.5 to MariaDB 10.6 with Galera Cluster](upgrading-from-mariadb-105-to-mariadb-106-with-galera-cluster)

- [Upgrading from MariaDB 10.4 to MariaDB 10.5](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-104-to-mariadb-105/)
- [Upgrading from MariaDB 10.3 to MariaDB 10.4](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-103-to-mariadb-104/)
- [Upgrading from MariaDB 10.2 to MariaDB 10.3](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-102-to-mariadb-103/)