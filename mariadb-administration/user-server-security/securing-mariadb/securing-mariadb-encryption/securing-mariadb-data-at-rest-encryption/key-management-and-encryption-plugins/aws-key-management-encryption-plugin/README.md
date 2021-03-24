# AWS Key Management Encryption Plugin

##### MariaDB starting with [10.1.13](/kb/en/mariadb-10113-release-notes/)

The AWS Key Management plugin was first added to the [MariaDB source code](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/) in [MariaDB 10.1.13](/kb/en/mariadb-10113-release-notes/).

MariaDB's [data-at-rest encryption](/kb/en/data-at-rest-encryption/) requires the use of a [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/). These plugins are responsible both for the management of encryption keys and for the actual encryption and decryption of data.

MariaDB supports the use of [multiple encryption keys](/kb/en/encryption-key-management/#using-multiple-encryption-keys). Each encryption key uses a 32-bit integer as a key identifier. If the specific plugin supports [key rotation](/kb/en/encryption-key-management/#key-rotation), then encryption keys can also be rotated, which creates a new version of the encryption key.

The AWS Key Management plugin is a [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) that uses the [Amazon Web Services (AWS) Key Management Service (KMS)](https://aws.amazon.com/kms/).

## Overview

The AWS Key Management plugin uses the [Amazon Web Services (AWS) Key Management Service (KMS)](https://aws.amazon.com/kms/) to generate and store AES keys on disk, in encrypted form, using the Customer Master Key (CMK) kept in AWS KMS. When MariaDB Server starts, the plugin will decrypt the encrypted keys, using the AWS KMS "Decrypt" API function. MariaDB data will then be encrypted and decrypted using the AES key. It supports multiple encryption keys. It supports key rotation.

## Tutorials

Tutorials related to the AWS Key Management plugin can be found at the following pages:

- [Amazon Web Services (AWS) Key Management Service (KMS) Encryption Plugin Setup Guide](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/aws-key-management-encryption-plugin-setup-guide/)
- [Amazon Web Services (AWS) Key Management Service (KMS) Encryption Plugin Advanced Usage](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/aws-key-management-encryption-plugin-advanced-usage/)

## Preparation

- Before you use the plugin, you need to create a Customer Master Key (CMK). Create a key using the AWS Console as described in the [AMS KMS developer guide](http://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html).
- The easiest way to give the AWS key management plugin access to the key is to create an IAM Role with access to the key, and to apply that IAM Role to an EC2 instance where MariaDB Server runs.
- Make sure that MariaDB Server runs under the correct AWS identity that has access to the above key. For example, you can store the AWS credentials in a AWS credentials file for the user who runs `mysqld`. More information about the credentials file can be found in [the AWS CLI Getting Started Guide](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-config-file).

## Installing the Plugin's Package

The AWS Key Management plugin depends on the [AWS SDK for C++](https://github.com/aws/aws-sdk-cpp), which uses the [Apache License, Version 2.0](https://github.com/aws/aws-sdk-cpp/blob/master/LICENSE). This license is not compatible with MariaDB Server's [GPL 2.0 license](/kb/en/mariadb-license/), so we are not able to distribute packages that contain the AWS Key Management plugin. Therefore, the only way to currently obtain the plugin is to install it from source.

### Installing from Source

When [compiling MariaDB from source](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/), the AWS Key Management plugin is not built by default in [MariaDB 10.1](/kb/en/what-is-mariadb-101/), but it is built by default in [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, on systems that support it.

Compilation is controlled by the `-DPLUGIN_AWS_KEY_MANAGEMENT=DYNAMIC -DAWS_SDK_EXTERNAL_PROJECT=1` <a undefined>cmake</a> arguments.

The plugin uses [AWS C++ SDK](https://github.com/awslabs/aws-sdk-cpp), which introduces the following restrictions:

- The plugin can only be built on Windows, Linux and macOS.
- The plugin requires that one of the following compilers is used: `gcc` 4.8 or later, `clang` 3.3 or later, Visual Studio 2013 or later.
- On Unix, the `libcurl` development package (e.g. `libcurl3-dev` on Debian Jessie), `uuid` development package and `openssl` need to be installed.
- You may need to use a newer version of <a undefined>cmake</a> than is provided by default in your OS.

## Installing the Plugin

Even after the package that contains the plugin's shared library is installed on the operating system, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). For example:

```sql
INSTALL SONAME 'aws_key_management';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = aws_key_management
```

## Uninstalling the Plugin

Before you uninstall the plugin, you should ensure that [data-at-rest encryption](/kb/en/data-at-rest-encryption/) is completely disabled, and that MariaDB no longer needs the plugin to decrypt tables or other files.

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'aws_key_management';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Configuring the AWS Key Management Plugin

To enable the AWS Key Management plugin, you also need to set the plugin's system variables. The <a undefined>aws_key_management_master_key_id</a> system variable is the primary one to set. These system variables can be specified as command-line arguments to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or they can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
aws_key_management_master_key_id=alias/<your key's alias>
```

Once you've updated the configuration file, [restart](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/) the MariaDB server to apply the changes and make the key management and encryption plugin available for use.

## Using the AWS Key Management Plugin

Once the AWS Key Management Plugin is enabled, you can use it by creating an encrypted table:

```sql
CREATE TABLE t (i int) ENGINE=InnoDB ENCRYPTED=YES
```

Now, table `t` will be encrypted using the encryption key generated by AWS.

For more information on how to use encryption, see [Data at Rest Encryption](/kb/en/data-at-rest-encryption/).

## Using Multiple Encryption Keys

The AWS Key Management Plugin supports [using multiple encryption keys](/kb/en/encryption-key-management/#using-multiple-encryption-keys). Each encryption key can be defined with a different 32-bit integer as a key identifier. If a previously unused identifier is used, then the plugin will automatically generate a new key.

When [encrypting InnoDB tables](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/), the key that is used to encrypt tables [can be changed](/kb/en/innodb-xtradb-encryption-keys/).

When [encrypting Aria tables](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/aria-encryption/), the key that is used to encrypt tables [cannot currently be changed](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/aria-encryption/aria-encryption-keys/).

## Key Rotation

The AWS Key Management plugin does support [key rotation](/kb/en/encryption-key-management/#key-rotation). To rotate a key, set the <a undefined>aws_key_management_rotate_key</a> system variable. For example, to rotate key with ID 2:

```sql
SET GLOBAL aws_key_management_rotate_key=2;
```

Or to rotate all keys, set the value to -1:

```sql
SET GLOBAL aws_key_management_rotate_key=-1;
```

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-1026-release-notes/">MariaDB 10.2.6</a>, <a href="/kb/en/mariadb-10124-release-notes/">MariaDB 10.1.24</a></td></tr>
<tr><td>1.0</td><td>Beta</td><td><a href="/kb/en/mariadb-10118-release-notes/">MariaDB 10.1.18</a></td></tr>
<tr><td>1.0</td><td>Experimental</td><td><a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a></td></tr>
</tbody></table>

## System Variables

### `aws_key_management_key_spec`

- <strong>Description:</strong> Encryption algorithm used to create new keys
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aws-key-management-key-spec=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `AES_128`
- <strong>Valid Values:</strong> `AES_128`, `AES_256`

---

### `aws_key_management_log_level`

- <strong>Description:</strong> Dump log of the AWS SDK to MariaDB error log. Permitted values, in increasing verbosity, are <strong>Off</strong> (default), <strong>Fatal</strong>, <strong>Error</strong>, <strong>Warn</strong>, <strong>Info</strong>, <strong>Debug</strong>, and <strong>Trace</strong>.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aws-key-management-log-level=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `Off`
- <strong>Valid Values:</strong> `Off`, `Fatal`, `Warn`, `Info`, `Debug` and `Trace`

---

### `aws_key_management_master_key_id`

- <strong>Description:</strong> AWS KMS Customer Master Key ID (ARN or alias prefixed by alias/) for the master encryption key. Used to create new data keys. If not set, no new data keys will be created.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aws-key-management-master-key-id=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong>

---

### `aws_key_management_mock`

- <strong>Description:</strong> Mock AWS KMS calls (for testing). Must be enabled at compile-time.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aws-key-management-mock</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Valid Values:</strong> `OFF`, `ON`

---

### `aws_key_management_region`

- <strong>Description:</strong> AWS region name, e.g us-east-1 . Default is SDK default, which is us-east-1.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aws-key-management-region=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `'us-east-1'`

---

### `aws_key_management_request_timeout`

- <strong>Description:</strong> Timeout in milliseconds for create HTTPS connection or execute AWS request. Specify 0 to use SDK default.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aws-key-management-request-timeout=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `integer`
- <strong>Default Value:</strong> 0

---

### `aws_key_management_rotate_key`

- <strong>Description:</strong> Set this variable to a data key ID to perform rotation of the key to the master key given in `aws_key_management_master_key_id`. Specify -1 to rotate all keys.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aws-key-management-rotate-key=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `integer`
- <strong>Default Value:</strong>

---

## Options

### `aws_key_management`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the [mysql.plugins](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlplugin-table/) table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aws-key-management=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---