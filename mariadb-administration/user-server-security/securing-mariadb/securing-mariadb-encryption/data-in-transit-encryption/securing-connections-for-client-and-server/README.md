# Securing Connections for Client and Server

By default, MariaDB transmits data between the server and clients without encrypting it. This is generally acceptable when the server and client run on the same host or in networks where security is guaranteed through other means. However, in cases where the server and client exist on separate networks or they are in a high-risk network, the lack of encryption does introduce security concerns as a malicious actor could potentially eavesdrop on the traffic as it is sent over the network between them.

To mitigate this concern, MariaDB allows you to encrypt data in transit between the server and clients using the Transport Layer Security (TLS) protocol. TLS was formerly known as Secure Socket Layer (SSL), but strictly speaking the SSL protocol is a predecessor to TLS and, that version of the protocol is now considered insecure. The documentation still uses the term SSL often and for compatibility reasons TLS-related server system and status variables still use the prefix `ssl_`, but internally, MariaDB only supports its secure successors.

In order to secure connections between the server and client, you need to ensure that your server was compiled with TLS support. See [Secure Connections Overview](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview/) to determine how to check whether a server was compiled with TLS support.

You also need an X509 certificate, a private key, and the Certificate Authority (CA) chain to verify the X509 certificate for the server. If you want to use two-way TLS, then you will also an X509 certificate, a private key, and the Certificate Authority (CA) chain to verify the X509 certificate for the client. If you want to use self-signed certificates that are created with OpenSSL, then see [Certificate Creation with OpenSSL](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/certificate-creation-with-openssl/) for information on how to create those.

## Enabling TLS

### Enabling TLS for MariaDB Server

In order to enable TLS on a MariaDB server that was compiled with TLS support, there are a number of system variables that you need to set, such as:

- You need to set the path to the server's X509 certificate by setting the <a undefined>ssl_cert</a> system variable.
- You need to set the path to the server's private key by setting the <a undefined>ssl_key</a> system variable.
- You need to set the path to the certificate authority (CA) chain that can verify the server's certificate by setting either the <a undefined>ssl_ca</a> or the <a undefined>ssl_capath</a> system variables.
- If you want to restrict the server to certain ciphers, then you also need to set the <a undefined>ssl_cipher</a> system variable.

For example, to set these variables for the server, add the system variables to a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

```sql
[mariadb]
...
ssl_cert = /etc/my.cnf.d/certificates/server-cert.pem
ssl_key = /etc/my.cnf.d/certificates/server-key.pem
ssl_ca = /etc/my.cnf.d/certificates/ca.pem
```

And then [restart the server](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/) to make the changes persistent.

Once the server is back up, you can check that TLS is enabled by checking the value of the <a undefined>have_ssl</a> system variable. For example:

```sql
SHOW VARIABLES LIKE 'have_ssl';

+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| have_ssl      | YES   |
+---------------+-------+
```

#### Reloading the Server's Certificates and Keys Dynamically

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

The `FLUSH SSL` command was first added in [MariaDB 10.4](/kb/en/what-is-mariadb-104/).

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, the `FLUSH SSL` command can be used to dynamically reinitialize the server's [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/) context.

See <a undefined>FLUSH SSL</a> for more information.

### Enabling TLS for MariaDB Clients

Different [clients and utilities](/clients-utilities/) may use different methods to enable TLS.

For many of the standard [clients and utilities](/clients-utilities/) that come bundled with MariaDB, you can enable two-way TLS by adding the same options that were set for the server to a relevant client [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[client-mariadb]
...
ssl_cert = /etc/my.cnf.d/certificates/client-cert.pem
ssl_key = /etc/my.cnf.d/certificates/client-key.pem
ssl_ca = /etc/my.cnf.d/certificates/ca.pem
```

The specific options that you would need to set would depend on whether you want one-way TLS or two-way TLS, and whether you want to verify the server certificate.

The same options may also enable TLS on non-standard [clients and utilities](/clients-utilities/) that are linked with either [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) or [MariaDB Connector/C](/kb/en/mariadb-connector-c/).

#### Enabling Two-Way TLS for MariaDB Clients

Two-way TLS means that both the client and server provide a private key and an X509 certificate. It is called "two-way" TLS because both the client and server can be authenticated. For example, to specify these options in a relevant client [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), you could set the following:

```sql
[client-mariadb]
...
ssl_cert = /etc/my.cnf.d/certificates/client-cert.pem
ssl_key = /etc/my.cnf.d/certificates/client-key.pem
ssl_ca = /etc/my.cnf.d/certificates/ca.pem
ssl-verify-server-cert
```

Or if you wanted to specify them on the command-line with the [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) client, then you could execute something like this:

```sql
$ mysql -u myuser -p -h myserver.mydomain.com \
   --ssl-cert=/etc/my.cnf.d/certificates/client-cert.pem \
   --ssl-key=/etc/my.cnf.d/certificates/client-key.pem \
   --ssl-ca=/etc/my.cnf.d/certificates/ca.pem \
   --ssl-verify-server-cert
```

Two-way SSL is required for an account if the `REQUIRE X509`, `REQUIRE SUBJECT`, and/or `REQUIRE ISSUER` clauses are specified for the account.

#### Enabling One-Way TLS for MariaDB Clients

##### Enabling One-Way TLS for MariaDB Clients with Server Certificate Verification

One-way TLS means that only the server provides a private key and an X509 certificate. When TLS is used without a client certificate, it is called "one-way" TLS, because only the server can be authenticated, so authentication is only possible in one direction. However, encryption is still possible in both directions. [Server certificate verification](/kb/en/secure-connections-overview/#server-certificate-verification) means that the client verifies that the certificate belongs to the server. For example, to specify these options in a a relevant client [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), you could set the following:

```sql
[client-mariadb]
...
ssl_ca = /etc/my.cnf.d/certificates/ca.pem
ssl-verify-server-cert
```

Or if you wanted to specify them on the command-line with the [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) client, then you could execute something like this:

```sql
$ mysql -u myuser -p -h myserver.mydomain.com \
   --ssl-ca=/etc/my.cnf.d/certificates/ca.pem \
   --ssl-verify-server-cert
```

##### Enabling One-Way TLS for MariaDB Clients without Server Certificate Verification

One-way TLS means that only the server provides a private key and an X509 certificate. When TLS is used without a client certificate, it is called "one-way" TLS, because only the server can be authenticated, so authentication is only possible in one direction. However, encryption is still possible in both directions. For example, to specify these options in a a relevant client [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), you could set the following:

```sql
[client-mariadb]
...
ssl
```

Or if you wanted to specify them on the command-line with the [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) client, then you could execute something like this:

```sql
$ mysql -u myuser -p -h myserver.mydomain.com \
   --ssl
```

### Enabling TLS for MariaDB Connector/C Clients

See the documentation on MariaDB Connector/C's [TLS Options](/kb/en/mysql_optionsv/#tlsssl-options) for information on how to enable TLS for clients that use MariaDB Connector/C.

### Enabling TLS for MariaDB Connector/ODBC Clients

See the documentation on MariaDB Connector/ODBC's [TLS-Related Connection Parameters](/kb/en/about-mariadb-connector-odbc/#tls-related-connection-parameters) for information on how to enable TLS for clients that use MariaDB Connector/ODBC.

### Enabling TLS for MariaDB Connector/J Clients

See the documentation on [Using TLS/SSL with MariaDB Connector/J](/kb/en/using-tls-ssl-with-mariadb-java-connector/) for information on how to enable TLS for clients that use MariaDB Connector/J.

## Verifying that a Connection is Using TLS

You can verify that a connection is using TLS by checking the connection's <a undefined>Ssl_cipher</a> status variable. If it is non-empty, then the connection is using TLS. For example:

```sql
SHOW SESSION STATUS LIKE 'Ssl_cipher';
+---------------+---------------------------+
| Variable_name | Value                     |
+---------------+---------------------------+
| Ssl_cipher    | DHE-RSA-AES256-GCM-SHA384 |
+---------------+---------------------------+
1 row in set (0.00 sec)
```

## Requiring TLS

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [require_secure_transport](/kb/en/server-system-variables/#require_secure_transport) system variable is available. When set (by default it is off), connections attempted using insecure transport will be rejected. Secure transports are SSL/TLS, Unix sockets or named pipes. Note that requirements set for specific user accounts will take precedence over this setting.

### Requiring TLS for Specific User Accounts

You can set certain TLS-related restrictions for specific user accounts. For instance, you might use this with user accounts that require access to sensitive data while sending it across networks that you do not control. These restrictions can be enabled for a user account with the [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/), [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user/), or [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) statements. For example:

- A user account must connect via TLS if the user account is defined with the `REQUIRE SSL` clause.

```sql
ALTER USER 'alice'@'%' 
   REQUIRE SSL;
```

- A user account must connect via TLS with a specific cipher if the user account is defined with the `REQUIRE CIPHER` clause.

```sql
ALTER USER 'alice'@'%' 
   REQUIRE CIPHER 'ECDH-RSA-AES256-SHA384';
```

- A user account must connect via TLS with a valid client certificate if the user account is defined with the `REQUIRE X509` clause.

```sql
ALTER USER 'alice'@'%' 
   REQUIRE X509;
```

- A user account must connect via TLS with a specific client certificate if the user account is defined with the `REQUIRE SUBJECT` clause.

```sql
ALTER USER 'alice'@'%' 
   REQUIRE SUBJECT '/CN=alice/O=My Dom, Inc./C=US/ST=Oregon/L=Portland';
```

- A user account must connect via TLS with a client certificate that must be signed by a specific certificate authority if the user account is defined with the `REQUIRE ISSUER` clause.

```sql
ALTER USER 'alice'@'%' 
   REQUIRE SUBJECT '/CN=alice/O=My Dom, Inc./C=US/ST=Oregon/L=Portland'
   AND ISSUER '/C=FI/ST=Somewhere/L=City/ O=Some Company/CN=Peter Parker/emailAddress=p.parker@marvel.com';
```

### Requiring TLS for Specific User Accounts from Specific Hosts

A user account can have different definitions depending on what host the user account is logging in from. Therefore, it is possible to have different TLS requirements for the same username for different hosts. For example:

```sql
CREATE USER 'alice'@'localhost' 
   REQUIRE NONE;

CREATE USER 'alice'@'%'
   REQUIRE SUBJECT '/CN=alice/O=My Dom, Inc./C=US/ST=Oregon/L=Portland'
   AND ISSUER '/C=FI/ST=Somewhere/L=City/ O=Some Company/CN=Peter Parker/emailAddress=p.parker@marvel.com'
   AND CIPHER 'ECDHE-ECDSA-AES256-SHA384';
```

In the above example, the `alice` user account does not require TLS when logging in from localhost. However, when the `alice` user account logs in from any other host, they must use TLS with the given cipher, and they must provide a valid client certificate with the given subject that must have been signed by the given issuer.