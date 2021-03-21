# Upgrading from MariaDB Galera Cluster 10.0 to MariaDB 10.1 with Galera Cluster

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

Since [MariaDB 10.1](/kb/en/what-is-mariadb-101/), the [MySQL-wsrep](https://github.com/codership/mysql-wsrep) patch has been merged into MariaDB Server. Therefore, in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and above, the functionality of MariaDB Galera Cluster can be obtained by installing the standard MariaDB Server packages and the Galera wsrep provider library package.

## Performing a Rolling Upgrade

The following steps can be used to perform a rolling upgrade from MariaDB Galera Cluster 10.0 to [MariaDB 10.1](/kb/en/what-is-mariadb-101/). In a rolling upgrade, each node is upgraded individually, so the cluster is always operational. There is no downtime from the application's perspective.

First, before you get started:

1 First, take a look at [Upgrading from MariaDB 10.0 to MariaDB 10.1](/kb/en/upgrading-from-mariadb-100-to-101/) to see what has changed between the major versions.
<ol start="1"><li>Check whether any system variables or options have been changed or removed. Make sure that your server's configuration is compatible with the new MariaDB version before upgrading.
</li><li>Check whether replication has changed in the new MariaDB version in any way that could cause issues while the cluster contains upgraded and non-upgraded nodes.
</li><li>Check whether any new features have been added to the new MariaDB version. If a new feature in the new MariaDB version cannot be replicated to the old MariaDB version, then do not use that feature until all cluster nodes have been upgrades to the new MariaDB version.
</li></ol>
2 Next, make sure that the Galera version numbers are compatible.
<ol start="1"><li>If you are upgrading from the most recent MariaDB Galera Cluster 10.0 release to [MariaDB 10.1](/kb/en/what-is-mariadb-101/), then the versions will be compatible. The latest releases of MariaDB Galera Cluster 10.0 and all releases of [MariaDB 10.1](/kb/en/what-is-mariadb-101/) use Galera 3 (i.e. Galera wsrep provider versions 25.3.x), so they should be compatible.
</li><li>If you are running an older MariaDB Galera Cluster 10.0 release that still uses Galera 2 (i.e. Galera wsrep provider versions 25.2.x), then it is recommended to first upgrade to the latest MariaDB Galera Cluster 10.0 release that uses Galera 3 (i.e. Galera wsrep provider versions 25.3.x).
</li><li>See [What is MariaDB Galera Cluster?: Galera wsrep provider Versions](/kb/en/what-is-mariadb-galera-cluster/#galera-wsrep-provider-versions) for information on which MariaDB releases uses which Galera wsrep provider versions.
</li></ol>
3 Ideally, you want to have a large enough gcache to avoid a [State Snapshot Transfer (SST)](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts/) during the rolling upgrade. The gcache size can be configured by setting <a undefined>gcache.size</a>  For example: <br>
<code class="fixed" style="white-space:pre-wrap">wsrep_provider_options="gcache.size=2G"</code>

Before you upgrade, it would be best to take a backup of your database. This is always a good idea to do before an upgrade. We would recommend [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/).

Then, for each node, perform the following steps:

1 Modify the repository configuration, so the system's package manager installs [MariaDB 10.1](/kb/en/what-is-mariadb-101/). For example,
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, see [Updating the MariaDB APT repository to a New Major Release](/kb/en/installing-mariadb-deb-files/#updating-the-mariadb-apt-repository-to-a-new-major-release) for more information.
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, see [Updating the MariaDB YUM repository to a New Major Release](/kb/en/yum/#updating-the-mariadb-yum-repository-to-a-new-major-release) for more information.
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, see [Updating the MariaDB ZYpp repository to a New Major Release](/kb/en/installing-mariadb-with-zypper/#updating-the-mariadb-zypp-repository-to-a-new-major-release) for more information.
</li></ul>
2 If you use a load balancing proxy such as MaxScale or HAProxy, make sure to drain the server from the pool so it does not receive any new connections.
3 Set <a undefined>innodb_fast_shutdown</a> to `0`. It can be changed dynamically with <a undefined>SET GLOBAL</a>. For example: <br>
<code class="fixed" style="white-space:pre-wrap">SET GLOBAL innodb_fast_shutdown=0;</code>
4 [Stop MariaDB](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).
5 Uninstall the old version of MariaDB and the Galera wsrep provider.
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, execute the following: <br>
<code class="fixed" style="white-space:pre-wrap">sudo apt-get remove mariadb-galera-server galera</code>
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, execute the following: <br>
<code class="fixed" style="white-space:pre-wrap">sudo yum remove MariaDB-Galera-server galera</code>
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, execute the following: <br>
<code class="fixed" style="white-space:pre-wrap">sudo zypper remove MariaDB-Galera-server galera</code>
</li></ul>
6 Install the new version of MariaDB and the Galera wsrep provider.
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, see [Installing MariaDB Packages with APT](/kb/en/installing-mariadb-deb-files/#installing-mariadb-packages-with-apt) for more information.
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, see [Installing MariaDB Packages with YUM](/kb/en/yum/#installing-mariadb-packages-with-yum) for more information.
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, see [Installing MariaDB Packages with ZYpp](/kb/en/installing-mariadb-with-zypper/#installing-mariadb-packages-with-zypp) for more information.
</li></ul>
7 Make any desired changes to configuration options in [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), such as `my.cnf`. This includes removing any system variables or options that are no longer supported.
<ul start="1"><li>It is important to note that a new option is needed for Galera to be active in [MariaDB 10.1](/kb/en/what-is-mariadb-101/): you must set <a undefined>wsrep_on=ON</a>.
</li></ul>
8 On Linux distributions that use `systemd` you may need to increase the service startup timeout as the default timeout of 90 seconds may not be sufficient. See [Systemd: Configuring the Systemd Service Timeout](/kb/en/systemd/#configuring-the-systemd-service-timeout) for more information.
9 [Start MariaDB](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).
10 Run [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade/) with the `--skip-write-binlog` option.
<ul start="1"><li>`mysql_upgrade` does two things:
<ol start="1"><li>Ensures that the system tables in the [mysq](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/)l database are fully compatible with the new version.
</li><li>Does a very quick check of all tables and marks them as compatible with the new version of MariaDB .
</li></ol>
</li></ul>

When this process is done for one node, move onto the next node.

Note that when upgrading the Galera wsrep provider, sometimes the Galera protocol version can change. The Galera wsrep provider should not start using the new protocol version until all cluster nodes have been upgraded to the new version, so this is not generally an issue during a rolling upgrade. However, this can cause issues if you restart a non-upgraded node in a cluster where the rest of the nodes have been upgraded.