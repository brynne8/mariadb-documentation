# Upgrading from MariaDB 10.2 to MariaDB 10.3 with Galera Cluster

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

Since [MariaDB 10.1](/kb/en/what-is-mariadb-101/), the [MySQL-wsrep](https://github.com/codership/mysql-wsrep) patch has been merged into MariaDB Server. Therefore, in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and above, the functionality of MariaDB Galera Cluster can be obtained by installing the standard MariaDB Server packages and the Galera wsrep provider library package.

Beginning in [MariaDB 10.1](/kb/en/what-is-mariadb-101/), [Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster) ships with the MariaDB Server.  Upgrading a Galera Cluster node is very similar to upgrading a server from [MariaDB 10.2](/kb/en/what-is-mariadb-102/) to [MariaDB 10.3](/kb/en/what-is-mariadb-103/).  For more information on that process as well as incompatibilities between versions, see the [Upgrade Guide](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-102-to-mariadb-103).

## Performing a Rolling Upgrade

The following steps can be used to perform a rolling upgrade from [MariaDB 10.2](/kb/en/what-is-mariadb-102/) to [MariaDB 10.3](/kb/en/what-is-mariadb-103/) when using Galera Cluster. In a rolling upgrade, each node is upgraded individually, so the cluster is always operational. There is no downtime from the application's perspective.

First, before you get started:

1 First, take a look at [Upgrading from MariaDB 10.2 to MariaDB 10.3](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-102-to-mariadb-103) to see what has changed between the major versions.
<ol start="1"><li>Check whether any system variables or options have been changed or removed. Make sure that your server's configuration is compatible with the new MariaDB version before upgrading.
</li><li>Check whether replication has changed in the new MariaDB version in any way that could cause issues while the cluster contains upgraded and non-upgraded nodes.
</li><li>Check whether any new features have been added to the new MariaDB version. If a new feature in the new MariaDB version cannot be replicated to the old MariaDB version, then do not use that feature until all cluster nodes have been upgrades to the new MariaDB version.
</li></ol>
2 Next, make sure that the Galera version numbers are compatible.
<ol start="1"><li>If you are upgrading from the most recent [MariaDB 10.2](/kb/en/what-is-mariadb-102/) release to [MariaDB 10.3](/kb/en/what-is-mariadb-103/), then the versions will be compatible. Both [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.3](/kb/en/what-is-mariadb-103/) use Galera 3 (i.e. Galera wsrep provider versions 25.3.x), so they should be compatible.
</li><li>See [What is MariaDB Galera Cluster?: Galera wsrep provider Versions](/kb/en/what-is-mariadb-galera-cluster/#galera-wsrep-provider-versions) for information on which MariaDB releases uses which Galera wsrep provider versions.
</li></ol>
3 Ideally, you want to have a large enough gcache to avoid a [State Snapshot Transfer (SST)](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts) during the rolling upgrade. The gcache size can be configured by setting <a undefined>gcache.size</a>  For example: <br>
<code class="fixed" style="white-space:pre-wrap">wsrep_provider_options="gcache.size=2G"</code>

Before you upgrade, it would be best to take a backup of your database. This is always a good idea to do before an upgrade. We would recommend [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup).

Then, for each node, perform the following steps:

1 Modify the repository configuration, so the system's package manager installs [MariaDB 10.3](/kb/en/what-is-mariadb-103/). For example,
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, see [Updating the MariaDB APT repository to a New Major Release](/kb/en/installing-mariadb-deb-files/#updating-the-mariadb-apt-repository-to-a-new-major-release) for more information.
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, see [Updating the MariaDB YUM repository to a New Major Release](/kb/en/yum/#updating-the-mariadb-yum-repository-to-a-new-major-release) for more information.
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, see [Updating the MariaDB ZYpp repository to a New Major Release](/kb/en/installing-mariadb-with-zypper/#updating-the-mariadb-zypp-repository-to-a-new-major-release) for more information.
</li></ul>
2 If you use a load balancing proxy such as MaxScale or HAProxy, make sure to drain the server from the pool so it does not receive any new connections.
3 [Stop MariaDB](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).
4 Uninstall the old version of MariaDB and the Galera wsrep provider.
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, execute the following: <br>
<code class="fixed" style="white-space:pre-wrap">sudo apt-get remove mariadb-server galera</code>
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, execute the following: <br>
<code class="fixed" style="white-space:pre-wrap">sudo yum remove MariaDB-server galera</code>
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, execute the following: <br>
<code class="fixed" style="white-space:pre-wrap">sudo zypper remove MariaDB-server galera</code>
</li></ul>
5 Install the new version of MariaDB and the Galera wsrep provider.
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, see [Installing MariaDB Packages with APT](/kb/en/installing-mariadb-deb-files/#installing-mariadb-packages-with-apt) for more information.
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, see [Installing MariaDB Packages with YUM](/kb/en/yum/#installing-mariadb-packages-with-yum) for more information.
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, see [Installing MariaDB Packages with ZYpp](/kb/en/installing-mariadb-with-zypper/#installing-mariadb-packages-with-zypp) for more information.
</li></ul>
6 Make any desired changes to configuration options in [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), such as `my.cnf`. This includes removing any system variables or options that are no longer supported.
7 On Linux distributions that use `systemd` you may need to increase the service startup timeout as the default timeout of 90 seconds may not be sufficient. See [Systemd: Configuring the Systemd Service Timeout](/kb/en/systemd/#configuring-the-systemd-service-timeout) for more information.
8 [Start MariaDB](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).
9 Run [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade) with the `--skip-write-binlog` option.
<ul start="1"><li>`mysql_upgrade` does two things:
<ol start="1"><li>Ensures that the system tables in the [mysq](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables)l database are fully compatible with the new version.
</li><li>Does a very quick check of all tables and marks them as compatible with the new version of MariaDB .
</li></ol>
</li></ul>

When this process is done for one node, move onto the next node.

Note that when upgrading the Galera wsrep provider, sometimes the Galera protocol version can change. The Galera wsrep provider should not start using the new protocol version until all cluster nodes have been upgraded to the new version, so this is not generally an issue during a rolling upgrade. However, this can cause issues if you restart a non-upgraded node in a cluster where the rest of the nodes have been upgraded.