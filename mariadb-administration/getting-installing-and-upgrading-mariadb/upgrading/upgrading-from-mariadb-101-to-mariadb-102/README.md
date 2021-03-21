# Upgrading from MariaDB 10.1 to MariaDB 10.2

### How to Upgrade

For Windows, see [Upgrading MariaDB on Windows](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-on-windows/) instead.

For MariaDB Galera Cluster, see [Upgrading from MariaDB 10.1 to MariaDB 10.2 with Galera Cluster](/replication/galera-cluster/upgrading-galera-cluster/upgrading-from-mariadb-101-to-mariadb-102-with-galera-cluster/) instead.

Before you upgrade, it would be best to take a backup of your database. This is always a good idea to do before an upgrade. We would recommend [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/).

The suggested upgrade procedure is:

1 Modify the repository configuration, so the system's package manager installs [MariaDB 10.2](/kb/en/what-is-mariadb-102/). For example,
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, see [Updating the MariaDB APT repository to a New Major Release](/kb/en/installing-mariadb-deb-files/#updating-the-mariadb-apt-repository-to-a-new-major-release) for more information.
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, see [Updating the MariaDB YUM repository to a New Major Release](/kb/en/yum/#updating-the-mariadb-yum-repository-to-a-new-major-release) for more information.
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, see [Updating the MariaDB ZYpp repository to a New Major Release](/kb/en/installing-mariadb-with-zypper/#updating-the-mariadb-zypp-repository-to-a-new-major-release) for more information.
</li></ul>
2 Set <a undefined>innodb_fast_shutdown</a> to `0`. It can be changed dynamically with <a undefined>SET GLOBAL</a>. For example: <br>
<code class="fixed" style="white-space:pre-wrap">SET GLOBAL innodb_fast_shutdown=0;</code>
<ul start="1"><li>This step is not necessary when upgrading to [MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/) or later. Omitting it can make the upgrade process far faster. See [MDEV-12289](https://jira.mariadb.org/browse/MDEV-12289) for more information.
</li></ul>
3 [Stop MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/starting-and-stopping-mariadb-automatically/).
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
6 Make any desired changes to configuration options in [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), such as `my.cnf`. This includes removing any options that are no longer supported.
7 [Start MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/starting-and-stopping-mariadb-automatically/).
8 Run [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade/).
<ul start="1"><li>`mysql_upgrade` does two things:
<ol start="1"><li>Ensures that the system tables in the [mysq](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/)l database are fully compatible with the new version.
</li><li>Does a very quick check of all tables and marks them as compatible with the new version of MariaDB .
</li></ol>
</li></ul>

### Incompatible Changes Between 10.1 and 10.2

On most servers upgrading from 10.1 should be painless. However, there are some things that have changed which could affect an upgrade:

#### InnoDB Instead of XtraDB

[MariaDB 10.2](/kb/en/what-is-mariadb-102/) uses [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) as the default storage engine, rather than XtraDB, used in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before. See [Why does MariaDB 10.2 use InnoDB instead of XtraDB?](/kb/en/why-does-mariadb-102-use-innodb-instead-of-xtradb/) In most cases this should have minimal effect as the latest InnoDB has incorporated most of the improvements made in earlier versions of XtraDB. Note that certain [XtraDB system variables](/kb/en/xtradbinnodb-server-system-variables/) are now ignored (although they still exist so as to permit easy upgrading).

#### Options That Have Changed Default Values

In particular, take note of the changes to [innodb_strict_mode](/kb/en/xtradbinnodb-server-system-variables/#innodb_strict_mode), [sql_mode](/kb/en/server-system-variables/#sql_mode), [binlog_format](/kb/en/replication-and-binary-log-server-system-variables/#binlog_format), [binlog_checksum](/kb/en/replication-and-binary-log-server-system-variables/#binlog_checksum) and [innodb_checksum_algorithm](/kb/en/xtradbinnodb-server-system-variables/#innodb_checksum_algorithm).

<table><tbody><tr><th>Option</th><th>Old default value</th><th>New default value</th></tr>
<tr><td><a href="/kb/en/aria-system-variables/#aria_recover_options">aria_recover(_options)</a></td><td>NORMAL</td><td>BACKUP, QUICK</td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#binlog_annotate_row_events">binlog_annotate_row_events</a></td><td>OFF</td><td>ON</td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#binlog_checksum">binlog_checksum</a></td><td>NONE</td><td>CRC32</td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#binlog_format">binlog_format</a></td><td>STATEMENT</td><td>MIXED</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#group_concat_max_len">group_concat_max_len</a></td><td>1024</td><td>1048576</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_autoinc_lock_mode">innodb_autoinc_lock_mode</a></td><td>1</td><td>2</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_dump_at_shutdown">innodb_buffer_pool_dump_at_shutdown</a></td><td>OFF</td><td>ON</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_dump_pct">innodb_buffer_pool_dump_pct</a></td><td>100</td><td>25</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_instances">innodb_buffer_pool_instances</a></td><td>8</td><td>Varies</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_load_at_startup">innodb_buffer_pool_load_at_startup</a></td><td>OFF</td><td>ON</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_checksum_algorithm">innodb_checksum_algorithm</a></td><td>innodb</td><td>crc32</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_file_format">innodb_file_format</a></td><td>Antelope</td><td>Barracuda</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_large_prefix">innodb_large_prefix</a></td><td>OFF</td><td>ON</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_lock_schedule_algorithm">innodb_lock_schedule_algorithm</a></td><td>VATS</td><td>FCFS</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_log_compressed_pages">innodb_log_compressed_pages</a></td><td>OFF</td><td>ON</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_max_dirty_pages_pct_lwm">innodb_max_dirty_pages_pct_lwm</a></td><td>0.001000</td><td>0</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_max_undo_log_size">innodb_max_undo_log_size</a></td><td>1073741824</td><td>10485760</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_purge_threads">innodb_purge_threads</a></td><td>1</td><td>4</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_strict_mode">innodb_strict_mode</a></td><td>OFF</td><td>ON</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_undo_directory">innodb_undo_directory</a></td><td>.</td><td>NULL</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_use_atomic_writes">innodb_use_atomic_writes</a></td><td>OFF</td><td>ON</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_use_trim">innodb_use_trim</a></td><td>OFF</td><td>ON</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#lock_wait_timeout">lock_wait_timeout</a></td><td>31536000</td><td>86400</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#log_slow_admin_statements">log_slow_admin_statements</a></td><td>OFF</td><td>ON</td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#log_slow_slave_statements">log_slow_slave_statements</a></td><td>OFF</td><td>ON</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#log_warnings">log_warnings</a></td><td>1</td><td>2</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#max_allowed_packet">max_allowed_packet</a></td><td>4M</td><td>16M</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#max_long_data_size">max_long_data_size</a></td><td>4M</td><td>16M</td></tr>
<tr><td><a href="/kb/en/myisam-system-variables/#myisam_recover_options">myisam_recover_options</a></td><td>NORMAL</td><td>BACKUP, QUICK</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#optimizer_switch">optimizer_switch</a></td><td>See <a href="/kb/en/optimizer_switch/">Optimizer Switch</a> for details.</td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_annotate_row_events">replicate_annotate_row_events</a></td><td>OFF</td><td>ON</td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#server_id">server_id</a></td><td>0</td><td>1</td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#slave_net_timeout">slave_net_timeout</a></td><td>3600</td><td>60</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#sql_mode">sql_mode</a></td><td>NO_AUTO_CREATE_USER, NO_ENGINE_SUBSTITUTION</td><td>STRICT_TRANS_TABLES, ERROR_FOR_DIVISION_BY_ZERO, NO_AUTO_CREATE_USER, NO_ENGINE_SUBSTITUTION</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#thread_cache_size">thread_cache_size</a></td><td>0</td><td>Auto</td></tr>
<tr><td><a href="/kb/en/thread-pool-system-and-status-variables/">thread_pool_max_threads</a></td><td>1000</td><td>65536</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#thread_stack">thread_stack</a></td><td>295936</td><td>299008</td></tr>
</tbody></table>

#### Options That Have Been Removed or Renamed

The following options should be removed or renamed if you use them in your [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

<table><tbody><tr><th>Option</th><th>Reason</th></tr>
<tr><td>aria_recover</td><td>Renamed to <a href="/kb/en/aria-system-variables/#aria_recover_options">aria_recover_options</a> to match  <a href="/kb/en/myisam-system-variables/#myisam_recover_options">myisam_recover_options</a>.</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_additional_mem_pool_size">innodb_additional_mem_pool_size</a></td><td>Deprecated in <a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a>.</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_api_bk_commit_interval">innodb_api_bk_commit_interval</a></td><td>Memcache never implemented in MariaDB.</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_api_disable_rowlock">innodb_api_disable_rowlock</a></td><td>Memcache never implemented in MariaDB.</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_api_enable_binlog">innodb_api_enable_binlog</a></td><td>Memcache never implemented in MariaDB.</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_api_enable_mdl">innodb_api_enable_mdl</a></td><td>Memcache never implemented in MariaDB.</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_api_trx_level">|innodb_api_trx_level</a></td><td>Memcache never implemented in MariaDB.</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_use_sys_malloc">innodb_use_sys_malloc</a></td><td>Deprecated in <a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a>.</td></tr>
</tbody></table>

#### Reserved Words

New [reserved words](/sql-statements-structure/sql-language-structure/reserved-words/): OVER, RECURSIVE and ROWS. These can no longer be used as [identifiers](/sql-statements-structure/sql-language-structure/identifier-names/) without being quoted.

#### TokuDB

[TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb/) has been split into a separate package, mariadb-plugin-tokudb.

#### Replication

[Replication](/replication/standard-replication/) from legacy MySQL servers may require setting [binlog_checksum](/kb/en/replication-and-binary-log-server-system-variables/#binlog_checksum) to NONE.

#### SQL Mode

[SQL_MODE](/kb/en/sql_mode/) has been changed; in particular, NOT NULL fields with no default will no longer fall back to a dummy value for inserts which do not specify a value for that field.

#### Auto_increment

[Auto_increment](/columns-storage-engines-and-plugins/data-types/auto_increment/) columns are no longer permitted in [CHECK constraints](/sql-statements-structure/sql-statements/data-definition/constraint/), [DEFAULT value expressions](/kb/en/create-table/#default) and [virtual columns](/kb/en/virtual-computed-columns/). They were permitted in earlier versions, but did not work correctly.

#### TLS

Starting with [MariaDB 10.2](/kb/en/what-is-mariadb-102/), when the user specifies the `--ssl` option with a [client or utility](/clients-utilities/), the [client or utility](/clients-utilities/) will not [verify the server certificate](/kb/en/secure-connections-overview/#server-certificate-verification) by default. In order to verify the server certificate, the user must specify the `--ssl-verify-server-cert` option to the [client or utility](/clients-utilities/). For more information, see the [list of options](/kb/en/mysql-command-line-client/#options) for the [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) client.

### Major New Features To Consider

You might consider using the following major new features in [MariaDB 10.2](/kb/en/what-is-mariadb-102/):

- [Window Functions](/built-in-functions/special-functions/window-functions/)
- [mysqlbinlog](/clients-utilities/mysqlbinlog/) now supports continuous binary log backups
- [Recursive Common Table Expressions](/kb/en/recursive-common-table-expressions-overview/)
- [JSON functions](/built-in-functions/special-functions/json-functions/)
- See also [System Variables Added in MariaDB 10.2](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/system-variables-added-in-mariadb-102/).

### See Also

- [The features in MariaDB 10.2](/kb/en/what-is-mariadb-102/)
- [Upgrading from MariaDB 10.1 to MariaDB 10.2 with Galera Cluster](/replication/galera-cluster/upgrading-galera-cluster/upgrading-from-mariadb-101-to-mariadb-102-with-galera-cluster/)
- [Upgrading from MariaDB 10.0 to MariaDB 10.1](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-100-to-mariadb-101/)
- [Upgrading from MariaDB 5.5 to MariaDB 10.0](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-55-to-mariadb-100/)