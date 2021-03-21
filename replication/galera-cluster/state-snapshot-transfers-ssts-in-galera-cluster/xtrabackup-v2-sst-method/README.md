# xtrabackup-v2 SST Method

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/) is the recommended backup method to use instead of Percona XtraBackup.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/), Percona XtraBackup is <strong>not supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), Percona XtraBackup is only <strong>partially supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

The `xtrabackup-v2` SST method uses the [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/) utility for performing SSTs. It is one of the methods that does not block the donor node.

Note that if you use the `xtrabackup-v2` SST method, then you also need to have <a undefined>socat</a> installed on the server. This is needed to stream the backup from the donor node to the joiner node.

Since [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/) is a third party product, it may require additional installation and additional configuration. Please refer to [Percona's xtrabackup SST documentation](http://www.percona.com/doc/percona-xtradb-cluster/5.5/manual/xtrabackup_sst.html) for information from the vendor.

## Choosing Percona XtraBackup for SSTs

To use the `xtrabackup-v2` SST method, you must set the <a undefined>wsrep_sst_method=xtrabackup-v2</a> on both the donor and joiner node. It can be changed dynamically with <a undefined>SET GLOBAL</a> on the node that you intend to be a SST donor. For example:

```sql
SET GLOBAL wsrep_sst_method='xtrabackup-v2';
```

It can be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up a node:

```sql
[mariadb]
...
wsrep_sst_method = xtrabackup-v2
```

For an SST to work properly, the donor and joiner node must use the same SST method. Therefore, it is recommended to set <a undefined>wsrep_sst_method</a> to the same value on all nodes, since any node will usually be a donor or joiner node at some point.

## Authentication and Privileges

To use the `xtrabackup-v2` SST method, [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/) needs to be able to authenticate locally on the donor node, so that it can create a backup to stream to the joiner. You can tell the donor node what username and password to use by setting the <a undefined>wsrep_sst_auth</a> system variable. It can be changed dynamically with <a undefined>SET GLOBAL</a> on the node that you intend to be a SST donor. For example:

```sql
SET GLOBAL wsrep_sst_auth = 'xtrabackup:mypassword';
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up a node:

```sql
[mariadb]
...
wsrep_sst_auth = xtrabackup:mypassword
```

Some [authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins/) do not require a password. For example, the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) and [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi/) authentication plugins do not require a password. If you are using a user account that does not require a password in order to log in, then you can just leave the password component of <a undefined>wsrep_sst_auth</a> empty. For example:

```sql
[mariadb]
...
wsrep_sst_auth = xtrabackup:
```

The user account that performs the backup for the SST needs to have [the same privileges as Percona XtraBackup](/kb/en/percona-xtrabackup-overview/#authentication-and-privileges), which are the `RELOAD` , `PROCESS`, `LOCK TABLES` and `REPLICATION CLIENT` [global privileges](/kb/en/grant/#global-privileges). To be safe, you should ensure that these privileges are set on each node in your cluster.  [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/) connects locally on the donor node to perform the backup, so the following user should be sufficient:

```sql
CREATE USER 'xtrabackup'@'localhost' IDENTIFIED BY 'mypassword';
GRANT RELOAD, PROCESS, LOCK TABLES, REPLICATION CLIENT ON *.* TO 'xtrabackup'@'localhost';
```

### Passwordless Authentication - Unix Socket

It is possible to use the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) authentication plugin for the user account that performs SSTs. This would provide the benefit of not needing to configure a plain-text password in <a undefined>wsrep_sst_auth</a>.

The user account would have to have the same name as the operating system user account that is running the `mysqld` process. On many systems, this is the user account configured as the <a undefined>user</a> option, and it tends to default to `mysql`.

For example, if the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) authentication plugin is already installed, then you could execute the following to create the user account:

```sql
CREATE USER 'mysql'@'localhost' IDENTIFIED VIA unix_socket;
GRANT RELOAD, PROCESS, LOCK TABLES, REPLICATION CLIENT ON *.* TO 'mysql'@'localhost';
```

And then to configure <a undefined>wsrep_sst_auth</a>, you could set the following in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up a node:

```sql
[mariadb]
...
wsrep_sst_auth = mysql:
```

### Passwordless Authentication - GSSAPI

It is possible to use the [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi/) authentication plugin for the user account that performs SSTs. This would provide the benefit of not needing to configure a plain-text password in <a undefined>wsrep_sst_auth</a>.

The following steps would need to be done beforehand:

- You need a KDC running [MIT Kerberos](http://web.mit.edu/Kerberos/krb5-1.12/doc/index.html) or [Microsoft Active Directory](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).
- You will need to [create a keytab file](/kb/en/authentication-plugin-gssapi/#creating-a-keytab-file-on-a-unix-server) for the MariaDB server.
- You will need to [install the package](/kb/en/authentication-plugin-gssapi/#installing-the-plugins-package) containing the [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi/) authentication plugin.
- You will need to [install the plugin](/kb/en/authentication-plugin-gssapi/#installing-the-plugin) in MariaDB, so that the [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi/) authentication plugin is available to use.
- You will need to [configure the plugin](/kb/en/authentication-plugin-gssapi/#configuring-the-plugin).
- You will need to [create a user account](/kb/en/authentication-plugin-gssapi/#creating-users) that authenticates with the [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi/) authentication plugin, so that the user account can be used for SSTs. This user account will need to correspond with a user account that exists on the backend KDC.

For example, you could execute the following to create the user account in MariaDB:

```sql
CREATE USER 'xtrabackup'@'localhost' IDENTIFIED VIA gssapi;
GRANT RELOAD, PROCESS, LOCK TABLES, REPLICATION CLIENT ON *.* TO 'xtrabackup'@'localhost';
```

And then to configure <a undefined>wsrep_sst_auth</a>, you could set the following in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up a node:

```sql
[mariadb]
...
wsrep_sst_auth = xtrabackup:
```

## Choosing a Donor Node

When Percona XtraBackup is used to create the backup for the SST on the donor node, XtraBackup briefly requires a system-wide lock at the end of the backup. This is done with [FLUSH TABLES WITH READ LOCK](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).

If a specific node in your cluster is acting as the <em>primary</em> node by receiving all of the application's write traffic, then this node should not usually be used as the donor node, because the system-wide lock could interfere with the application. In this case, you can define one or more preferred donor nodes by setting the <a undefined>wsrep_sst_donor</a> system variable.

For example, let's say that we have a 5-node cluster with the nodes `node1`, `node2`,  `node3`, `node4`, and `node5`, and let's say that `node1` is acting as the <em>primary</em> node.The preferred donor nodes for `node2` could be configured by setting the following in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up a node:

```sql
[mariadb]
...
wsrep_sst_donor=node3,node4,node5,
```

The trailing comma tells the server to allow any other node as donor when the preferred donors are not available. Therefore, if `node1` is the only node left in the cluster, the trailing comma allows it to be used as the donor node.

## Socat Dependency

During the SST process, the donor node uses [socat](http://www.dest-unreach.org/socat/doc/socat.html) to stream the backup to the joiner node.  Then the joiner node prepares the backup before restoring it. The socat utility must be installed on both the donor node and the joiner node in order for this to work. Otherwise, the MariaDB error log will contain an error like:

```sql
WSREP_SST: [ERROR] socat not found in path: /usr/sbin:/sbin:/usr//bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin (20180122 14:55:32.993)
```

### Installing Socat on RHEL/CentOS

On RHEL/CentOS, `socat` can be installed from the [Extra Packages for Enterprise Linux (EPEL)](https://fedoraproject.org/wiki/EPEL) repository.

## TLS

This SST method supports three different TLS methods. The specific method can be selected by setting the `encrypt` option in the `[sst]` section of the MariaDB configuration file. The options are:

- TLS using OpenSSL encryption built into `socat` (`encrypt=2`)
- TLS using OpenSSL encryption with Galera-compatible certificates and keys (`encrypt=3`)
- TLS using OpenSSL encryption with MariaDB-compatible certificates and keys (`encrypt=4`)

Note that `encrypt=1` refers to a TLS encryption method that has been deprecated and removed.

### TLS Using OpenSSL Encryption Built into Socat

To generate keys compatible with this encryption method, you can follow [these directions](http://www.dest-unreach.org/socat/doc/socat-openssltunnel.html).

For example:

- First, generate the keys and certificates:

```sql
FILENAME=sst
openssl genrsa -out $FILENAME.key 1024
openssl req -new -key $FILENAME.key -x509 -days 3653 -out $FILENAME.crt
cat $FILENAME.key $FILENAME.crt >$FILENAME.pem
chmod 600 $FILENAME.key $FILENAME.pem
```

- On some systems, you may also have to add dhparams to the certificate:

```sql
openssl dhparam -out dhparams.pem 2048
cat dhparams.pem >> sst.pem
```

- Then, copy the certificate and keys to all nodes in the cluster.

- Then, configure the following on all nodes in the cluster:

```sql
[sst]
encrypt=2
tca=/etc/my.cnf.d/certificates/sst.crt
tcert=/etc/my.cnf.d/certificates/sst.pem
```

But replace the paths with whatever is relevant on your system.

This should allow your SSTs to be encrypted.

### TLS Using OpenSSL Encryption with Galera-compatible Certificates and Keys

To generate keys compatible with this encryption method, you can follow [these directions](https://galeracluster.com/library/documentation/ssl-sst.html#ssl-xtrabackup).

For example:

- First, generate the keys and certificates:

```sql
# CA
openssl genrsa 2048 > ca-key.pem
openssl req -new -x509 -nodes -days 365000 \
-key ca-key.pem -out ca-cert.pem
 
# server1
openssl req -newkey rsa:2048 -days 365000 \
-nodes -keyout server1-key.pem -out server1-req.pem
openssl rsa -in server1-key.pem -out server1-key.pem
openssl x509 -req -in server1-req.pem -days 365000 \
-CA ca-cert.pem -CAkey ca-key.pem -set_serial 01 \
-out server1-cert.pem
```

- Then, copy the certificate and keys to all nodes in the cluster.

- Then, configure the following on all nodes in the cluster:

```sql
[sst]
encrypt=3
tkey=/etc/my.cnf.d/certificates/server1-key.pem
tcert=/etc/my.cnf.d/certificates/server1-cert.pem
```

But replace the paths with whatever is relevant on your system.

This should allow your SSTs to be encrypted.

### TLS Using OpenSSL Encryption with MariaDB-compatible Certificates and Keys

To generate keys compatible with this encryption method, you can follow [these directions](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/certificate-creation-with-openssl/).

For example:

- First, generate the keys and certificates:

```sql
# CA
openssl genrsa 2048 > ca-key.pem
openssl req -new -x509 -nodes -days 365000 \
-key ca-key.pem -out ca-cert.pem
 
# server1
openssl req -newkey rsa:2048 -days 365000 \
-nodes -keyout server1-key.pem -out server1-req.pem
openssl rsa -in server1-key.pem -out server1-key.pem
openssl x509 -req -in server1-req.pem -days 365000 \
-CA ca-cert.pem -CAkey ca-key.pem -set_serial 01 \
-out server1-cert.pem
```

- Then, copy the certificate and keys to all nodes in the cluster.

- Then, configure the following on all nodes in the cluster:

```sql
[sst]
encrypt=4
ssl-ca=/etc/my.cnf.d/certificates/ca-cert.pem
ssl-cert=/etc/my.cnf.d/certificates/server1-cert.pem
ssl-key=/etc/my.cnf.d/certificates/server1-key.pem
```

But replace the paths with whatever is relevant on your system.

This should allow your SSTs to be encrypted.

## Logs

The `xtrabackup-v2` SST method has its own logging outside of the MariaDB Server logging.

### Logging to SST Logs

By default, on the donor node, it logs to `innobackup.backup.log`. This log file is located in the <a undefined>datadir</a>.

By default, on the joiner node, it logs to `innobackup.prepare.log` and `innobackup.move.log`. These log files are located in the `.sst` directory, which is a hidden directory inside the <a undefined>datadir</a>.

These log files are overwritten by each subsequent SST, so if an SST fails, it is best to copy them somewhere safe before starting another SST, so that the log files can be analyzed. See [MDEV-17973](https://jira.mariadb.org/browse/MDEV-17973) about that.

### Logging to Syslog

You can redirect the SST logs to the syslog instead by setting the following in the `[sst]` [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

```sql
[sst]
sst-syslog=1
```

You can also redirect the SST logs to the syslog by setting the following in the `[mysqld_safe]` [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

```sql
[mysqld_safe]
syslog
```

## Performing SSTs with IPv6 Addresses

If you are performing Percona XtraBackup SSTs with IPv6 addresses, then the `socat` utility needs to be passed the `pf=ip6` option. This can be done by setting the `sockopt` option in the `[sst]` [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[sst]
sockopt=",pf=ip6"
```

See [MDEV-18797](https://jira.mariadb.org/browse/MDEV-18797) for more information.

## Manual SST with Percona XtraBackup

In some cases, if Galera Cluster's automatic SSTs repeatedly fail, then it can be helpful to perform a "manual SST". See the following page on how to do that:

- [Manual SST of Galera Cluster node with Percona XtraBackup](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/manual-sst-of-galera-cluster-node-with-percona-xtrabackup/)

## See Also

- [Percona XtraBackup SST Configuration](https://www.percona.com/doc/percona-xtradb-cluster/5.7/manual/xtrabackup_sst.html)
- [Encrypting PXC Traffic: 
ENCRYPTING SST TRAFFIC](https://www.percona.com/doc/percona-xtradb-cluster/5.7/security/encrypt-traffic.html#encrypt-sst)
- [XTRABACKUP PARAMETERS](https://galeracluster.com/library/documentation/xtrabackup-options.html)
- [SSL FOR STATE SNAPSHOT TRANSFERS: ENABLING SSL FOR XTRABACKUP](https://galeracluster.com/library/documentation/ssl-sst.html#ssl-xtrabackup)