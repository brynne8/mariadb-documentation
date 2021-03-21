# Securing Communications in Galera Cluster

By default, Galera Cluster replicates data between each node without encrypting it.  This is generally acceptable when the cluster nodes runs on the same host or in networks where security is guaranteed through other means. However, in cases where the cluster nodes exist on separate networks or they are in a high-risk network, the lack of encryption does introduce security concerns as a malicious actor could potentially eavesdrop on the traffic or get a complete copy of the data by triggering an SST.

To mitigate this concern, Galera Cluster allows you to encrypt data in transit as it is replicated between each cluster node using the Transport Layer Security (TLS) protocol. TLS was formerly known as Secure Socket Layer (SSL), but strictly speaking the SSL protocol is a predecessor to TLS and, that version of the protocol is now considered insecure. The documentation still uses the term SSL often and for compatibility reasons TLS-related server system and status variables still use the prefix `ssl_`, but internally, MariaDB only supports its secure successors.

In order to secure connections between the cluster nodes, you need to ensure that all servers were compiled with TLS support. See [Secure Connections Overview](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview/) to determine how to check whether a server was compiled with TLS support.

For each cluster node, you also need a certificate, private key, and the Certificate Authority (CA) chain to verify the certificate. If you want to use self-signed certificates that are created with OpenSSL, then see [Certificate Creation with OpenSSL](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/certificate-creation-with-openssl/) for information on how to create those.

## Securing Galera Cluster Replication Traffic

In order to enable TLS for Galera Cluster's replication traffic, there are a number of [wsrep_provider_options](/replication/galera-cluster/wsrep_provider_options/) that you need to set, such as:

- You need to set the path to the server's certificate by setting the <a undefined>socket.ssl_cert</a> wsrep_provider_option.
- You need to set the path to the server's private key by setting the <a undefined>socket.ssl_key</a> wsrep_provider_option.
- You need to set the path to the certificate authority (CA) chain that can verify the server's certificate by setting the <a undefined>socket.ssl_ca</a> wsrep_provider_option.
- If you want to restrict the server to certain ciphers, then you also need to set the <a undefined>socket.ssl_cipher</a> wsrep_provider_option.

It is also a good idea to set MariaDB Server's regular TLS-related system variables, so that TLS will be enabled for regular client connections as well. See [Securing Connections for Client and Server](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/securing-connections-for-client-and-server/) for information on how to do that.

For example, to set these variables for the server, add the system variables to a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

```sql
[mariadb]
...
ssl_cert = /etc/my.cnf.d/certificates/server-cert.pem
ssl_key = /etc/my.cnf.d/certificates/server-key.pem
ssl_ca = /etc/my.cnf.d/certificates/ca.pem
wsrep_provider_options="socket.ssl_cert=/etc/my.cnf.d/certificates/server-cert.pem;socket.ssl_key=/etc/my.cnf.d/certificates/server-key.pem;socket.ssl_ca=/etc/my.cnf.d/certificates/ca.pem"
```

And then [restart the server](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/) to make the changes persistent.

By setting both MariaDB Server's TLS-related system variables and Galera Cluster's TLS-related wsrep_provider_options, the server can secure both external client connections and Galera Cluster's replication traffic.

## Securing State Snapshot Transfers

The method that you would use to enable TLS for [State Snapshot Transfers (SSTs)](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts/) would depend on the value of <a undefined>wsrep_sst_method</a>.

### mariabackup

See [mariabackup SST Method: TLS](/kb/en/mariabackup-sst-method/#tls) for more information.

### xtrabackup-v2

See [xtrabackup-v2 SST Method: TLS](/kb/en/xtrabackup-v2-sst-method/#tls) for more information.

### mysqldump

This SST method simply uses the [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) utility, so TLS would be enabled by following the guide at [Securing Connections for Client and Server: Enabling TLS for MariaDB Clients](/kb/en/securing-connections-for-client-and-server/#enabling-tls-for-mariadb-clients)

### rsync

This SST method supports encryption in transit via <a undefined>stunnel</a>. See [Introduction to State Snapshot Transfers (SSTs): rsync](/kb/en/introduction-to-state-snapshot-transfers-ssts/#rsync) for more information.