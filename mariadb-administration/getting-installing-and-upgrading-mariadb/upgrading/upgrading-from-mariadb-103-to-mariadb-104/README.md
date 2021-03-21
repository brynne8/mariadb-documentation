# Upgrading from MariaDB 10.3 to MariaDB 10.4

### How to Upgrade

For Windows, see [Upgrading MariaDB on Windows](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-on-windows/) instead.

For MariaDB Galera Cluster, see [Upgrading from MariaDB 10.3 to MariaDB 10.4 with Galera Cluster](/replication/galera-cluster/upgrading-galera-cluster/upgrading-from-mariadb-103-to-mariadb-104-with-galera-cluster/) instead.

Before you upgrade, it would be best to take a backup of your database. This is always a good idea to do before an upgrade. We would recommend [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/).

The suggested upgrade procedure is:

1 Modify the repository configuration, so the system's package manager installs [MariaDB 10.4](/kb/en/what-is-mariadb-104/). For example,
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

### Incompatible Changes Between 10.3 and 10.4

On most servers upgrading from 10.3 should be painless. However, there are some things that have changed which could affect an upgrade:

#### Options That Have Changed Default Values

<table><tbody><tr><th>Option</th><th>Old default value</th><th>New default value</th></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-system-variables/#slave_transaction_retry_errors">slave_transaction_retry_errors</a></td><td>1213,1205</td><td>1158,1159,1160,1161,1205,1213,1429,2013,12701</td></tr>
<tr><td><a href="/kb/en/galera-cluster-system-variables/#wsrep_debug">wsrep_debug</a></td><td>OFF</td><td>NONE</td></tr>
<tr><td><a href="/kb/en/galera-cluster-system-variables/#wsrep_load_data_splitting">wsrep_load_data_splitting</a></td><td>ON</td><td>OFF</td></tr>
</tbody></table>

#### Options That Have Been Removed or Renamed

The following options should be removed or renamed if you use them in your [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

<table><tbody><tr><th>Option</th><th>Reason</th></tr>
</tbody></table>

#### Authentication and TLS

- See [Authentication from MariaDB 10.4](/mariadb-administration/user-server-security/user-account-management/authentication-from-mariadb-104/) for an overview of the changes.
- The [unix_socket authentication plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) is now default on Unix-like systems.
- TLSv1.0 is disabled by default in [MariaDB 10.4](/kb/en/what-is-mariadb-104/). See [tls_version](/kb/en/ssltls-system-variables/#tls_version) and [TLS Protocol Versions](/kb/en/secure-connections-overview/#tls-protocol-versions).

### Major New Features To Consider

You might consider using the following major new features in [MariaDB 10.4](/kb/en/what-is-mariadb-104/):

- [Galera](/replication/galera-cluster/) has been upgraded from [Galera](/replication/galera-cluster/) 3 to [Galera](/replication/galera-cluster/) 4.
- [System-versioning](/kb/en/temporal-data-tables/) extended with support for [application-time periods](/kb/en/temporal-data-tables/#application-time-periods).
- [User password expiry](/mariadb-administration/user-server-security/user-account-management/user-password-expiry/)
- [Account Locking](/mariadb-administration/user-server-security/user-account-management/account-locking/)
- See also [System Variables Added in MariaDB 10.4](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/system-variables-added-in-mariadb-104/).

### See Also

- [The features in MariaDB 10.4](/kb/en/what-is-mariadb-104/)
- [Upgrading from MariaDB 10.3 to MariaDB 10.4 with Galera Cluster](/replication/galera-cluster/upgrading-galera-cluster/upgrading-from-mariadb-103-to-mariadb-104-with-galera-cluster/)
- [Upgrading from MariaDB 10.2 to MariaDB 10.3](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-102-to-mariadb-103/)
- [Upgrading from MariaDB 10.1 to MariaDB 10.2](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-101-to-mariadb-102/)
- [Upgrading from MariaDB 10.0 to MariaDB 10.1](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-100-to-mariadb-101/)