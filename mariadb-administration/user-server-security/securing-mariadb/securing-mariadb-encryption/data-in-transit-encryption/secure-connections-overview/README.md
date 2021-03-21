# Secure Connections Overview

By default, MariaDB transmits data between the server and clients without encrypting it. This is generally acceptable when the server and client run on the same host or in networks where security is guaranteed through other means. However, in cases where the server and client exist on separate networks or they are in a high-risk network, the lack of encryption does introduce security concerns as a malicious actor could potentially eavesdrop on the traffic as it is sent over the network between them.

To mitigate this concern, MariaDB allows you to encrypt data in transit between the server and clients using the Transport Layer Security (TLS) protocol. TLS was formerly known as Secure Socket Layer (SSL), but strictly speaking the SSL protocol is a predecessor to TLS and, that version of the protocol is now considered insecure. The documentation still uses the term SSL often and for compatibility reasons TLS-related server system and status variables still use the prefix `ssl_`, but internally, MariaDB only supports its secure successors.

## Checking MariaDB Server for TLS Support

In order for MariaDB Server to use TLS, it needs to be compiled with TLS support. All MariaDB packages distributed by MariaDB Foundation and MariaDB Corporation are compiled with TLS support.

If you aren't sure whether your MariaDB Server binary was compiled with TLS support, then you can check the value of the <a undefined>have_ssl</a> system variable. For example:

```sql
SHOW GLOBAL VARIABLES LIKE 'have_ssl';
+---------------+----------+
| Variable_name | Value    |
+---------------+----------+
| have_ssl      | DISABLED |
+---------------+----------+
```

The possible values are:

- If it is `DISABLED`, then the server was compiled with TLS support, but TLS is not enabled.
- If it is `YES`, then the server was compiled with TLS support, and TLS is enabled.
- If it is `NO`, then the server was not compiled with TLS support.

## TLS Libraries

When MariaDB is compiled with TLS and cryptography support, it is usually either statically linked with MariaDB's bundled TLS and cryptography library, which might be [wolfSSL](https://www.wolfssl.com/products/wolfssl/) or [yaSSL](https://www.wolfssl.com/products/yassl/), or dynamically linked with the system's TLS and cryptography library, which might be [OpenSSL](https://www.openssl.org/), [GnuTLS](https://www.gnutls.org/), or [Schannel](https://docs.microsoft.com/en-us/windows/desktop/secauthn/secure-channel).

See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb/) for more information about which libraries are used on which platforms.

## TLS Protocol Versions

There are 4 versions of the TLS protocol:

- `TLSv1.0`
- `TLSv1.1`
- `TLSv1.2`
- `TLSv1.3`

### Enabling Specific TLS Protocol Versions

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

The <a undefined>tls_version</a> system variable was first introduced in [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/).

In some cases, it might make sense to only enable specific TLS protocol versions. For example, it would make sense if your organization has to comply with a specific security standard. It would also make sense if a vulnerability is found in a specific TLS protocol version, and you would like to ensure that your server does not use the vulnerable protocol version.

The [PCI DSS v3.2](https://blog.pcisecuritystandards.org/resource-guide-migrating-from-ssl-and-early-tls) recommends using a minimum protocol version of TLSv1.2.

On the <strong>server</strong> side, users can enable specific TLS protocol versions by setting the <a undefined>tls_version</a> system variable. This system variable accepts a comma-separated list of TLS protocol versions. A TLS protocol version will only be enabled if it is present in this list. All other TLS protocol versions will not be permitted. This system variable can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
tls_version = TLSv1.2,TLSv1.3
```

You can check which TLS protocol versions are enabled on a server by executing [SHOW GLOBAL VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables/). For example:

```sql
SHOW GLOBAL VARIABLES LIKE 'tls_version';
```

On the <strong>client</strong> side, users can enable specific TLS protocol versions by setting the `--tls-version` option. This option accepts a comma-separated list of TLS protocol versions. A TLS protocol version will only be enabled if it is present in this list. All other TLS protocol versions will not be permitted. For example, to specify this option in a relevant client [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), you could set the following:

```sql
[client-mariadb]
...
tls_version = TLSv1.2,TLSv1.3
```

Or if you wanted to specify it on the command-line with the [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) client, then you could execute something like this:

```sql
$ mysql -u myuser -p -h myserver.mydomain.com \
   --ssl \
   --tls-version="TLSv1.2,TLSv1.3"
```

### TLS Protocol Version Support

The TLS protocol versions that are supported depend on the underlying TLS library used by the specific MariaDB binary.

<table><tbody><tr><th>TLS Library</th><th>Supported TLS Protocol Versions</th></tr>
<tr><td>openSSL</td><td>TLSv1, TLSv1.1, TLSv1.2, TLSv1.3</td></tr>
<tr><td>wolfSSL</td><td>TLSv1, TLSv1.1, TLSv1.2, TLSv1.3</td></tr>
<tr><td>yaSSL</td><td>TLSv1, TLSv1.1</td></tr>
<tr><td>Schannel</td><td>TLSv1, TLSv1.1, TLSv1.2</td></tr>
<tr><td>GnuTLS</td><td>TLSv1, TLSv1.1, TLSv1.2, TLSv1.3</td></tr>
</tbody></table>

See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb/) for more information about which libraries are used by the server and by clients on each platform.

#### TLS Protocol Version Support in OpenSSL

MariaDB binaries built with the [OpenSSL](https://www.openssl.org/) library ([OpenSSL 1.0.1](https://www.openssl.org/news/changelog.html) or later) support TLSv1.1 and TLSv1.2 since [MariaDB 5.5.41](/kb/en/mariadb-5541-release-notes/), [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/), and [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/).

MariaDB binaries built with the [OpenSSL](https://www.openssl.org/) library ([OpenSSL 1.1.1](https://www.openssl.org/news/changelog.html) or later) support TLSv1.3 since [MariaDB 10.2.16](/kb/en/mariadb-10216-release-notes/) and [MariaDB 10.3.8](/kb/en/mariadb-1038-release-notes/).

If your MariaDB Server binary is built with [OpenSSL](https://www.openssl.org/), then you can set the <a undefined>ssl_cipher</a> system variable to values like `SSLv3` or `TLSv1.2` to allow all SSLv3.0 or all TLSv1.2 ciphers. However, this does not necessarily limit the protocol version to TLSv1.2. See [MDEV-14101](https://jira.mariadb.org/browse/MDEV-14101) for more information about that.

Note that the `TLSv1.3` ciphers cannot be excluded when using [OpenSSL](https://www.openssl.org/), even by using the <a undefined>ssl_cipher</a> system variable. See [Using TLSv1.3](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/using-tlsv13/) for details.

SSLv3.0 is known to be vulnerable to the [POODLE attack](https://en.wikipedia.org/wiki/POODLE), so it should not be used. SSLv2.0 and SSLv3.0 are disabled for MariaDB Server binaries linked with [OpenSSL](https://www.openssl.org/) since [MariaDB 5.5.41](/kb/en/mariadb-5541-release-notes/), [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/), and [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/). If you are using a MariaDB version older than that and you cannot upgrade, then please see the section titled "SSL 3.0 Fallback protection" in [OpenSSL Security Advisory - 15 Oct 2014](https://www.openssl.org/news/secadv/20141015.txt).

#### TLS Protocol Version Support in wolfSSL

MariaDB binaries built with the bundled [wolfSSL](https://www.wolfssl.com/products/wolfssl/) library support TLSv1.0, TLSv1.1, TLSv1.2, and TLSv1.3.

#### TLS Protocol Version Support in yaSSL

MariaDB binaries built with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) library support SSLv3.0, TLSv1.0, and TLSv1.1.

SSLv3.0 is known to be vulnerable to the [POODLE attack](https://en.wikipedia.org/wiki/POODLE), so it should not be used. SSLv2.0 and SSLv3.0 are disabled for MariaDB Server binaries linked with [yaSSL](https://www.wolfssl.com/products/yassl/) since [MariaDB 5.5.41](/kb/en/mariadb-5541-release-notes/), [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/), and [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/).

#### TLS Protocol Version Support in Schannel

MariaDB binaries built with the [Schannel](https://docs.microsoft.com/en-us/windows/desktop/secauthn/secure-channel) library support different versions of TLS on different versions of Windows. See the [Protocols in TLS/SSL (Schannel SSP)](https://docs.microsoft.com/en-us/windows/desktop/secauthn/protocols-in-tls-ssl--schannel-ssp-) documentation from Microsoft to determine which versions of TLS are supported on each version of Windows.

#### TLS Protocol Version Support in GnuTLS

MariaDB binaries built with the [GnuTLS](https://www.gnutls.org/) library support TLSv1.0, TLSv1.1, TLSv1.2, and TLSv1.3.

## Enabling TLS

See [Securing Connections for Client and Server](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/securing-connections-for-client-and-server/) for information on how to enable TLS on the client and server.

## Certificate Verification

Certificate verification is how TLS authenticates its connections by verifying that it is talking to who it says it is. There are multiple components to this verification process:

- Was the certificate signed by a trusted Certificate Authority (CA)?
- Is the certificate expired?
- Is the certificate on my Certificate Revocation List (CRL)?
- Does the certificate belong to who I believe that I'm communicating with?

### Certificate Authorities (CAs)

Certificate Authorities (CAs) are entities that you trust to sign TLS certificates. Your organization might have its own internal CA, or it might use trusted third-party CAs.

CAs are specified on the server and client by using the <a undefined>ssl_ca</a> and <a undefined>ssl_capath</a> options.

The <a undefined>ssl_ca</a> option defines a path to a PEM file that should contain one or more X509 certificates for trusted Certificate Authorities (CAs). This option requires that you use the absolute path, not a relative path.

The <a undefined>ssl_capath</a> option defines a path to a directory that contains one or more PEM files that should each contain one X509 certificate for a trusted Certificate Authority (CA). This option requires that you use the absolute path, not a relative path. The <a undefined>ssl_capath</a> option is only supported if the server or client was built with [OpenSSL](https://www.openssl.org/), [wolfSSL](https://www.wolfssl.com/products/wolfssl/), or [yaSSL](https://www.wolfssl.com/products/yassl/). If the client was built with [GnuTLS](https://www.gnutls.org/) or [Schannel](https://docs.microsoft.com/en-us/windows/desktop/secauthn/secure-channel), then the <a undefined>ssl_capath</a> option is not supported.

See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb/) for more information about which libraries are used on which platforms.

The directory specified by <a undefined>ssl_capath</a> needs to be run through the <a undefined>openssl rehash</a> command. For example, if the following is configured:

```sql
ssl_capath=/etc/my.cnf.d/certificates/ca/
```

Then you would have to execute the following:

```sql
openssl rehash /etc/my.cnf.d/certificates/ca/
```

#### Requiring a Specific Certificate Authority (CA)

The server can require a specific Certificate Authority (CA) for a client if the client's user account has been defined with `REQUIRE ISSUER`. See [Securing Connections for Client and Server: Requiring TLS](/kb/en/securing-connections-for-client-and-server/#requiring-tls) for more information.

### Certificate Revocation Lists (CRLs)

Certificate Revocation Lists (CRLs) are lists of certificates that have been revoked by the Certificate Authority (CA) before they were due to expire.

CRLs are specified on the server and client by using the <a undefined>ssl_crl</a> and <a undefined>ssl_crlpath</a> options.

The <a undefined>ssl_crl</a> option defines a path to a PEM file that should contain one or more X509 revoked certificates. This option requires that you use the absolute path, not a relative path. For servers, the <a undefined>ssl_crl</a> option is only valid if the server was built with OpenSSL. If the server was built with [wolfSSL](https://www.wolfssl.com/products/wolfssl/) or [yaSSL](https://www.wolfssl.com/products/yassl/), then the <a undefined>ssl_crl</a> option is not supported. For clients, the <a undefined>ssl_crl</a> option is only valid if the client was built with [OpenSSL](https://www.openssl.org/) or [Schannel](https://docs.microsoft.com/en-us/windows/desktop/secauthn/secure-channel). Likewise, if the client was built with [GnuTLS](https://www.gnutls.org/), [wolfSSL](https://www.wolfssl.com/products/wolfssl/) or [yaSSL](https://www.wolfssl.com/products/yassl/), then the <a undefined>ssl_crl</a> option is not supported.

The <a undefined>ssl_crlpath</a> option defines a path to a directory that contains one or more PEM files that should each contain one revoked X509 certificate. This option requires that you use the absolute path, not a relative path. The <a undefined>ssl_crlpath</a> option is only supported if the server or client was built with [OpenSSL](https://www.openssl.org/). If the server was built with [wolfSSL](https://www.wolfssl.com/products/wolfssl/) or [yaSSL](https://www.wolfssl.com/products/yassl/), then the <a undefined>ssl_crlpath</a> option is not supported. Likewise, if the client was built with [GnuTLS](https://www.gnutls.org/), [Schannel](https://docs.microsoft.com/en-us/windows/desktop/secauthn/secure-channel), [wolfSSL](https://www.wolfssl.com/products/wolfssl/), or [yaSSL](https://www.wolfssl.com/products/yassl/), then the <a undefined>ssl_crlpath</a> option is not supported.

See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb/) for more information about which libraries are used on which platforms.

The directory specified by <a undefined>ssl_crlpath</a> needs to be run through the <a undefined>openssl rehash</a> command. For example, if the following is configured:

```sql
ssl_crlpath=/etc/my.cnf.d/certificates/crl/
```

Then you would have to execute the following:

```sql
openssl rehash /etc/my.cnf.d/certificates/crl/
```

### Server Certificate Verification

[Clients and utilities](/clients-utilities/) verify a server certificate by checking the server's host name and IP address against certain attributes in the certificate. For most [clients and utilities](/clients-utilities/), server certificate verification is disabled by default, and it is only enabled if an option, such as `ssl-verify-server-cert` is specified.

To verify the server's certificate, [clients and utilities](/clients-utilities/) will check the <strong>Common Name (CN)</strong> attribute located in the <strong>[Subject](https://tools.ietf.org/html/rfc5280#section-4.1.2.6)</strong> field of the certificate against the server's host name and IP address. If the <strong>Common Name (CN)</strong> matches either of those, then the certificate is verified.

#### Server Certificate Verification with Subject Alternative Names (SANs)

The <strong>[Subject Alternative Name (SAN)](https://tools.ietf.org/html/rfc5280#section-4.2.1.6)</strong> field, which is an X.509v3 extension, can also be used for server certificate verification, if it is present in the server certificate. This field is also sometimes called <strong>subjectAltName</strong>. When using a [client or utility](/clients-utilities/) that supports server certificate verification with <strong>subjectAltName</strong> fields, if the server certificate contains any <strong>subjectAltName</strong> fields, then those fields will also be checked against the server's host name and IP address.

Whether server certificate verification with <strong>subjectAltName</strong> fields is supported depends on the underlying TLS library used by the [client or utility](/clients-utilities/).

See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb/) for more information about which libraries are used on which platforms.

##### SAN Support with OpenSSL, wolfSSL, and yaSSL

For [clients and utilities](/clients-utilities/) built with [OpenSSL](https://www.openssl.org/) ([OpenSSL 1.0.2](https://www.openssl.org/news/changelog.html) or later), support for server certificate verification with <strong>subjectAltName</strong> fields that contain the server's <strong>host name</strong> was added in [MariaDB 10.1.23](/kb/en/mariadb-10123-release-notes/) and [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/). See [MDEV-10594](https://jira.mariadb.org/browse/MDEV-10594) for more information.

For [clients and utilities](/clients-utilities/) built with [OpenSSL](https://www.openssl.org/) ([OpenSSL 1.0.2](https://www.openssl.org/news/changelog.html) or later), support for server certificate verification with <strong>subjectAltName</strong> fields that contain the server's <strong>IP address</strong> was added in [MariaDB 10.1.39](/kb/en/mariadb-10139-release-notes/), [MariaDB 10.2.24](/kb/en/mariadb-10224-release-notes/), [MariaDB 10.3.15](/kb/en/mariadb-10315-release-notes/), and [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/). See [MDEV-18131](https://jira.mariadb.org/browse/MDEV-18131) for more information.

This support also applies to other TLS libraries that use OpenSSL's API. In OpenSSL's API, server certificate verification with <strong>subjectAltName</strong> fields depends on the <a undefined>X509_check_host</a> and <a undefined>X509_check_ip</a> functions. These functions are supported in the following TLS libraries:

- [OpenSSL](https://www.openssl.org/) 1.0.2 or later
- [wolfSSL](https://www.wolfssl.com/products/wolfssl/)

And they are <strong>not</strong> supported in the following TLS libraries:

- [yaSSL](https://www.wolfssl.com/products/yassl/)

MariaDB's [RPM packages](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/) were built with [OpenSSL](https://www.openssl.org/) 1.0.1 on RHEL 7 and CentOS 7, even after OpenSSL 1.0.2 became available on those distributions. As a side effect, the [clients and utilities](/clients-utilities/) bundled in these packages did not support server certificate verification with the <strong>subjectAltName</strong> field, even if the packages were installed on a system that had OpenSSL 1.0.2 installed. Starting with MariaDB [MariaDB 10.1.39](/kb/en/mariadb-10139-release-notes/), [MariaDB 10.2.23](/kb/en/mariadb-10223-release-notes/), [MariaDB 10.3.14](/kb/en/mariadb-10314-release-notes/), and [MariaDB 10.4.4](/kb/en/mariadb-1044-release-notes/), MariaDB's [RPM packages](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/) on RHEL 7 and CentOS 7 are built with OpenSSL 1.0.2. See [MDEV-18277](https://jira.mariadb.org/browse/MDEV-18277) for more information.

##### SAN Support with Schannel

For [clients and utilities](/clients-utilities/) linked with [Schannel](https://docs.microsoft.com/en-us/windows/desktop/secauthn/secure-channel), support for server certificate verification with <strong>subjectAltName</strong> fields was added in [MariaDB Connector/C](/kb/en/mariadb-connector-c/) 3.0.2. See [CONC-250](https://jira.mariadb.org/browse/CONC-250) for more information.

##### SAN Support with GnuTLS

For [clients and utilities](/clients-utilities/) linked with GnuTLS, support for server certificate verification with <strong>subjectAltName</strong> fields was added in [MariaDB Connector/C](/kb/en/mariadb-connector-c/) 3.0.0. See [CONC-250](https://jira.mariadb.org/browse/CONC-250) for more information.

### Client Certificate Verification

The server verifies a client certificate by checking the client's known `SUBJECT` against the <strong>Subject</strong> attribute in the client's certificate. This is only done for user accounts that have been defined with `REQUIRE SUBJECT`. See [Securing Connections for Client and Server: Requiring TLS](/kb/en/securing-connections-for-client-and-server/#requiring-tls) for more information.

## See Also

- [Securing Connections for Client and Server](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/securing-connections-for-client-and-server/)
- [Using TLSv1.3](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/using-tlsv13/)
- [Certificate Creation with OpenSSL](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/certificate-creation-with-openssl/)
- [Replication with Secure Connections](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/replication-with-secure-connections/)
- [Securing Communications in Galera Cluster](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/securing-communications-in-galera-cluster/)
- [SSL/TLS System Variables](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/ssltls-system-variables/)
- [Data-at-Rest Encryption](/kb/en/data-at-rest-encryption/)
- [Cyberciti tutorial: How to setup MariaDB SSL and secure connections from clients](https://www.cyberciti.biz/faq/how-to-setup-mariadb-ssl-and-secure-connections-from-clients/)