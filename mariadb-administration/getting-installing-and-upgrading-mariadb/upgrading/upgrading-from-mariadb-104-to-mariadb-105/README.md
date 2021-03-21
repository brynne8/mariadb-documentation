# Upgrading from MariaDB 10.4 to MariaDB 10.5

### How to Upgrade

For Windows, see [Upgrading MariaDB on Windows](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-on-windows/) instead.

For Windows, see [Upgrading MariaDB on Windows](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-on-windows/) instead.

For MariaDB Galera Cluster, see [Upgrading from MariaDB 10.4 to MariaDB 10.5 with Galera Cluster](upgrading-from-mariadb-104-to-mariadb-105-with-galera-cluster) instead.

Before you upgrade, it would be best to take a backup of your database. This is always a good idea to do before an upgrade. We would recommend [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/).

The suggested upgrade procedure is:

1 Modify the repository configuration, so the system's package manager installs [MariaDB 10.5](/kb/en/what-is-mariadb-105/). For example,
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
7 Run [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade/).
<ul start="1"><li>`mysql_upgrade` does two things:
<ol start="1"><li>Ensures that the system tables in the#[mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/) database are fully compatible with the new version.
</li><li>Does a very quick check of all tables and marks them as compatible with the new version of MariaDB .
</li></ol>
</li></ul>

### Incompatible Changes Between 10.4 and 10.5

On most servers upgrading from 10.4 should be painless. However, there are some things that have changed which could affect an upgrade:

#### Binary name changes

All binaries previously beginning with mysql now begin with mariadb, with symlinks for the corresponding mysql command.

Usually that shouldn't cause any changed behavior, but when starting the MariaDB server via [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/), or via the [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/) script symlink, the server process will now always be started as `mariadbd`, not `mysqld`.

So anything looking for the `mysqld` name in the system process list, like e.g. monitoring solutions, now needs for `mariadbd` instead when the server / service is not started directly, but via `mysqld_safe` or as a system service.

#### GRANT PRIVILEGE changes

A number of statements changed the privileges that they require. The old privileges were historically inappropriately chosen in the upstream. 10.5.2 fixes this problem. Note, these changes are incompatible to previous versions. A number of GRANT commands might be needed after upgrade.

- `SHOW BINLOG EVENTS` now requires the `BINLOG MONITOR` privilege (requred `REPLICATION SLAVE` prior to 10.5.2).
- `SHOW SLAVE HOSTS` now requires the `REPLICATION MASTER ADMIN` privilege (required `REPLICATION SLAVE` prior to 10.5.2).
- `SHOW SLAVE STATUS` now requires the `REPLICATION SLAVE ADMIN` or the `SUPER` privilege (required `REPLICATION CLIENT` or `SUPER` prior to 10.5.2).
- `SHOW RELAYLOG EVENTS` now requires the `REPLICATION SLAVE ADMIN` privilege (required `REPLICATION SLAVE` prior to 10.5.2).

#### Options That Have Changed Default Values

<table><tbody><tr><th>Option</th><th>Old default value</th><th>New default value</th></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_adaptive_hash_index">innodb_adaptive_hash_index</a></td><td>ON</td><td>OFF</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_checksum_algorithm">innodb_checksum_algorithm</a></td><td>crc32</td><td>full_crc32</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_log_optimize_ddl">innodb_log_optimize_ddl</a></td><td>ON</td><td>OFF</td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-system-variables/#slave_parallel_mode">slave_parallel_mode</a></td><td>conservative</td><td>optimistic</td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_max_cond_classes">performance_schema_max_cond_classes</a></td><td>80</td><td>90</td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_max_file_classes">performance_schema_max_file_classes</a></td><td>50</td><td>80</td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_max_mutex_classes">performance_schema_max_mutex_classes</a></td><td>200</td><td>210</td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_max_rwlock_classes">performance_schema_max_rwlock_classes</a></td><td>40</td><td>50</td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_setup_actors_size">performance_schema_setup_actors_size</a></td><td>100</td><td>-1</td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_setup_objects_size">performance_schema_setup_objects_size</a></td><td>100</td><td>-1</td></tr>
</tbody></table>

#### Options That Have Been Removed or Renamed

The following options should be removed or renamed if you use them in your [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

<table><tbody><tr><th>Option</th><th>Reason</th></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_checksums">innodb_checksums</a></td><td>Deprecated and functionality replaced by <a href="/kb/en/innodb-system-variables/#innodb_checksum_algorithm">innodb_checksum_algorithms</a> in <a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a>.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_idle_flush_pct">idle_flush_pct</a></td><td>Has had no effect since merging InnoDB 5.7 from mysql-5.7.9 (<a href="/kb/en/mariadb-1022-release-notes/">MariaDB 10.2.2</a>).</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_locks_unsafe_for_binlog">innodb_locks_unsafe_for_binlog</a></td><td>Deprecated in <a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a>. Use <a href="/kb/en/set-transaction/#read-committed">READ COMMITTED transaction isolation level</a> instead.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_rollback_segments">innodb_rollback_segments</a></td><td>Deprecated and replaced by <a href="/kb/en/innodb-system-variables/#innodb_undo_logs">innodb_undo_logs</a> in <a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a>.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_stats_sample_pages">innodb_stats_sample_pages</a></td><td>Deprecated in <a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a>. Use <a href="/kb/en/innodb-system-variables/#innodb_stats_transient_sample_pages">innodb_stats_transient_sample_pages</a> instead.</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#max_long_data_size">max_long_data_size</a></td><td>Deprecated and replaced by <a href="/kb/en/server-system-variables/#max_allowed_packet">max_allowed_packet</a> in <a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a>.</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#multi_range_count">multi_range_count</a></td><td>Deprecated and has had no effect since <a href="/kb/en/what-is-mariadb-53/">MariaDB 5.3</a>.</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#thread_concurrency">thread_concurrency</a></td><td>Deprecated and has had no effect since <a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a>.</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#timed_mutexes">timed_mutexes</a></td><td>Deprecated and has had no effect since <a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a>.</td></tr>
</tbody></table>

#### Deprecated Options

The following options have been deprecated. They have not yet been removed, but will be in a future version, and should ideally no longer be used.

<table><tbody><tr><th>Option</th><th>Reason</th></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_adaptive_max_sleep_delay">innodb_adaptive_max_sleep_delay</a></td><td>No need for thread throttling any more.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_background_scrub_data_check_interval">innodb_background_scrub_data_check_interval</a></td><td>Problematic ‘background scrubbing’ code removed.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_background_scrub_data_interval">innodb_background_scrub_data_interval</a></td><td>Problematic ‘background scrubbing’ code removed.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_background_scrub_data_compressed">innodb_background_scrub_data_compressed</a></td><td>Problematic ‘background scrubbing’ code removed.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_background_scrub_data_uncompressed">innodb_background_scrub_data_uncompressed</a></td><td>Problematic ‘background scrubbing’ code removed.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_buffer_pool_instances">innodb_buffer_pool_instances</a></td><td>Having more than one buffer pool is no longer necessary.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_commit_concurrency">innodb_commit_concurrency</a></td><td>No need for thread throttling any more.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_concurrency_tickets">innodb_concurrency_tickets</a></td><td>No need for thread throttling any more.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_log_files_in_group">innodb_log_files_in_group</a></td><td>Redo log was unnecessarily split into multiple files. Limited to 1 from <a href="/kb/en/what-is-mariadb-105/">MariaDB 10.5</a>.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_log_optimize_ddl">innodb_log_optimize_ddl</a></td><td>Prohibited optimizations.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_page_cleaners">innodb_page_cleaners</a></td><td>Having more than one page cleaner task no longer necessary.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_replication_delay">innodb_replication_delay</a></td><td>No need for thread throttling any more.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_scrub_log">innodb_scrub_log</a></td><td>Never really worked as intended, redo log format is being redone.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_scrub_log_speed">innodb_scrub_log_speed</a></td><td>Never really worked as intended, redo log format is being redone.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_thread_concurrency">innodb_thread_concurrency</a></td><td>No need for thread throttling any more.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_thread_sleep_delay">innodb_thread_sleep_delay</a></td><td>No need for thread throttling any more.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_undo_logs">innodb_undo_logs</a></td><td>It always makes sense to use the maximum number of rollback segments.</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#large_page_size">large_page_size</a></td><td>Unused since multiple page size support was added.</td></tr>
</tbody></table>

### Major New Features To Consider

You might consider using the following major new features in [MariaDB 10.5](/kb/en/what-is-mariadb-105/):

- The [S3 storage engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/) allows one to archive MariaDB tables in Amazon S3, or any third-party public or private cloud that implements S3 API.
- [ColumnStore](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/) columnar storage engine.
- See also [System Variables Added in MariaDB 10.5](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/system-variables-added-in-mariadb-105/).

### See Also

- [The features in MariaDB 10.5](/kb/en/what-is-mariadb-105/)

- [Upgrading from MariaDB 10.4 to MariaDB 10.5 with Galera Cluster](upgrading-from-mariadb-104-to-mariadb-105-with-galera-cluster)

- [Upgrading from MariaDB 10.3 to MariaDB 10.4](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-103-to-mariadb-104/)
- [Upgrading from MariaDB 10.2 to MariaDB 10.3](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-102-to-mariadb-103/)
- [Upgrading from MariaDB 10.1 to MariaDB 10.2](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-101-to-mariadb-102/)