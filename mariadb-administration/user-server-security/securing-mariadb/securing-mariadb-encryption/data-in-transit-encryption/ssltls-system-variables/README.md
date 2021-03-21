# SSL/TLS System Variables

The system variables listed on this page relate to encrypting data during transfer between servers and clients using the Transport Layer Security (TLS) protocol.  Often, the term Secure Sockets Layer (SSL) is used interchangeably with TLS, although strictly speaking the SSL protocol is the predecessor of TLS and is no longer considered secure.

For compatibility reasons, the TLS system variables in MariaDB still use the `ssl_` prefix, but MariaDB only supports its more secure successors.  For more information on SSL/TLS in MariaDB, see [Secure Connections Overview](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview).

## Variables

#### `have_openssl`

- <strong>Description:</strong> This variable shows whether the server is linked with [OpenSSL](https://www.openssl.org/) rather than MariaDB's bundled TLS library, which might be [wolfSSL](https://www.wolfssl.com/products/wolfssl/) or [yaSSL](https://www.wolfssl.com/products/yassl/).
<ul start="1"><li>In [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/) and later, if this system variable shows `YES`, then the server is linked with OpenSSL.
</li><li>In [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/) and before, this system variable was an alias for the <a undefined>have_ssl</a> system variable.
</li><li>See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb) for more information about which libraries are used on which platforms.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No

---

#### `have_ssl`

- <strong>Description:</strong> This variable shows whether the server supports using [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption) to secure connections.
<ul start="1"><li>If the value is `YES`, then the server supports TLS, and TLS is enabled.
</li><li>If the value is `DISABLED`, then the server supports TLS, but TLS is <strong>not</strong> enabled.
</li><li>If the value is `NO`, then the server was not compiled with TLS support, so TLS cannot be enabled.
</li><li>When TLS is supported, check the <a undefined>have_openssl</a> system variable to determine whether the server is using OpenSSL or MariaDB's bundled TLS library. See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb) for more information about which libraries are used on which platforms.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No

---

#### `ssl_ca`

- <strong>Description:</strong> Defines a path to a PEM file that should contain one or more X509 certificates for trusted Certificate Authorities (CAs) to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). This system variable requires that you use the absolute path, not a relative path. This system variable implies the <a undefined>ssl</a> option.
<ul start="1"><li>See [Secure Connections Overview: Certificate Authorities (CAs)](/kb/en/secure-connections-overview/#certificate-authorities-cas) for more information.
</li></ul>
- <strong>Commandline:</strong> `--ssl-ca=file_name`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `file name`

---

#### `ssl_capath`

- <strong>Description:</strong> Defines a path to a directory that contains one or more PEM files that should each contain one X509 certificate for a trusted Certificate Authority (CA) to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). This system variable requires that you use the absolute path, not a relative path. The directory specified by this variable needs to be run through the <a undefined>openssl rehash</a> command. This system variable implies the <a undefined>ssl</a> option.
<ul start="1"><li>See [Secure Connections Overview: Certificate Authorities (CAs)](/kb/en/secure-connections-overview/#certificate-authorities-cas) for more information.
</li></ul>
- <strong>Commandline:</strong> `--ssl-capath=directory_name`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `directory name`

---

#### `ssl_cert`

- <strong>Description:</strong> Defines a path to the X509 certificate file to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). This system variable requires that you use the absolute path, not a relative path. This system variable implies the <a undefined>ssl</a> option.
- <strong>Commandline:</strong> `--ssl-cert=name`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `file name`
- <strong>Default Value:</strong> None

---

#### `ssl_cipher`

- <strong>Description:</strong> List of permitted ciphers or cipher suites to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). Besides cipher names, if MariaDB was compiled with OpenSSL, this variable could be set to "SSLv3" or "TLSv1.2" to allow all SSLv3 or all TLSv1.2 ciphers. Note that the TLSv1.3 ciphers cannot be excluded when using OpenSSL, even by using this system variable. See [Using TLSv1.3](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/using-tlsv13) for details. This system variable implies the <a undefined>ssl</a> option.
- <strong>Commandline:</strong> `--ssl-cipher=name`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> None

---

#### `ssl_crl`

- <strong>Description:</strong> Defines a path to a PEM file that should contain one or more revoked X509 certificates to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). This system variable requires that you use the absolute path, not a relative path.
<ul start="1"><li>See [Secure Connections Overview: Certificate Revocation Lists (CRLs)](/kb/en/secure-connections-overview/#certificate-revocation-lists-crls) for more information.
</li><li>This variable is only valid if the server was built with OpenSSL. If the server was built with [wolfSSL](https://www.wolfssl.com/products/wolfssl/) or [yaSSL](https://www.wolfssl.com/products/yassl/), then this variable is not supported. See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb) for more information about which libraries are used on which platforms.
</li></ul>
- <strong>Commandline:</strong> `--ssl-crl=name`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `file name`
- <strong>Default Value:</strong> None
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `ssl_crlpath`

- <strong>Description:</strong> Defines a path to a directory that contains one or more PEM files that should each contain one revoked X509 certificate to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). This system variable requires that you use the absolute path, not a relative path. The directory specified by this variable needs to be run through the <a undefined>openssl rehash</a> command.
<ul start="1"><li>See [Secure Connections Overview: Certificate Revocation Lists (CRLs)](/kb/en/secure-connections-overview/#certificate-revocation-lists-crls) for more information.
</li><li>This variable is only supported if the server was built with OpenSSL. If the server was built with [wolfSSL](https://www.wolfssl.com/products/wolfssl/) or [yaSSL](https://www.wolfssl.com/products/yassl/), then this variable is not supported. See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb) for more information about which libraries are used on which platforms.
</li></ul>
- <strong>Commandline:</strong> `--ssl-crlpath=name`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `directory name`
- <strong>Default Value:</strong> None
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `ssl_key`

- <strong>Description:</strong> Defines a path to a private key file to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption). This system variable requires that you use the absolute path, not a relative path. This system variable implies the <a undefined>ssl</a> option.
- <strong>Commandline:</strong> `--ssl-key=name`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> None

---

#### `tls_version`

- <strong>Description:</strong> This system variable accepts a comma-separated list of TLS protocol versions. A TLS protocol version will only be enabled if it is present in this list. All other TLS protocol versions will not be permitted.
<ul start="1"><li>See [Secure Connections Overview: TLS Protocol Versions](/kb/en/secure-connections-overview/#tls-protocol-versions) for more information.
</li></ul>
- <strong>Commandline:</strong> `--tls-version=name`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `TLSv1.1`,`TLSv1.2`,`TLSv1.3`
- <strong>Valid Values:</strong> `TLSv1.0`,`TLSv1.1`,`TLSv1.2`,`TLSv1.3`
- <strong>Introduced:</strong> [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/)

---

#### `version_ssl_library`

- <strong>Description:</strong> The version of the [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption) library that is being used. Note that the version returned by this system variable does not always necessarily correspond to the exact version of the OpenSSL package installed on the system. OpenSSL shared libraries tend to contain interfaces for multiple versions at once to allow for backward compatibility. Therefore, if the OpenSSL package installed on the system is newer than the OpenSSL version that the MariaDB server binary was built with, then the MariaDB server binary might use one of the interfaces for an older version.
<ul start="1"><li>See [TLS and Cryptography Libraries Used by MariaDB: Checking the Server's OpenSSL Version](/kb/en/tls-and-cryptography-libraries-used-by-mariadb/#checking-the-servers-openssl-version) for more information.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> None
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

## See Also

- [Secure Connections Overview](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview)
- [System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables) for a complete list of system variables and instructions on setting them.
- [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables)