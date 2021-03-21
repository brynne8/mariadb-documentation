# TLS and Cryptography Libraries Used by MariaDB

When MariaDB Server is compiled with TLS and cryptography support, it is usually either statically linked with MariaDB's bundled TLS and cryptography library or dynamically linked with the system's [OpenSSL](https://www.openssl.org/) library. MariaDB's bundled TLS library is either [wolfSSL](https://www.wolfssl.com/products/wolfssl/) or [yaSSL](https://www.wolfssl.com/products/yassl/), depending on the server version.

When a MariaDB client or client library is compiled with TLS and cryptography support, it is usually either statically linked with MariaDB's bundled TLS and cryptography library or dynamically linked with the system's TLS and cryptography library, which might be [OpenSSL](https://www.openssl.org/), [GnuTLS](https://www.gnutls.org/), or [Schannel](https://docs.microsoft.com/en-us/windows/desktop/secauthn/secure-channel).

## Checking Dynamically vs. Statically Linked

Dynamically linking MariaDB to the system's TLS and cryptography library can often be beneficial, since this allows you to fix bugs in the system's TLS and cryptography library independently of MariaDB. For example, when information on the [Heartbleed Bug](http://heartbleed.com/) in [OpenSSL](https://www.openssl.org/) was released in 2014, the bug could be mitigated by simply updating your system to use a fixed version of the [OpenSSL](https://www.openssl.org/) library, and then restarting the MariaDB Server.

You can verify that `mysqld` is in fact dynamically linked to the [OpenSSL](https://www.openssl.org/) shared library on your system by using the <a undefined>ldd</a> command:

```sql
$ ldd $(which mysqld) | grep -E '(libssl|libcrypto)'
        libssl.so.10 => /lib64/libssl.so.10 (0x00007f8736386000)
        libcrypto.so.10 => /lib64/libcrypto.so.10 (0x00007f8735f25000)
```

If the command does not return any results, then either your `mysqld` is statically linked to the TLS and cryptography library on your system or your `mysqld` is not built with TLS and cryptography support at all.

## Checking If the Server Uses OpenSSL

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later, if you aren't sure whether your server is linked with [OpenSSL](https://www.openssl.org/) or the bundled TLS library, then you can check the value of the <a undefined>have_openssl</a> system variable. For example:

```sql
SHOW GLOBAL VARIABLES LIKE 'have_openssl';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| have_openssl  | YES   |
+---------------+-------+
```

## Checking the Server's OpenSSL Version

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, if you want to see what version of [OpenSSL](https://www.openssl.org/) your server is using, then you can check the value of the <a undefined>version_ssl_library</a> system variable. For example:

```sql
SHOW GLOBAL VARIABLES LIKE 'version_ssl_library';
+---------------------+---------------------------------+
| Variable_name       | Value                           |
+---------------------+---------------------------------+
| version_ssl_library | OpenSSL 1.0.1e-fips 11 Feb 2013 |
+---------------------+---------------------------------+
```

Note that the version returned by this system variable does not always necessarily correspond to the exact version of the [OpenSSL](https://www.openssl.org/) package installed on the system. [OpenSSL](https://www.openssl.org/) shared libraries tend to contain interfaces for multiple versions at once to allow for backward compatibility. Therefore, if the [OpenSSL](https://www.openssl.org/) package installed on the system is newer than the [OpenSSL](https://www.openssl.org/) version that the MariaDB Server binary was built with, then the MariaDB Server binary might use one of the interfaces for an older version. See [MDEV-15848](https://jira.mariadb.org/browse/MDEV-15848) for more information. For example:

```sql
$ cat /etc/redhat-release
Red Hat Enterprise Linux Server release 7.5 (Maipo)
$ rpm -q openssl
openssl-1.0.2k-12.el7.x86_64
$ mysql -u root --batch --execute="SHOW GLOBAL VARIABLES LIKE 'version_ssl_library';"
Variable_name   Value
version_ssl_library     OpenSSL 1.0.1e-fips 11 Feb 2013
$ ldd $(which mysqld) | grep libcrypto
        libcrypto.so.10 => /lib64/libcrypto.so.10 (0x00007f3dd3482000)
$ readelf -a /lib64/libcrypto.so.10 | grep SSLeay_version
  1374: 000000000006f5d0    21 FUNC    GLOBAL DEFAULT   13 SSLeay_version@libcrypto.so.10
  1375: 000000000006f5f0    21 FUNC    GLOBAL DEFAULT   13 SSLeay_version@OPENSSL_1.0.1
  1377: 000000000006f580    70 FUNC    GLOBAL DEFAULT   13 SSLeay_version@@OPENSSL_1.0.2
```

## FIPS Certification

[Federal Information Processing Standards (FIPS)](https://www.nist.gov/itl/itl-publications/federal-information-processing-standards-fips) are standards published by the U.S. federal government that are used to establish requirements for various aspects of computer systems. [FIPS 140-2](https://www.nist.gov/publications/security-requirements-cryptographic-modules-includes-change-notices-1232002?pub_id=902003) is a set of standards for security requirements for cryptographic modules.

This standard is relevant when discussing the TLS and cryptography libraries used by MariaDB. Some of these libraries have been certified to meet the standards set by FIPS 140-2.

### FIPS Certification by OpenSSL

The [OpenSSL](https://www.openssl.org/) library has a special FIPS mode that has been certified to meet the FIPS 140-2 standard. In FIPS mode, only algorithms and key sizes that meet the FIPS 140-2 standard are enabled by the library.

MariaDB does not yet support enabling FIPS mode within the database server. See [MDEV-20260](https://jira.mariadb.org/browse/MDEV-20260) for more information. Therefore, if you would like to use OpenSSL's FIPS mode with MariaDB, then you would need to enable FIPS mode at the kernel level. See the following resources for more information on how to do that:

- [Red Hat Enterprise Linux 7: Security Guide: Chapter 8. Federal Standards and Regulations](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/chap-federal_standards_and_regulations)
- [Ubuntu Security Certifications Documentation: FIPS for Ubuntu 16.04 and 18.04](https://security-certs.docs.ubuntu.com/en/fips)
- [Ubuntu Security Certifications Documentation: Ubuntu FIPS 140-2 Modules FAQ](https://docs.ubuntu.com/security-certs/en/fips-faq)

### FIPS Certification by wolfSSL

The standard version of the [wolfSSL](https://www.wolfssl.com/products/wolfssl/) library has not been certified to meet the FIPS 140-2 standard, but a special ["FIPS-ready"](https://www.wolfssl.com/wolfssl-fips-ready/) version has been certified. Unfortunately, the "FIPS-ready" version of wolfSSL uses a license that is incompatible with MariaDB's license, so it cannot be used with MariaDB.

### FIPS Certification by yaSSL

The [yaSSL](https://www.wolfssl.com/products/yassl/) library has not been certified to meet the FIPS 140-2 standard.

## Libraries Used by Each Platform and Package

### MariaDB Server

#### MariaDB Server on Windows

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

MariaDB Server is statically linked with the bundled [wolfSSL](https://www.wolfssl.com/products/wolfssl/)  library in [MSI](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-msi-packages-on-windows/) and [ZIP](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-windows-zip-packages/) packages on Windows.

##### MariaDB until [10.4.5](/kb/en/mariadb-1045-release-notes/)

MariaDB Server is statically linked with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) library in [MSI](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-msi-packages-on-windows/) and [ZIP](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-windows-zip-packages/) packages on Windows.

#### MariaDB Server on Linux

##### MariaDB Server in Binary Tarballs

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

In [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/) and later, MariaDB Server is statically linked with the bundled [wolfSSL](https://www.wolfssl.com/products/wolfssl/) library in [binary tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs/) on Linux.

##### MariaDB until [10.4.5](/kb/en/mariadb-1045-release-notes/)

In [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/) and before, MariaDB Server is statically linked with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) library in [binary tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs/) on Linux.

##### MariaDB Server in DEB Packages

MariaDB Server is dynamically linked with the system's [OpenSSL](https://www.openssl.org/) library in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files/) packages provided by MariaDB Foundation and MariaDB Corporation.

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

In [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/) and later, MariaDB Server is statically linked with the bundled [wolfSSL](https://www.wolfssl.com/products/wolfssl/) library in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files/) packages provided by Debian's and Ubuntu's default repositories.

##### MariaDB until [10.4.5](/kb/en/mariadb-1045-release-notes/)

In [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/) and before, MariaDB Server is statically linked with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) library in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files/) packages provided by Debian's and Ubuntu's default repositories.

See [Differences in MariaDB in Debian (and Ubuntu)](/mariadb-administration/getting-installing-and-upgrading-mariadb/troubleshooting-installation-issues/installation-issues-on-debian-and-ubuntu/differences-in-mariadb-in-debian-and-ubuntu/) for more information.

##### MariaDB Server in RPM Packages

MariaDB Server is dynamically linked with the system's [OpenSSL](https://www.openssl.org/) library in [.rpm](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/) packages.

### MariaDB Clients and Utilities

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, [MariaDB Connector/C](/kb/en/mariadb-connector-c/) has been [included with MariaDB Server](/kb/en/about-mariadb-connector-c/#integration-with-mariadb-server), and the bundled and the [clients and utilities](/clients-utilities/) are linked with it. On some platforms, [MariaDB Connector/C](/kb/en/mariadb-connector-c/) and these [clients and utilities](/clients-utilities/) may use a different TLS library than the one used by MariaDB Server and [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html).

#### MariaDB Clients and Utilities on Windows

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

In [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/) and later, MariaDB's [clients and utilities](/clients-utilities/) and [MariaDB Connector/C](/kb/en/mariadb-connector-c/)  are are dynamically linked with the system's [Schannel](https://docs.microsoft.com/en-us/windows/desktop/secauthn/secure-channel) libraries in [MSI](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-msi-packages-on-windows/) and [ZIP](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-windows-zip-packages/) packages on Windows. [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) is still statically linked with the bundled [wolfSSL](https://www.wolfssl.com/products/wolfssl/) library.

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, MariaDB's [clients and utilities](/clients-utilities/) and [MariaDB Connector/C](/kb/en/mariadb-connector-c/)  are are dynamically linked with the system's [Schannel](https://docs.microsoft.com/en-us/windows/desktop/secauthn/secure-channel) libraries in [MSI](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-msi-packages-on-windows/) and [ZIP](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-windows-zip-packages/) packages on Windows. [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) is still statically linked with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) library.

##### MariaDB until [10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, MariaDB's [clients and utilities](/clients-utilities/) and [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) are statically linked with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) library in [MSI](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-msi-packages-on-windows/) and [ZIP](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-windows-zip-packages/) packages on Windows.

#### MariaDB Clients and Utilities on Linux

##### MariaDB Clients and Utilities in Binary Tarballs

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

In [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/) and later, MariaDB's [clients and utilities](/clients-utilities/) and [MariaDB Connector/C](/kb/en/mariadb-connector-c/) are statically linked with the [GnuTLS](https://www.gnutls.org/) library in [binary tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs/) on Linux. [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) is still statically linked with the bundled [wolfSSL](https://www.wolfssl.com/products/wolfssl/) library.

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, MariaDB's [clients and utilities](/clients-utilities/) and [MariaDB Connector/C](/kb/en/mariadb-connector-c/) are statically linked with the [GnuTLS](https://www.gnutls.org/) library in [binary tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs/) on Linux. [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) is still statically linked with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) library.

##### MariaDB until [10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, MariaDB's [clients and utilities](/clients-utilities/) and [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) are statically linked with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) library in [binary tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs/) on Linux.

##### MariaDB Clients and Utilities in DEB Packages

MariaDB's [clients and utilities](/clients-utilities/), [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html), and [MariaDB Connector/C](/kb/en/mariadb-connector-c/) are dynamically linked with the system's [OpenSSL](https://www.openssl.org/) library in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files/) packages provided by MariaDB Foundation's and MariaDB Corporation's repositories.

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

In [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/) and later, MariaDB's [clients and utilities](/clients-utilities/) and [MariaDB Connector/C](/kb/en/mariadb-connector-c/) are dynamically linked with the system's [GnuTLS](https://www.gnutls.org/) library in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files/) packages provided by Debian's and Ubuntu's default repositories. [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) is still statically linked with the bundled [wolfSSL](https://www.wolfssl.com/products/wolfssl/) libraries.

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, MariaDB's [clients and utilities](/clients-utilities/) and [MariaDB Connector/C](/kb/en/mariadb-connector-c/) are dynamically linked with the system's [GnuTLS](https://www.gnutls.org/) library in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files/) packages provided by Debian's and Ubuntu's default repositories. [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) is still statically linked with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) libraries.

##### MariaDB until [10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and earlier, MariaDB's [clients and utilities](/clients-utilities/) and [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) are statically linked with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) library in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files/) packages provided by Debian's and Ubuntu's default repositories.

See [Differences in MariaDB in Debian (and Ubuntu)](/mariadb-administration/getting-installing-and-upgrading-mariadb/troubleshooting-installation-issues/installation-issues-on-debian-and-ubuntu/differences-in-mariadb-in-debian-and-ubuntu/) for more information.

##### MariaDB Clients and Utilities in RPM Packages

MariaDB's [clients and utilities](/clients-utilities/), [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html), and [MariaDB Connector/C](/kb/en/mariadb-connector-c/)  are dynamically linked with the system's [OpenSSL](https://www.openssl.org/) library in [.rpm](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/) packages.

## Updating Dynamically Linked OpenSSL Libraries on Linux

When the MariaDB Server or clients and utilities are dynamically linked to the system's [OpenSSL](https://www.openssl.org/) library, it makes it very easy to update the libraries. The information below will show how to update these libraries for each platform.

### Updating Dynamically Linked OpenSSL Libraries with yum/dnf

On RHEL, CentOS, Fedora, and other similar Linux distributions, it is highly recommended to update the libraries using [yum](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum/) or <a undefined>dnf</a>. Starting with RHEL 8 and Fedora 22, `yum` has been replaced by `dnf`, which is the next major version of `yum`. However, `yum` commands still work on many systems that use `dnf`. For example:

Update the package by executing the following command:

```sql
sudo yum update openssl
```

And then [restart](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/) MariaDB server and any clients or applications that use the library.

### Updating Dynamically Linked OpenSSL Libraries with apt-get

On Debian, Ubuntu, and other similar Linux distributions, it is highly recommended to recommended to update the libraries using <a undefined>apt-get</a>. For example:

First update the package cache by executing the following command:

```sql
sudo apt update
```

And then update the package by executing the following command:

```sql
sudo apt-get update openssl
```

And then [restart](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/) MariaDB server and any clients or applications that use the library.

### Updating Dynamically Linked OpenSSL Libraries with zypper

On SLES, OpenSUSE, and other similar Linux distributions, it is highly recommended to recommended to update the libraries using [zypper](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/installing-mariadb-with-zypper/). For example:

Update the package by executing the following command:

```sql
sudo zypper update openssl
```

And then [restart](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/) MariaDB server and any clients or applications that use the library.