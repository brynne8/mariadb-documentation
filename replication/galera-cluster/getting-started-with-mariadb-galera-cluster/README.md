# Getting Started with MariaDB Galera Cluster

The most recent release of [MariaDB 10.5](/kb/en/what-is-mariadb-105/) is:<br>
<span class="cstm-style lead"><strong>[MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/)</strong>  Stable (GA) [Download<span>&nbsp;</span>Now](https://mariadb.com/downloads/)</span><br>
<sub><em>[Alternate download from mariadb.org](https://downloads.mariadb.org/mariadb/10.5.9/)</em></sub>

<span class="cstm-style lead">The current [versions](/kb/en/what-is-mariadb-galera-cluster/#galera-versions) of the Galera wsrep provider library are 26.4.7 for [Galera](/kb/en/galera/) 4 and 25.3.32 for [Galera](/kb/en/galera/) 3.</span><br>
<em>For convenience, packages containing these libraries are included in the MariaDB [YUM and APT repositories](https://downloads.mariadb.org/mariadb/repositories/).</em>

Currently, MariaDB Galera Cluster only supports the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) storage engine.

A great resource for Galera users is the mailing list run by the developers at Codership. It can be found at [Codership on Google Groups](https://groups.google.com/forum/?fromgroups#!forum/codership-team). If you use Galera, then it is recommended you subscribe.

## Galera Cluster Support in MariaDB Server

MariaDB Galera Cluster is powered by:

- MariaDB Server.
- The [MySQL-wsrep](https://github.com/codership/mysql-wsrep) patch for MySQL Server and MariaDB Server developed by [Codership](http://www.codership.com). The patch currently supports only Unix-like operating systems.
- The [Galera wsrep provider library](https://github.com/codership/galera/).

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, the [MySQL-wsrep](https://github.com/codership/mysql-wsrep) patch has been merged into MariaDB Server. This means that the functionality of MariaDB Galera Cluster can be obtained by installing the standard MariaDB Server packages and the [Galera wsrep provider library](https://github.com/codership/galera/) package. The following [Galera](/kb/en/galera/) version corresponds to each MariaDB Server version:

- In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, MariaDB Galera Cluster uses [Galera](/kb/en/galera/) 4. This means that the [MySQL-wsrep](https://github.com/codership/mysql-wsrep) patch is version 26 and the [Galera wsrep provider library](https://github.com/codership/galera/) is version 4.
- In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, MariaDB Galera Cluster uses [Galera](/kb/en/galera/) 3. This means that the [MySQL-wsrep](https://github.com/codership/mysql-wsrep) patch is version 25 and the [Galera wsrep provider library](https://github.com/codership/galera/) is version 3.

See [Deciphering Galera Version Numbers](https://mariadb.com/resources/blog/deciphering-galera-version-numbers/) for more information about how to interpret these version numbers.

See [What is MariaDB Galera Cluster?: Galera Versions](/kb/en/what-is-mariadb-galera-cluster/#galera-versions) for more information about which specific [Galera](/kb/en/galera/) version is included in each release of MariaDB Server.

In supported builds, Galera Cluster functionality can be enabled by setting some configuration options that are mentioned below. Galera Cluster functionality is not enabled in a standard MariaDB Server installation unless explicitly enabled with these configuration options.

## Prerequisites

### Swap Size Requirements

During normal operation a MariaDB Galera node does not consume much more memory
than a regular MariaDB server. Additional memory is consumed for the
certification index and uncommitted writesets, but normally this should not be
noticeable in a typical application. There is one exception though:

1 <strong>Writeset caching during state transfer.</strong> When a node is receiving a state
  transfer it cannot process and apply incoming writesets because it has no
  state to apply them to yet. Depending on a state transfer mechanism (e.g.
  [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/)) the node that sends the state transfer may not be able to
  apply writesets as well. Thus they need to cache those writesets for a
  catch-up phase. Currently the writesets are cached in memory and, if the
  system runs out of memory either the state transfer will fail or the cluster
  will block waiting for the state transfer to end.

To control memory usage for writeset caching, check the [Galera parameters](https://galeracluster.com/library/documentation/galera-parameters.html): `gcs.recv_q_hard_limit`, `gcs.recv_q_soft_limit`, and `gcs.max_throttle`.

### Limitations

Before using MariaDB Galera Cluster, we would recommend reading through the [known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations/), so you can be sure that it is appropriate for your application.

## Installing MariaDB Galera Cluster

To use MariaDB Galera Cluster, there are two primary packages that you need to install:

1 A MariaDB Server version that supports Galera Cluster.

1 The Galera wsrep provider library.

As mentioned in the previous section, in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and above, Galera Cluster support is actually included in the standard MariaDB Server packages. That means that installing MariaDB Galera Cluster package is the same as installing standard MariaDB Server package in those versions. However, you will also have to install an additional package to obtain the Galera wsrep provider library.

Some [SST](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts/) methods may also require additional packages to be installed. The [mariabackup](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/mariabackup-sst-method/) SST method is generally the best option for large clusters that expect a lot of load.

### Installing MariaDB Galera Cluster with a Package Manager

MariaDB Galera Cluster can be installed via a package manager on Linux. In order to do so, your system needs to be configured to install from one of the MariaDB repositories.

You can configure your package manager to install it from MariaDB Corporation's MariaDB Package Repository by using the [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage/).

You can also configure your package manager to install it from MariaDB Foundation's MariaDB Repository by using the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/).

#### Installing MariaDB Galera Cluster with yum/dnf

On RHEL, CentOS, Fedora, and other similar Linux distributions, it is highly recommended to install the relevant [RPM packages](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/) from MariaDB's
repository using [yum](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum/) or <a undefined>dnf</a>. Starting with RHEL 8 and Fedora 22, `yum` has been replaced by `dnf`, which is the next major version of `yum`. However, `yum` commands still work on many systems that use `dnf`.

To install MariaDB Galera Cluster with `yum` or `dnf`, follow the instructions at [Installing MariaDB Galera Cluster with yum](/kb/en/installing-mariadb-with-yum/#installing-mariadb-galera-cluster-with-yum).

#### Installing MariaDB Galera Cluster with apt-get

On Debian, Ubuntu, and other similar Linux distributions, it is highly recommended to install the relevant [DEB packages](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files/) from MariaDB's
repository using <a undefined>apt-get</a>.

To install MariaDB Galera Cluster with `apt-get`, follow the instructions at [Installing MariaDB Galera Cluster with apt-get](/kb/en/installing-mariadb-deb-files/#installing-mariadb-galera-cluster-with-apt).

#### Installing MariaDB Galera Cluster with zypper

On SLES, OpenSUSE, and other similar Linux distributions, it is highly recommended to install the relevant [RPM packages](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/) from MariaDB's repository using [zypper](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/installing-mariadb-with-zypper/).

To install MariaDB Galera Cluster with `zypper`, follow the instructions at [Installing MariaDB Galera Cluster with ZYpp](/kb/en/installing-mariadb-with-zypper/#installing-mariadb-galera-cluster-with-zypp).

### Installing MariaDB Galera Cluster with a Binary Tarball

To install MariaDB Galera Cluster with a binary tarball, follow the instructions at [Installing MariaDB Binary Tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs/).

##### MariaDB Galera Cluster starting with 10.0.24

To make the location of the `libgalera_smm.so` library in binary tarballs more similar to its location in other packages, the library is now found at `lib/galera/libgalera_smm.so` in the binary tarballs, and there is a symbolic link in the `lib` directory that points to it.

### Installing MariaDB Galera Cluster from Source

To install MariaDB Galera Cluster by compiling it from source, you will have to compile both MariaDB Server and the Galera wsrep provider library. For some information on how to do this, see the pages at [Installing Galera From Source](/replication/galera-cluster/installing-galera-from-source/). The pages at [Compiling MariaDB From Source](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/) and [Galera Cluster Documentation: Building Galera Cluster for MySQL](https://galeracluster.com/library/documentation/install-mysql-src.html#building-galera-cluster-for-mysql) may also be helpful. When compiling [MariaDB 10.1](/kb/en/what-is-mariadb-101/) or earlier and you want to enable Galera Cluster support, be sure to set set `-DWITH_WSREP=ON` and `-DWITH_INNODB_DISALLOW_WRITES=ON` when running [cmake](/kb/en/generic-build-instructions/#using-cmake). When compiling [MariaDB 10.2](/kb/en/what-is-mariadb-102/) or later, it is enabled by default.

## Configuring MariaDB Galera Cluster

A number of options need to be set in order for Galera Cluster to work when using MariaDB. See [Configuring MariaDB Galera Cluster](/replication/galera-cluster/configuring-mariadb-galera-cluster/) for more information.

## Bootstrapping a New Cluster

To first node of a new cluster needs to be bootstrapped by starting [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) on that node with the option <a undefined>--wsrep-new-cluster</a> option. This option tells the node that there is no existing cluster to connect to. The node will create a new UUID to identify the new cluster.

Do not use the <a undefined>--wsrep-new-cluster</a> option when connecting to an existing cluster. Restarting the node with this option set will cause the node to create new UUID to identify the cluster again, and the node won't reconnect to the old cluster. See the next section about how to reconnect to an existing cluster.

For example, if you were manually starting [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) on a node, then you could bootstrap it by executing the following:

```sql
$ mysqld --wsrep-new-cluster
```

However, keep in mind that most users are not going to be starting [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) manually. Instead, most users will use a [service manager](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/) to start [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/). See the following sections on how to bootstrap a node with the most common service managers.

### Systemd and Bootstrapping

On operating systems that use [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/), a node can be bootstrapped in the following way:

```sql
$ galera_new_cluster
```

This wrapper uses [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) to run [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) with the <a undefined>--wsrep-new-cluster</a> option.

If you are using the [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) service that supports the [systemd service's method for interacting with multiple MariaDB Server processes](/kb/en/systemd/#interacting-with-multiple-mariadb-server-processes), then you can bootstrap a specific instance by specifying the instance name as a suffix. For example:

```sql
$ galera_new_cluster mariadb@node1
```

##### MariaDB starting with [10.1.8](/kb/en/mariadb-1018-release-notes/)

Systemd support and the galera_new_cluster script were added in [MariaDB 10.1](/kb/en/what-is-mariadb-101/).

### SysVinit and Bootstrapping

On operating systems that use [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit/), a node can be bootstrapped in the following way:

```sql
$ service mysql bootstrap
```

This runs [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) with the <a undefined>--wsrep-new-cluster</a> option.

## Adding Another Node to a Cluster

Once you have a cluster running and you want to add/reconnect another node to it, you must supply an address of one or more of the existing cluster members in the <a undefined>wsrep_cluster_address</a> option. For example, if the first node of the cluster has the address 192.168.0.1, then you could add a second node to the cluster by setting the following option in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

```sql
[mariadb]
...
wsrep_cluster_address=gcomm://192.168.0.1  # DNS names work as well, IP is preferred for performance
```

The new node only needs to connect to one of the existing cluster nodes. Once it connects to one of the existing cluster nodes, it will be able to see all of the nodes in the cluster. However, it is generally better to list all nodes of the cluster in <a undefined>wsrep_cluster_address</a>, so that any node can join a cluster by connecting to any of the other cluster nodes, even if one or more of the cluster nodes are down. It is even OK to list a node's own IP address in <a undefined>wsrep_cluster_address</a>, since Galera Cluster is smart enough to ignore it.

Once all members agree on the membership, the cluster's state will be exchanged. If the new node's state is different from that of the cluster, then it will request an IST or [SST](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts/) to make itself consistent with the other nodes.

## Restarting the Cluster

If you shut down all nodes at the same time, then you have effectively terminated the cluster. Of course, the cluster's data still exists, but the running cluster no longer exists. When this happens, you'll need to bootstrap the cluster again.

If the cluster is not bootstrapped and [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) on the first node is just [started normally](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/), then the node willl try to connect to at least one of the nodes listed in the <a undefined>wsrep_cluster_address</a> option. If no nodes are currently running, then this will fail. Bootstrapping the first node solves this problem.

### Determining the Most Advanced Node

In some cases Galera will refuse to bootstrap a node if it detects that it might not be the most advanced node in the cluster. Galera makes this determination if the node was not the last one in the cluster to be shut down or if the node crashed. In those cases, manual intervention is needed.

If you know for sure which node is the most advanced you can edit the `grastate.dat` file in the <a undefined>datadir</a>. You can set `safe_to_bootstrap=1` on the most advanced node.

You can determine which node is the most advanced by checking `grastate.dat` on each node and looking for the node with the highest `seqno`. If the node crashed and `seqno=-1`, then you can find the most advanced node by recovering the `seqno` on each node with the <a undefined>wsrep_recover</a> option. For example:

```sql
$ mysqld --wsrep_recover
```

#### Systemd and Galera Recovery

On operating systems that use [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/), the position of a node can be recovered by running the `galera_recovery` script. For example:

```sql
$ galera_recovery
```

If you are using the [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) service that supports the [systemd service's method for interacting with multiple MariaDB Server processes](/kb/en/systemd/#interacting-with-multiple-mariadb-server-processes), then you can recover the position of a specific instance by specifying the instance name as a suffix. For example:

```sql
$ galera_recovery mariadb@node1
```

The `galera_recovery` script recovers the position of a node by running [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) with the <a undefined>wsrep_recover</a> option.

When the `galera_recovery` script runs [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/), it does not write to the [error log](/mariadb-administration/server-monitoring-logs/error-log/). Instead, it redirects [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) log output to a file named with the format `/tmp/wsrep_recovery.XXXXXX`, where `XXXXXX` is replaced with random characters.

When Galera is enabled, MariaDB's [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) service automatically runs the `galera_recovery` script prior to starting MariaDB, so that MariaDB starts with the proper Galera position.

##### MariaDB starting with [10.1.8](/kb/en/mariadb-1018-release-notes/)

Support for [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) and the `galera_recovery` script were added in [MariaDB 10.1](/kb/en/what-is-mariadb-101/).

## State Snapshot Transfers (SSTs)

In a State Snapshot Transfer (SST), the cluster provisions nodes by transferring a full data copy from one node to another. When a new node joins the cluster, the new node initiates a State Snapshot Transfer to synchronize its data with a node that is already part of the cluster.

See [Introduction to State Snapshot Transfers (SSTs)](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts/) for more information.

## Incremental State Transfers (ISTs)

In an Incremental State Transfer (SST), the cluster provisions nodes by transferring a node's missing writesets from one node to another. When a new node joins the cluster, the new node initiates a Incremental State Transfer to synchronize its data with a node that is already part of the cluster.

If a node has only been out of a cluster for a little while, then an IST is generally faster than an SST.

## Data at Rest Encryption

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and above, MariaDB Galera Cluster supports [Data at Rest Encryption](/kb/en/data-at-rest-encryption/). See [SSTs and Data at Rest Encryption](/kb/en/introduction-to-state-snapshot-transfers-ssts/#ssts-and-data-at-rest-encryption) for some disclaimers on how SSTs are affected when encryption is configured.

Some data still cannot be encrypted:

- The disk-based [Galera gcache](https://galeracluster.com/library/documentation/state-transfer.html#write-set-cache-gcache) is not encrypted ([MDEV-8072](https://jira.mariadb.org/browse/MDEV-8072)).

## Monitoring

### Status Variables

[Galera Cluster's status variables](/kb/en/mariadb-galera-cluster-status-variables/) can be queried with the standard [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status/) command. For example:

```sql
SHOW GLOBAL STATUS LIKE 'wsrep_%';
```

### Cluster Change Notifications

The cluster nodes can be configured to invoke a command when cluster membership or node status changes. This mechanism can also be used to communicate the event to some external monitoring agent. This is configured by setting <a undefined>wsrep_notify_cmd</a>. See [Galera Cluster documentation: Notification Command](https://galeracluster.com/library/documentation/notification-cmd.html)  for more information.

## See Also

- [What is MariaDB Galera Cluster?](/replication/galera-cluster/what-is-mariadb-galera-cluster/)
- [About Galera Replication](/replication/galera-cluster/about-galera-replication/)
- [Galera Use Cases](/replication/galera-cluster/galera-use-cases/)
- [Codership on Google Groups](https://groups.google.com/forum/?fromgroups#!forum/codership-team)
- [Galera Cluster documentation](https://galeracluster.com/library/documentation/)
- [Galera Cluster documentation: Notification Command](http://galeracluster.com/documentation-webpages/notificationcmd.html)
- [Introducing the “Safe-To-Bootstrap” feature in Galera Cluster](http://galeracluster.com/2016/11/introducing-the-safe-to-bootstrap-feature-in-galera-cluster/)
- [Github - galera](https://github.com/codership/galera/)
- [Github - mysql-wsrep](https://github.com/codership/mysql-wsrep/)

## Footnotes

