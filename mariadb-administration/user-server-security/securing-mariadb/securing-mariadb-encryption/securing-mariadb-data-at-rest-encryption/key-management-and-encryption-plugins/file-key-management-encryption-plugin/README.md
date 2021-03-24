# File Key Management Encryption Plugin

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

The File Key Management plugin was first released in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/).

MariaDB's [data-at-rest encryption](/kb/en/data-at-rest-encryption/) requires the use of a [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/). These plugins are responsible both for the management of encryption keys and for the actual encryption and decryption of data.

MariaDB supports the use of [multiple encryption keys](/kb/en/encryption-key-management/#using-multiple-encryption-keys). Each encryption key uses a 32-bit integer as a key identifier. If the specific plugin supports [key rotation](/kb/en/encryption-key-management/#key-rotation), then encryption keys can also be rotated, which creates a new version of the encryption key.

The File Key Management plugin that ships with MariaDB is a [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) that reads encryption keys from a plain-text file.

## Overview

The File Key Management plugin is the easiest [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) to set up for users who want to use [data-at-rest encryption](/kb/en/data-at-rest-encryption/). Some of the plugin's primary features are:

- It reads encryption keys from a plain-text key file.
- As an extra protection mechanism, the plain-text key file can be encrypted.
- It supports multiple encryption keys.
- It does <strong>not</strong> support key rotation.
- It supports two different algorithms for encrypting data.

It can also serve as an example and as a starting point when developing a key management and encryption plugin with the [encryption plugin API](/kb/en/encryption-plugin-api/).

## Installing the File Key Management Plugin's Package

The File Key Management plugin is included in MariaDB packages as the `file_key_management.so` or `file_key_management.dll` shared library. The shared library is in the main server package, so no additional package installations are necessary. The plugin was first included in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/).

## Installing the Plugin

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default. The plugin can be installed by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = file_key_management
```

## Uninstalling the Plugin

Before you uninstall the plugin, you should ensure that [data-at-rest encryption](/kb/en/data-at-rest-encryption/) is completely disabled, and that MariaDB no longer needs the plugin to decrypt tables or other files.

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'file_key_management';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Creating the Key File

In order to encrypt your tables with encryption keys using the File Key Management plugin, you first need to create the file that contains the encryption keys. The file needs to contain two pieces of information for each encryption key. First, each encryption key needs to be identified with a 32-bit integer as the key identifier. Second, the encryption key itself needs to be provided in hex-encoded form. These two pieces of information need to be separated by a semicolon. For example, the file is formatted in the following way:

```sql
<encryption_key_id1>;<hex-encoded_encryption_key1>
<encryption_key_id2>;<hex-encoded_encryption_key2>
```

You can also optionally encrypt the key file to make it less accessible from the file system. That is explained further in the section below.

The File Key Management plugin uses [Advanced Encryption Standard (AES)](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) to encrypt data, which supports 128-bit, 192-bit, and 256-bit encryption keys. Therefore, the plugin also supports 128-bit, 192-bit, and 256-bit encryption keys.

You can generate random encryption keys using the <a undefined>openssl rand</a> command. For example, to create a random 256-bit (32-byte) encryption key, you would run the following command:

```sql
$ openssl rand -hex 32
a7addd9adea9978fda19f21e6be987880e68ac92632ca052e5bb42b1a506939a
```

You can copy this encryption key to the key file using a text editor, or you can append a series of keys to a new key file. For example, to append three new encryption keys to a new key file, you could execute the following:

```sql
$ sudo openssl rand -hex 32 >> /etc/mysql/encryption/keyfile
$ sudo openssl rand -hex 32 >> /etc/mysql/encryption/keyfile
$ sudo openssl rand -hex 32 >> /etc/mysql/encryption/keyfile
```

The new key file would look something like the following after this step:

```sql
a7addd9adea9978fda19f21e6be987880e68ac92632ca052e5bb42b1a506939a
49c16acc2dffe616710c9ba9a10b94944a737de1beccb52dc1560abfdd67388b
8db1ee74580e7e93ab8cf157f02656d356c2f437d548d5bf16bf2a56932954a3
```

The key file still needs to have a key identifier for each encryption key added to the beginning of each line. Key identifiers do not need to be contiguous. Open the new key file in your preferred text editor and add the key identifiers. For example, the key file would look something like the following after this step:

```sql
1;a7addd9adea9978fda19f21e6be987880e68ac92632ca052e5bb42b1a506939a
2;49c16acc2dffe616710c9ba9a10b94944a737de1beccb52dc1560abfdd67388b
100;8db1ee74580e7e93ab8cf157f02656d356c2f437d548d5bf16bf2a56932954a3
```

The key identifiers give you a way to reference the encryption keys from MariaDB. In the example above, you could reference these encryption keys using the key identifiers `1`, `2` or `100` with the <a undefined>ENCRYPTION_KEY_ID</a> table option or with system variables such as <a undefined>innodb_default_encryption_key_id</a>. You do not necessarily need multiple encryption keys--the encryption key with the key identifier `1` is the only mandatory encryption key.

### Configuring the Path to an Unencrypted Key File

If the key file is unencrypted, then the File Key Management plugin only requires the <a undefined>file_key_management_filename</a> system variable to be configured.

This system variable can be specified as command-line arguments to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
loose_file_key_management_filename = /etc/mysql/encryption/keyfile
```

Note that the <a undefined>loose</a> option prefix is specified. This option prefix is used in case the plugin hasn't been installed yet.

## Encrypting the Key File

By enabling the File Key Management plugin and setting the appropriate path on the <a undefined>file_key_management_filename</a> system variable, you can begin using the plugin to manage your encryption keys.  But, there is a security risk in doing so, given that the keys are stored in plain text on your system.  You can reduce this exposure using file permissions, but it's better to encrypt the whole key file to further restrict access.

There are some important details to keep in mind about encrypting the key file, such as:

- The only algorithm that MariaDB currently supports to encrypt the key file is [Cipher Block Chaining (CBC)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CBC) mode of [Advanced Encryption Standard (AES)](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).
- The encryption key size can be 128-bits, 192-bits, or 256-bits.
- The encryption key is created from the [SHA-1](https://en.wikipedia.org/wiki/SHA-1) hash of the encryption password.
- The encryption password has a max length of 256 characters.

You can generate a random encryption password using the <a undefined>openssl rand</a> command. For example, to create a random 256 character encryption password, you could execute the following:

```sql
$ sudo openssl rand -hex 128 > /etc/mysql/encryption/keyfile.key
```

You can encrypt the key file using the <a undefined>openssl enc</a> command. For example, to encrypt the key file with the encryption password created in the previous step, you could execute the following:

```sql
$ sudo openssl enc -aes-256-cbc -md sha1 \
   -pass file:/etc/mysql/encryption/keyfile.key \
   -in /etc/mysql/encryption/keyfile \
   -out /etc/mysql/encryption/keyfile.enc
```

Running this command reads the unencrypted `keyfile` file created above and creates a new encrypted `keyfile.enc` file, using the encryption password stored in `keyfile.key`.  Once you've finished preparing your system, you can delete the unencrypted `keyfile` file, as it's no longer necessary.

### Configuring the Path to an Encrypted Key File

If the key file is encrypted, then the File Key Management plugin requires both the <a undefined>file_key_management_filename</a>  and the <a undefined>file_key_management_filekey</a> system variables to be configured.

The <a undefined>file_key_management_filekey</a> system variable can be provided in two forms:

- It can be the actual plain-text encryption password. This is not recommended, since the plain-text encryption password would be visible in the output of the [SHOW VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables/) statement.
- If it is prefixed with `FILE:`, then it can be a path to a file that contains the plain-text encryption password.

These system variables can be specified as command-line arguments to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or they can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
loose_file_key_management_filename = /etc/mysql/encryption/keyfile.enc
loose_file_key_management_filekey = FILE:/etc/mysql/encryption/keyfile.key
```

Note that the <a undefined>loose</a> option prefix is specified. This option prefix is used in case the plugin hasn't been installed yet.

## Choosing an Encryption Algorithm

The File Key Management plugin currently supports two encryption algorithms for encrypting data: `AES_CBC` and `AES_CTR`. Both of these algorithms use [Advanced Encryption Standard (AES)](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) in different modes. AES uses 128-bit blocks, and supports 128-bit, 192-bit, and 256-bit keys. The modes are:

- The `AES_CBC` mode uses AES in the [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher_Block_Chaining_.28CBC.29) mode.
- The `AES_CTR` mode uses AES in two slightly different modes in different contexts. When encrypting tablespace pages (such as pages in InnoDB, XtraDB, and Aria tables), it uses AES in the 
[Counter (CTR)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Counter_.28CTR.29) mode. When encrypting temporary files (where the cipher text is allowed to be larger than the plain text), it uses AES in the authenticated [Galois/Counter Mode (GCM)](http://en.wikipedia.org/wiki/Galois/Counter_Mode).

The recommended algorithm is `AES_CTR`, but this algorithm is only available when MariaDB is built with recent versions of [OpenSSL](https://www.openssl.org/). If the server is built with [wolfSSL](https://www.wolfssl.com/products/wolfssl/) or [yaSSL](https://www.wolfssl.com/products/yassl/), then this algorithm is not available. See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb/) for more information about which libraries are used on which platforms.

### Configuring the Encryption Algorithm

The encryption algorithm can be configured by setting the <a undefined>file_key_management_encryption_algorithm</a> system variable.

This system variable can be set to one of the following values:

<table><tbody><tr><th>System Variable Value</th><th>Description</th></tr>
<tr><td><code>AES_CBC</code></td><td>Data is encrypted using AES in the <a href="http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher_Block_Chaining_.28CBC.29">Cipher Block Chaining (CBC)</a> mode. This is the default value.</td></tr>
<tr><td><code>AES_CTR</code></td><td>Data is encrypted using AES either in the <a href="http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Counter_.28CTR.29">Counter (CTR)</a> mode or in the authenticated <a href="http://en.wikipedia.org/wiki/Galois/Counter_Mode">Galois/Counter Mode (GCM)</a> mode, depending on context. This is only supported in some builds. See the previous section for more information.</td></tr>
</tbody></table>

This system variable can be specified as command-line arguments to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
loose_file_key_management_encryption_algorithm = AES_CTR
```

Note that the <a undefined>loose</a> option prefix is specified. This option prefix is used in case the plugin hasn't been installed yet.

Note that this variable does not affect the algorithm that MariaDB uses to decrypt the key file. This variable only affects the encryption algorithm that MariaDB uses to encrypt and decrypt data. The only algorithm that MariaDB currently supports to encrypt the key file is [Cipher Block Chaining (CBC)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CBC) mode of [Advanced Encryption Standard (AES)](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).

## Using the File Key Management Plugin

Once the File Key Management Plugin is enabled, you can use it by creating an encrypted table:

```sql
CREATE TABLE t (i int) ENGINE=InnoDB ENCRYPTED=YES
```

Now, table `t` will be encrypted using the encryption key from the key file.

For more information on how to use encryption, see [Data at Rest Encryption](/kb/en/data-at-rest-encryption/).

## Using Multiple Encryption Keys

The File Key Management Plugin supports [using multiple encryption keys](/kb/en/encryption-key-management/#using-multiple-encryption-keys). Each encryption key can be defined with a different 32-bit integer as a key identifier.

When [encrypting InnoDB tables](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/), the key that is used to encrypt tables [can be changed](/kb/en/innodb-xtradb-encryption-keys/).

When [encrypting Aria tables](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/aria-encryption/), the key that is used to encrypt tables [cannot currently be changed](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/aria-encryption/aria-encryption-keys/).

## Key Rotation

The File Key Management plugin does not currently support [key rotation](/kb/en/encryption-key-management/#key-rotation). See [MDEV-20713](https://jira.mariadb.org/browse/MDEV-20713) for more information.

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10118-release-notes/">MariaDB 10.1.18</a></td></tr>
<tr><td>1.0</td><td>Gamma</td><td><a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a></td></tr>
<tr><td>1.0</td><td>Alpha</td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a></td></tr>
</tbody></table>

## System Variables

### `file_key_management_encryption_algorithm`

- <strong>Description:</strong> This system variable is used to determine which algorithm the plugin will use to encrypt data.
<ul start="1"><li>The recommended algorithm is `AES_CTR`, but this algorithm is only available when MariaDB is built with recent versions of [OpenSSL](https://www.openssl.org/). If the server is built with [wolfSSL](https://www.wolfssl.com/products/wolfssl/) or [yaSSL](https://www.wolfssl.com/products/yassl/), then this algorithm is not available. See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb/) for more information about which libraries are used on which platforms.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--file-key-management-encryption-algorithm=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `AES_CBC`
- <strong>Valid Values:</strong> `AES_CBC`, `AES_CTR`

---

### `file_key_management_filekey`

- <strong>Description:</strong> This system variable is used to determine the encryption password that is used to decrypt the key file defined by <a undefined>file_key_management_filename</a>.
<ul start="1"><li>If this system variable's value is prefixed with `FILE:`, then it is interpreted as a path to a file that contains the plain-text encryption password.
</li><li>If this system variable's value is <strong>not</strong> prefixed with `FILE:`, then it is interpreted as the plain-text encryption password. However, this is not recommended.
</li><li>The encryption password has a max length of 256 characters.
</li><li>The only algorithm that MariaDB currently supports when decrypting the key file is [Cipher Block Chaining (CBC)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CBC) mode of [Advanced Encryption Standard (AES)](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard). The encryption key size can be 128-bits, 192-bits, or 256-bits. The encryption key is calculated by taking a [SHA-1](https://en.wikipedia.org/wiki/SHA-1) hash of the encryption password. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--file-key-management-filekey=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (empty)

---

### `file_key_management_filename`

- <strong>Description:</strong> This system variable is used to determine the path to the file that contains the encryption keys. If <a undefined>file_key_management_filekey</a> is set, then this file can be encrypted with [Cipher Block Chaining (CBC)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CBC) mode of [Advanced Encryption Standard (AES)](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--file-key-management-filename=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (empty)

---

## Options

### `file_key_management`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the [mysql.plugins](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlplugin-table/) table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--file-key-management=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---