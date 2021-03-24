# Upgrading from MariaDB 10.2 to MariaDB 10.3

### How to Upgrade

For Windows, see [Upgrading MariaDB on Windows](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-on-windows/) instead.

For MariaDB Galera Cluster, see [Upgrading from MariaDB 10.2 to MariaDB 10.3 with Galera Cluster](/replication/galera-cluster/upgrading-galera-cluster/upgrading-from-mariadb-102-to-mariadb-103-with-galera-cluster/) instead.

Before you upgrade, it would be best to take a backup of your database. This is always a good idea to do before an upgrade. We would recommend [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/).

The suggested upgrade procedure is:

1 Modify the repository configuration, so the system's package manager installs [MariaDB 10.3](/kb/en/what-is-mariadb-103/). For example,
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
<ol start="1"><li>Ensures that the system tables in the [mysq](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/)l database are fully compatible with the new version.
</li><li>Does a very quick check of all tables and marks them as compatible with the new version of MariaDB .
</li></ol>
</li></ul>

### Incompatible Changes Between 10.2 and 10.3

On most servers upgrading from 10.2 should be painless. However, there are some things that have changed which could affect an upgrade:

#### Options That Have Changed Default Values

<table><tbody><tr><th>Option</th><th>Old default value</th><th>New default value</th></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_flush_method">innodb_flush_method</a></td><td>(empty)</td><td>fsync</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_spin_wait_delay">innodb_spin_wait_delay</a></td><td>6</td><td>4</td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_max_stage_classes">performance_schema_max_stage_classes</a></td><td>150</td><td>160</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#plugin_maturity">plugin_maturity</a></td><td>unknown</td><td>One less than the server maturity</td></tr>
</tbody></table>

#### Options That Have Been Removed or Renamed

The following options should be removed or renamed if you use them in your [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

<table><tbody><tr><th>Option</th><th>Reason</th></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_buffer_pool_populate">innodb_buffer_pool_populate</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_cleaner_lsn_age_factor">innodb_cleaner_lsn_age_factor</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_corrupt_table_action">innodb_corrupt_table_action</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_empty_free_list_algorithm">innodb_empty_free_list_algorithm</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_fake_changes">innodb_fake_changes</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_file_format">innodb_file_format</a></td><td>The <a href="/kb/en/xtradbinnodb-file-format/">InnoDB file format</a> is now Barracuda, and the old Antelope file format is no longer supported.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_file_format_check">innodb_file_format_check</a></td><td>No longer necessary as the Antelope <a href="/kb/en/xtradbinnodb-file-format/">InnoDB file format</a> is no longer supported.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_file_format_max">innodb_file_format_max</a></td><td>No longer necessary as the Antelope <a href="/kb/en/xtradbinnodb-file-format/">InnoDB file format</a> is no longer supported.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_foreground_preflush">innodb_foreground_preflush</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_instrument_semaphores">innodb_instrument_semaphores</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_kill_idle_transaction">innodb_kill_idle_transaction</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_large_prefix">innodb_large_prefix</a></td><td>Large index key prefixes were made default from <a href="/kb/en/what-is-mariadb-102/">MariaDB 10.2</a>, and limiting tables to small prefixes is no longer permitted in <a href="/kb/en/what-is-mariadb-103/">MariaDB 10.3</a>.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_locking_fake_changes">innodb_locking_fake_changes</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_log_arch_dir">innodb_log_arch_dir</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_log_arch_expire_sec">innodb_log_arch_expire_sec</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_log_archive">innodb_log_archive</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_log_block_size">innodb_log_block_size</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_log_checksum_algorithm">innodb_log_checksum_algorithm</a></td><td>Translated to <a href="/kb/en/innodb-system-variables/#innodb_log_checksums">innodb_log_checksums</a> (NONE to OFF, everything else to ON); only existed to allow easier upgrade from earlier XtraDB versions.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_mtflush_threads">innodb_mtflush_threads</a></td><td>Replaced by the <a href="/kb/en/innodb-system-variables/#innodb_page_cleaners">innodb_page_cleaners</a> system variable.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_sched_priority_cleaner">innodb_sched_priority_cleaner</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_show_locks_held">innodb_show_locks_held</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_show_verbose_locks">innodb_show_verbose_locks</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_support_xa">innodb_support_xa</a></td><td>XA transactions are always supported.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_use_fallocate">innodb_use_fallocate</a></td><td></td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_use_global_flush_log_at_trx_commit">innodb_use_global_flush_log_at_trx_commit</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_use_mtflush">innodb_use_mtflush</a></td><td>Replaced by the <a href="/kb/en/innodb-system-variables/#innodb_page_cleaners">innodb_page_cleaners</a> system variable.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_use_stacktrace">innodb_use_stacktrace</a></td><td>Used in XtraDB-only</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_use_trim">innodb_use_trim</a></td><td></td></tr>
</tbody></table>

#### Reserved Words

- New [reserved words](/sql-statements-structure/sql-language-structure/reserved-words/): EXCEPT and INTERSECT. These can no longer be used as [identifiers](/sql-statements-structure/sql-language-structure/identifier-names/) without being quoted.

#### SQL_MODE=ORACLE

- [MariaDB 10.3](/kb/en/what-is-mariadb-103/) has introduced major new Oracle compatibility features. If you upgrade and are using this setting, please check the [changes carefully](/kb/en/what-is-mariadb-103/).

#### Functions

- As a result of implementing Table Value Constructors, the [VALUES function](/kb/en/values/) has been renamed to VALUE().
- Functions that used to only return 64-bit now can return 32-bit results ([MDEV-12619](https://jira.mariadb.org/browse/MDEV-12619)). This could cause incompatibilities with strongly-typed clients.

#### mysqldump

- [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) in [MariaDB 10.3](/kb/en/what-is-mariadb-103/) includes logic to cater for the [mysql.transaction_registry table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqltransaction_registry-table/). `mysqldump` from an earlier MariaDB release cannot be used on [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and beyond.

#### MariaDB Backup and Percona XtraBackup

- [Percona XtraBackup](/kb/en/percona-xtrabackup/) is not compatible with [MariaDB 10.3](/kb/en/what-is-mariadb-103/). Installations currently using XtraBackup should upgrade to [MariaDB Backup](/kb/en/mariadb-backup/) before upgrading to [MariaDB 10.3](/kb/en/what-is-mariadb-103/).

#### Privileges

- If a user has the [SUPER privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) but not the `DELETE HISTORY` privilege, running [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade/) will grant `DELETE HISTORY` as well.

### Major New Features To Consider

You might consider using the following major new features in [MariaDB 10.3](/kb/en/what-is-mariadb-103/):

- [System-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables/)
- [Sequences](/sql-statements-structure/sequences/)
- See also [System Variables Added in MariaDB 10.3](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/system-variables-added-in-mariadb-103/).

### See Also

- [The features in MariaDB 10.3](/kb/en/what-is-mariadb-103/)
- [Upgrading from MariaDB 10.2 to MariaDB 10.3 with Galera Cluster](/replication/galera-cluster/upgrading-galera-cluster/upgrading-from-mariadb-102-to-mariadb-103-with-galera-cluster/)
- [Upgrading from MariaDB 10.1 to MariaDB 10.2](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-101-to-mariadb-102/)
- [Upgrading from MariaDB 10.0 to MariaDB 10.1](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-100-to-mariadb-101/)