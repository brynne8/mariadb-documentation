# Eperi Key Management Encryption Plugin

MariaDB's [data-at-rest encryption](/kb/en/data-at-rest-encryption/) requires the use of a [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/). These plugins are responsible both for the management of encryption keys and for the actual encryption and decryption of data.

MariaDB supports the use of [multiple encryption keys](/kb/en/encryption-key-management/#using-multiple-encryption-keys). Each encryption key uses a 32-bit integer as a key identifier. If the specific plugin supports [key rotation](/kb/en/encryption-key-management/#rotating-keys), then encryption keys can also be rotated, which creates a new version of the encryption key.

The Eperi Key Management plugin is a [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) that integrates with [eperi Gateway for Databases](https://eperi.com/database-encryption/).

## Overview

The Eperi Key Management plugin is one of the [key management and encryption plugins](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) that can be set up for users who want to use [data-at-rest encryption](/kb/en/data-at-rest-encryption/). Some of the plugin's primary features are:

- It reads encryption keys from [eperi Gateway for Databases](https://eperi.com/database-encryption/).
- It supports multiple encryption keys.
- It supports key rotation.
- It supports two different algorithms for encrypting data.

The [eperi Gateway for Databases](https://eperi.com/database-encryption/) stores encryption keys on the key server outside of the database server itself, which provides an extra level of security. The [eperi Gateway for Databases](https://eperi.com/database-encryption/) also supports performing all data encryption operations on the key server as well, but this is optional.

It also provides the following benefits:

- Key management outside the database
- No keys on database server hard disk
- Graphical user interface for configuration
- Encryption and decryption outside the database, supporting HSM's for maximum security.

Support for MariaDB is provided in [eperi Gateway for Databases 3.4](https://eperi.com/eperi-gateway-for-databases-version-3-4-offers-native-mariadb-support/).

## Installing the Eperi Key Management Plugin's Package

For information on how to install the package, see Eperi's documentation at the [Eperi Customer Portal](https://customer.eperi.de/index.jsp).

## Installing the Plugin

Even after the package that contains the plugin's shared library is installed on the operating system, the plugin is not actually installed by MariaDB by default. The plugin can be installed by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = eperi_key_management_plugin
```

## Uninstalling the Plugin

Before you uninstall the plugin, you should ensure that [data-at-rest encryption](/kb/en/data-at-rest-encryption/) is completely disabled, and that MariaDB no longer needs the plugin to decrypt tables or other files.

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'eperi_key_management_plugin';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Configuring the Eperi Key Management Plugin

For information on how to configure the plugin, see Eperi's documentation at the [Eperi Customer Portal](https://customer.eperi.de/index.jsp).

## Using the Eperi Key Management Plugin

Once the Eperi Key Management Plugin is enabled, you can use it by creating an encrypted table:

```sql
CREATE TABLE t (i int) ENGINE=InnoDB ENCRYPTED=YES
```

Now, table `t` will be encrypted using the encryption key from the key server.

For more information on how to use encryption, see [Data at Rest Encryption](/kb/en/data-at-rest-encryption/).

## Using Multiple Encryption Keys

The Eperi Key Management Plugin supports [using multiple encryption keys](/kb/en/encryption-key-management/#using-multiple-encryption-keys). Each encryption key can be defined with a different 32-bit integer as a key identifier.

When [encrypting InnoDB tables](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/), the key that is used to encrypt tables [can be changed](/kb/en/innodb-xtradb-encryption-keys/).

When [encrypting Aria tables](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/aria-encryption/), the key that is used to encrypt tables [cannot currently be changed](/kb/en/aria-encryption-keys/).

## Key Rotation

The Eperi Key Management plugin supports [key rotation](/kb/en/encryption-key-management/#key-rotation).

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.0</td><td>Unknown</td><td><a href="https://eperi.com/database-encryption/">eperi Gateway for Databases</a> 3.4.0</td></tr>
</tbody></table>

## System Variables

### `eperi_key_management_plugin_databaseId`

- <strong>Description:</strong> Determines the database ID which is processed in the eperi Gateway.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--eperi-key-management-plugin-databaseid=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `integer`
- <strong>Default Value:</strong> `1`

---

### `eperi_key_management_plugin_encryption_algorithm`

- <strong>Description:</strong> This system variable is used to determine which algorithm the plugin will use to encrypt data.
<ul start="1"><li>The recommended algorithm is `AES_CTR`, but this algorithm is only available when MariaDB is built with recent versions of [OpenSSL](https://www.openssl.org/). If the server is built with [wolfSSL](https://www.wolfssl.com/products/wolfssl/) or [yaSSL](https://www.wolfssl.com/products/yassl/), then this algorithm is not available. See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb/) for more information about which libraries are used on which platforms.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--eperi-key-management-plugin-encryption-algorithm=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `AES_CBC`
- <strong>Valid Values:</strong> `AES_CBC`, `AES_CTR`

---

### `eperi_key_management_plugin_encryption_mode`

- <strong>Description:</strong> Encryption mode.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--eperi-key-management-plugin-encryption-mode=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `database`
- <strong>Valid Values:</strong> `database`, `gateway`

---

### `eperi_key_management_plugin_osslmt`

- <strong>Description:</strong> Determines, whether the plugin should register callback functions for OpenSSL thread support.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--eperi-key-management-plugin-osslmt=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0` (Linux), `1` (Windows)

---

### `eperi_key_management_plugin_port`

- <strong>Description:</strong> Listener port for plugin.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--eperi-key-management-plugin-port=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `integer`
- <strong>Default Value:</strong> `14332`

---

### `eperi_key_management_plugin_url`

- <strong>Description:</strong> URL to key server. The expected format of the URL is &lt;host&gt;:&lt;port&gt;. The port number is optional, and the port number defaults to 14333 if it is not specified.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--eperi-key-management-plugin-url=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `NULL`

---

### `eperi_key_management_plugin_url_check_disabled`

- <strong>Description:</strong> Determines, whether the connection between plugin and eperi Gateway is tested at server startup.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--eperi-key-management-plugin-url-check-disabled=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`

---

## Options

### `eperi_key_management_plugin`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the <a undefined>mysql.plugins</a> table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--eperi-key-management-plugin=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---

## See Also

- [Database Encryption - eperi](https://eperi.com/database-encryption/)
- [eperi Gateway for Databases version 3.4 offers native MariaDB support](https://eperi.com/eperi-gateway-for-databases-version-3-4-offers-native-mariadb-support/)
- [eperi Customer Portal](https://customer.eperi.de/index.jsp)