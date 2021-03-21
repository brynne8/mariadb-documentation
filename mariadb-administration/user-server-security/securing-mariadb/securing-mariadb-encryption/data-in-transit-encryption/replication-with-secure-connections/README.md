# Replication with Secure Connections

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

By default, MariaDB replicates data between masters and slaves without encrypting it. This is generally acceptable when the master and slave run are in networks where security is guaranteed through other means. However, in cases where the master and slave exist on separate networks or they are in a high-risk network, the lack of encryption does introduce security concerns as a malicious actor could potentially eavesdrop on the traffic as it is sent over the network between them.

To mitigate this concern, MariaDB allows you to encrypt replicated data in transit between masters and slaves using the Transport Layer Security (TLS) protocol. TLS was formerly known as Secure Socket Layer (SSL), but strictly speaking the SSL protocol is a predecessor to TLS and, that version of the protocol is now considered insecure. The documentation still uses the term SSL often and for compatibility reasons TLS-related server system and status variables still use the prefix `ssl_`, but internally, MariaDB only supports its secure successors.

In order to secure connections between the master and slave, you need to ensure that both servers were compiled with TLS support. See [Secure Connections Overview](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview) to determine how to check whether a server was compiled with TLS support.

You also need an X509 certificate, a private key, and the Certificate Authority (CA) chain to verify the X509 certificate for the master. If you want to use two-way TLS, then you will also an X509 certificate, a private key, and the Certificate Authority (CA) chain to verify the X509 certificate for the slave. If you want to use self-signed certificates that are created with OpenSSL, then see [Certificate Creation with OpenSSL](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/certificate-creation-with-openssl) for information on how to create those.

## Securing Replication Traffic

In order to secure replication traffic, you will need to ensure that TLS is enabled on the master. If you want to use two-way TLS, then you will also need to ensure that TLS is enabled on the slave. See [Securing Connections for Client and Server](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/securing-connections-for-client-and-server) for information on how to do that.

For example, to set the TLS system variables for each server, add them to a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) on each server:

```sql
[mariadb]
...
ssl_cert = /etc/my.cnf.d/certificates/server-cert.pem
ssl_key = /etc/my.cnf.d/certificates/server-key.pem
ssl_ca = /etc/my.cnf.d/certificates/ca.pem
```

And then [restart the server](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/) to make the changes persistent.

At this point, you can reconfigure the slaves to use TLS to encrypt replicated data in transit. There are two methods available to do this:

- Executing the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) statement to set the relevant TLS options.
- Setting TLS client options in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

### Executing CHANGE MASTER

TLS can be enabled on a replication slave by executing the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) statement. In order to do so, there are a number of options that you would need to set. The specific options that you would need to set would depend on whether you want one-way TLS or two-way TLS, and whether you want to verify the server certificate.

#### Enabling Two-Way TLS with CHANGE MASTER

Two-way TLS means that both the client and server provide a private key and an X509 certificate. It is called "two-way" TLS because both the client and server can be authenticated. In this case, the "client" is the slave. To configure two-way TLS, you would need to set the following options:

- You need to set the path to the server's certificate by setting the <a undefined>MASTER_SSL_CERT</a> option.
- You need to set the path to the server's private key by setting the <a undefined>MASTER_SSL_KEY</a> option.
- You need to set the path to the certificate authority (CA) chain that can verify the server's certificate by setting either the <a undefined>MASTER_SSL_CA</a> or the <a undefined>MASTER_SSL_CAPATH</a> options.
- If you want [server certificate verification](/kb/en/secure-connections-overview/#server-certificate-verification), then you also need to set the <a undefined>MASTER_SSL_VERIFY_SERVER_CERT</a> option.
- If you want to restrict the server to certain ciphers, then you also need to set the <a undefined>MASTER_SSL_CIPHER</a> option.

If the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) are currently running, you first need to stop them by executing the <a undefined>STOP SLAVE</a> statement. For example:

```sql
STOP SLAVE;
```

Then, execute the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) statement to configure the slave to use TLS. For example:

```sql
CHANGE MASTER TO
   MASTER_SSL_CERT = '/path/to/client-cert.pem',
   MASTER_SSL_KEY = '/path/to/client-key.pem',
   MASTER_SSL_CA = '/path/to/ca/ca.pem',
   MASTER_SSL_VERIFY_SERVER_CERT=1;
```

At this point, you can start replication by executing the <a undefined>START SLAVE</a> statement. For example:

```sql
START SLAVE;
```

The slave now uses TLS to encrypt data in transit as it replicates it from the master.

#### Enabling One-Way TLS with CHANGE MASTER

##### Enabling One-Way TLS with CHANGE MASTER with Server Certificate Verification

One-way TLS means that only the server provides a private key and an X509 certificate. When TLS is used without a client certificate, it is called "one-way" TLS, because only the server can be authenticated, so authentication is only possible in one direction. However, encryption is still possible in both directions. [Server certificate verification](/kb/en/secure-connections-overview/#server-certificate-verification) means that the client verifies that the certificate belongs to the server. In this case, the "client" is the slave. To configure one-way TLS, you would need to set the following options:

- You need to set the path to the certificate authority (CA) chain that can verify the server's certificate by setting either the <a undefined>MASTER_SSL_CA</a> or the <a undefined>MASTER_SSL_CAPATH</a> options.
- If you want [server certificate verification](/kb/en/secure-connections-overview/#server-certificate-verification), then you also need to set the <a undefined>MASTER_SSL_VERIFY_SERVER_CERT</a> option.
- If you want to restrict the server to certain ciphers, then you also need to set the <a undefined>MASTER_SSL_CIPHER</a> option.

If the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) are currently running, you first need to stop them by executing the <a undefined>STOP SLAVE</a> statement. For example:

```sql
STOP SLAVE;
```

Then, execute the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) statement to configure the slave to use TLS. For example:

```sql
CHANGE MASTER TO
   MASTER_SSL_CA = '/path/to/ca/ca.pem',
   MASTER_SSL_VERIFY_SERVER_CERT=1;
```

At this point, you can start replication by executing the <a undefined>START SLAVE</a> statement. For example:

```sql
START SLAVE;
```

The slave now uses TLS to encrypt data in transit as it replicates it from the master.

##### Enabling One-Way TLS with CHANGE MASTER without Server Certificate Verification

One-way TLS means that only the server provides a private key and an X509 certificate. When TLS is used without a client certificate, it is called "one-way" TLS, because only the server can be authenticated, so authentication is only possible in one direction. However, encryption is still possible in both directions. In this case, the "client" is the slave. To configure two-way TLS without server certificate verification, you would need to set the following options:

- You need to configure the slave to use TLS by setting the <a undefined>MASTER_SSL</a> option.
- If you want to restrict the server to certain ciphers, then you also need to set the <a undefined>MASTER_SSL_CIPHER</a> option.

If the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) are currently running, you first need to stop them by executing the <a undefined>STOP SLAVE</a> statement. For example:

```sql
STOP SLAVE;
```

Then, execute the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) statement to configure the slave to use TLS. For example:

```sql
CHANGE MASTER TO
   MASTER_SSL=1;
```

At this point, you can start replication by executing the <a undefined>START SLAVE</a> statement. For example:

```sql
START SLAVE;
```

The slave now uses TLS to encrypt data in transit as it replicates it from the master.

### Setting TLS Client Options in an Option File

In cases where you don't mind restarting the server or you are setting the server up from scratch for the first time, you may find it more convenient to configure TLS options for replication through an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). This is done the same way as it is for other clients. The specific options that you would need to set would depend on whether you want one-way TLS or two-way TLS, and whether you want to verify the server certificate. See [Securing Connections for Client and Server: Enabling TLS for MariaDB Clients](/kb/en/securing-connections-for-client-and-server/#enabling-tls-for-mariadb-clients) for more information.

For example, to enable two-way TLS with [server certificate verification](/kb/en/secure-connections-overview/#server-certificate-verification), then you could specify the following options in a a relevant client [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

```sql
[client-mariadb]
...
ssl_cert = /etc/my.cnf.d/certificates/client-cert.pem
ssl_key = /etc/my.cnf.d/certificates/client-key.pem
ssl_ca = /etc/my.cnf.d/certificates/ca.pem
ssl-verify-server-cert
```

Before you restart the server, you may also want to set the <a undefined>--skip-slave-start</a> option in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). This option prevents the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) from restarting automatically when the server starts. Instead, they will have to be restarted manually.

After these changes have been made, you can [restart the server](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

Once the server is back online, set the <a undefined>MASTER_SSL</a> option by executing the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) statement. This will enable TLS. For example:

```sql
CHANGE MASTER TO
   MASTER_SSL=1;
```

The certificate and keys will be read from the option file.

At this point, you can start replication by executing the <a undefined>START SLAVE</a> statement.

```sql
START SLAVE;
```