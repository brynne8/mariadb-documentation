# Introduction to State Snapshot Transfers (SSTs)

In a State Snapshot Transfer (SST), the cluster provisions nodes by transferring a full data copy from one node to another. When a new node joins the cluster, the new node initiates a State Snapshot Transfer to synchronize its data with a node that is already part of the cluster.

## Types of SSTs

There are two conceptually different ways to transfer a state from one MariaDB server to another:

1 <strong>Logical</strong>

The only SST method of this type is the `mysqldump` SST method, which actually uses the [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) utility to get a logical dump of the donor. This SST method requires the joiner node to be fully initialized and ready to accept connections before the transfer. This method is, by definition, blocking, in that it blocks the donor node from modifying its own state for the duration of the transfer. It is also the slowest of all, and that might be an issue in a cluster with a lot of load.

1 <strong>Physical</strong>

SST methods of this type physically copy the data files from the donor node to the joiner node. This requires that the joiner node is initialized after the transfer. The [mariabackup](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/mariabackup-sst-method/) SST method and other a few other SST methods fall into this category. These SST methods are much faster than the `mysqldump` SST method, but they have certain limitations. For example, they can be used only on server startup and the joiner node must be configured very similarly to the donor node (e.g. [innodb_file_per_table](/kb/en/xtradbinnodb-server-system-variables/#innodb_file_per_table) should be the same and so on). Some of the SST methods in this category are non-blocking on the donor node, meaning that the donor node is still able to process queries while donating the SST (e.g. the [mariabackup](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/mariabackup-sst-method/) SST method is non-blocking).

## SST Methods

SST methods are supported via a scriptable interface. New SST methods could potentially be developed by creating new SST scripts. The scripts usually have names of the form `wsrep_sst_&lt;method&gt;` where `&lt;method&gt;` is one of the SST methods listed below.

You can choose your SST method by setting the <a undefined>wsrep_sst_method</a> system variable. It can be changed dynamically with <a undefined>SET GLOBAL</a> on the node that you intend to be a SST donor. For example:

```sql
SET GLOBAL wsrep_sst_method='mariabackup';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up a node:

```sql
[mariadb]
...
wsrep_sst_method = mariabackup
```

For an SST to work properly, the donor and joiner node must use the same SST method. Therefore, it is recommended to set <a undefined>wsrep_sst_method</a> to the same value on all nodes, since any node will usually be a donor or joiner node at some point.

MariaDB Galera Cluster comes with the following built-in SST methods:

### mariabackup

This SST method uses the [Mariabackup](/kb/en/backup-restore-and-import-clients-mariadb-backup/) utility for performing SSTs. It is one of the two non-locking methods. This is the recommended SST method if you require the ability to run queries on the donor node during the SST. Note that if you use the `mariabackup` SST method, then you also need to have `socat` installed on the server. This is needed to stream the backup from the donor to the joiner. This is a limitation inherited from the `xtrabackup-v2` SST method.

This SST method supports [GTID](/replication/standard-replication/gtid/).

This SST method supports [Data at Rest Encryption](/kb/en/data-at-rest-encryption/).

This SST method is available from [MariaDB 10.1.26](/kb/en/mariadb-10126-release-notes/) and [MariaDB 10.2.10](/kb/en/mariadb-10210-release-notes/).

See [mariabackup SST method](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/mariabackup-sst-method/) for more information.

### rsync

This is the default method. This method uses the <a undefined>rsync</a> utility to create a snapshot of the donor node. `rsync` should be available by default on all modern Linux distributions. The donor node is blocked with a read lock during the SST. This is the fastest SST method, especially for large datasets since it copies binary data. Because of that, this is the recommended SST method if you do not need to allow the donor node to execute queries during the SST.

This SST method supports [GTID](/replication/standard-replication/gtid/).

This SST method supports [Data at Rest Encryption](/kb/en/data-at-rest-encryption/).

As of [MariaDB 10.1.36](/kb/en/mariadb-10136-release-notes/), [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/), and [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), <a undefined>stunnel</a> can be used to encrypt data over the wire. Be sure to have `stunnel` installed. You will also need to generate certificates and keys. See [the stunnel documentation](https://www.stunnel.org/howto.html) for information on how to do that. Once you have the keys, you will need to add the `tkey` and `tcert` options to the `[sst]` option group in your MariaDB configuration file, such as:

```sql
[sst]
tkey = /etc/my.cnf.d/certificates/client-key.pem
tcert = /etc/my.cnf.d/certificates/client-cert.pem
```

You also need to run the certificate directory through <a undefined>openssl rehash</a>.

### mysqldump

This SST method runs [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) on the donor node and pipes
the output to the [mysql](/clients-utilities/mysql-client/) client connected to the joiner node. The `mysqldump` SST method
needs a username/password pair set in the [wsrep_sst_auth](/kb/en/galera-cluster-system-variables/#wsrep_sst_auth) variable in order to get the dump. The donor node is blocked with a read lock during the SST. This is the slowest SST method.

This SST method supports [GTID](/replication/standard-replication/gtid/).

This SST method supports [Data at Rest Encryption](/kb/en/data-at-rest-encryption/).

### xtrabackup-v2

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/) is the recommended backup method to use instead of Percona XtraBackup.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/), Percona XtraBackup is <strong>not supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), Percona XtraBackup is only <strong>partially supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

This SST method uses the [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/) utility for performing SSTs. It is one of the two non-blocking methods. Note that if you use the `xtrabackup-v2` SST method, you also need to have `socat` installed on the server. Since Percona XtraBackup is a third party product, this SST method requires an additional installation some additional configuration. Please refer to [Percona's xtrabackup SST documentation](http://www.percona.com/doc/percona-xtradb-cluster/5.7/manual/xtrabackup_sst.html) for information from the vendor.

This SST method does <strong>not</strong> support [GTID](/replication/standard-replication/gtid/).

This SST method does <strong>not</strong> support [Data at Rest Encryption](/kb/en/data-at-rest-encryption/).

This SST method is available from MariaDB Galera Cluster 5.5.37 and MariaDB Galera Cluster 10.0.10.

See [xtrabackup-v2 SST method](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/xtrabackup-v2-sst-method/) for more information.

### xtrabackup

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/) is the recommended backup method to use instead of Percona XtraBackup.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/), Percona XtraBackup is <strong>not supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), Percona XtraBackup is only <strong>partially supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

This SST method is an older SST method that uses the [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/) utility for performing SSTs. The `xtrabackup-v2` SST method should be used instead of the `xtrabackup` SST method starting from [MariaDB 5.5.33](/kb/en/mariadb-5533-release-notes/).

This SST method does <strong>not</strong> support [GTID](/replication/standard-replication/gtid/).

This SST method does <strong>not</strong> support [Data at Rest Encryption](/kb/en/data-at-rest-encryption/).

## Authentication

All SST methods except `rsync` require authentication via username and password. You can tell the client what username and password to use by setting the <a undefined>wsrep_sst_auth</a> system variable. It can be changed dynamically with <a undefined>SET GLOBAL</a> on the node that you intend to be a SST donor. For example:

```sql
SET GLOBAL wsrep_sst_auth = 'mariabackup:password';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up a node:

```sql
[mariadb]
...
wsrep_sst_auth = mariabackup:password
```

Some [authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins/) do not require a password. For example, the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) and [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi/) authentication plugins do not require a password. If you are using a user account that does not require a password in order to log in, then you can just leave the password component of <a undefined>wsrep_sst_auth</a> empty. For example:

```sql
[mariadb]
...
wsrep_sst_auth = mariabackup:
```

See the relevant description or page for each SST method to find out what privileges need to be [granted](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) to the user and whether the privileges are needed on the donor node or joiner node for that method.

## SSTs and Systemd

MariaDB's [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) unit file has a default startup timeout of about 90 seconds on most systems. If an SST takes longer than this default startup timeout on a joiner node, then `systemd` will assume that `mysqld` has failed to startup, which causes `systemd` to kill the `mysqld` process on the joiner node. To work around this, you can reconfigure the MariaDB `systemd` unit to have an infinite timeout, such as by executing one of the following commands:

If you are using `systemd` 228 or older, then you can execute the following to set an infinite timeout:

```sql
sudo tee /etc/systemd/system/mariadb.service.d/timeoutstartsec.conf <<EOF
[Service]

TimeoutStartSec=0
EOF
sudo systemctl daemon-reload
```

[Systemd 229 added the infinity option](https://lists.freedesktop.org/archives/systemd-devel/2016-February/035748.html), so if you are using `systemd` 229 or later, then you can execute the following to set an infinite timeout:

```sql
sudo tee /etc/systemd/system/mariadb.service.d/timeoutstartsec.conf <<EOF
[Service]

TimeoutStartSec=infinity
EOF
sudo systemctl daemon-reload
```

See [Configuring the Systemd Service Timeout](/kb/en/systemd/#configuring-the-systemd-service-timeout) for more details.

Note that [systemd 236 added the `EXTEND_TIMEOUT_USEC` environment variable](https://lists.freedesktop.org/archives/systemd-devel/2017-December/039996.html) that allows services to extend the startup timeout during long-running processes. Starting with [MariaDB 10.1.35](/kb/en/mariadb-10135-release-notes/), [MariaDB 10.2.17](/kb/en/mariadb-10217-release-notes/), and [MariaDB 10.3.8](/kb/en/mariadb-1038-release-notes/), on systems with systemd versions that support it, MariaDB uses this feature to extend the startup timeout during long SSTs. Therefore, if you are using `systemd` 236 or later, then you should not need to manually override `TimeoutStartSec`, even if your SSTs run for longer than the configured value. See [MDEV-15607](https://jira.mariadb.org/browse/MDEV-15607) for more information.

## SST Failure

An SST failure generally renders the joiner node unusable. Therefore, when an SST failure is detected, the joiner node will abort.

Restarting a node after a `mysqldump` SST failure may require manual restoration of the administrative tables.

## SSTs and Data at Rest Encryption

Look at the description of each SST method to determine which methods support [Data at Rest Encryption](/kb/en/data-at-rest-encryption/).

For logical SST methods like `mysqldump`, each node should be able to have different [encryption keys](/kb/en/data-at-rest-encryption/#encryption-key-management). For physical SST methods, all nodes need to have the same [encryption keys](/kb/en/data-at-rest-encryption/#encryption-key-management), since the donor node will copy encrypted data files to the joiner node, and the joiner node will need to be able to decrypt them.

## Minimal Cluster Size

In order to avoid a split-brain condition, the minimum recommended number of nodes in a cluster is 3.

When using an SST method that blocks the donor, there is yet another reason to require a minimum of 3 nodes. In a 3-node cluster, if one node is acting as an SST joiner and one other node is acting as an SST donor, then there is still one more node to continue executing queries.

## Manual SSTs

In some cases, if Galera Cluster's automatic SSTs repeatedly fail, then it can be helpful to perform a "manual SST". See the following pages on how to do that:

- [Manual SST of Galera Cluster node with Mariabackup](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/manual-sst-of-galera-cluster-node-with-mariabackup/)
- [Manual SST of Galera Cluster node with Percona XtraBackup](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/manual-sst-of-galera-cluster-node-with-percona-xtrabackup/)

## Known Issues

### mysqld_multi

SST scripts can't currently read the `[mysqldN]` [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) in [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) that are read by instances managed by [mysqld_multi](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_multi/).

See [MDEV-18863](https://jira.mariadb.org/browse/MDEV-18863) for more information.

## See Also

- [Galera Cluster documentation: STATE SNAPSHOT TRANSFERS](https://galeracluster.com/library/documentation/sst.html)