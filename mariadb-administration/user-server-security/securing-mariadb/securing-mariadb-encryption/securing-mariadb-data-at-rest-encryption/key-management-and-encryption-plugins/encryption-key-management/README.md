# Encryption Key Management

MariaDB's [data-at-rest encryption](/kb/en/data-at-rest-encryption/) requires the use of a key management and encryption plugin. These plugins are responsible both for the management of encryption keys and for the actual encryption and decryption of data.

MariaDB supports the use of multiple encryption keys. Each encryption key uses a 32-bit integer as a key identifier. If the specific plugin supports key rotation, then encryption keys can also be rotated, which creates a new version of the encryption key.

## Choosing an Encryption Key Management Solution

How MariaDB manages encryption keys depends on which encryption key management solution you choose. Currently, MariaDB has three options:

### File Key Management Plugin

The File Key Management plugin that ships with MariaDB is a basic key management and encryption plugin that reads keys from a plain-text file. It can also serve as example and as a starting point when developing a key management plugin.

For more information, see [File Key Management Plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/file-key-management-encryption-plugin/).

### AWS Key Management Plugin

The AWS Key Management plugin is a key management and encryption plugin that uses the Amazon Web Services (AWS) Key Management Service (KMS). The AWS Key Management plugin depends on the [AWS SDK for C++](https://github.com/aws/aws-sdk-cpp), which uses the [Apache License, Version 2.0](https://github.com/aws/aws-sdk-cpp/blob/master/LICENSE). This license is not compatible with MariaDB Server's [GPL 2.0 license](/kb/en/mariadb-license/), so we are not able to distribute packages that contain the AWS Key Management plugin. Therefore, the only way to currently obtain the plugin is to install it from source.

For more information, see [AWS Key Management Plugin](/kb/en/aws-key-management-encryption-plugin/).

### Eperi Key Management Plugin

The Eperi Key Management plugin is a key management and encryption plugin that uses the [eperi Gateway for Databases](https://eperi.com/database-encryption/). The [eperi Gateway for Databases](https://eperi.com/database-encryption/) stores encryption keys on the key server outside of the database server itself, which provides an extra level of security. The [eperi Gateway for Databases](https://eperi.com/database-encryption/) also supports performing all data encryption operations on the key server as well, but this is optional.

For more information, see [Eperi Key Management Plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/eperi-key-management-encryption-plugin/).

## Using Multiple Encryption Keys

Key management and encryption plugins support using multiple encryption keys. Each encryption key can be defined with a different 32-bit integer as a key identifier.

The support for multiple keys opens up some potential use cases. For example, let's say that a hypothetical key management and encryption plugin is configured to provide two encryption keys. One encryption key might be intended for "low security" tables. It could use short keys, which might not be rotated, and data could be encrypted with a fast encryption algorithm. Another encryption key might be intended for "high security" tables. It could use long keys, which are rotated often, and data could be encrypted with a slower, but more secure encryption algorithm. The user would specify the identifier of the key that they want to use for different tables, only using high level security where it's needed.

There are two encryption key identifiers that have special meanings in MariaDB. Encryption key `1` is intended for encrypting system data, such as InnoDB redo logs, binary logs, and so on. It must always exist when [data-at-rest encryption](/kb/en/data-at-rest-encryption/) is enabled. Encryption key `2` is intended for encrypting temporary data, such as temporary files and temporary tables. It is optional. If it doesn't exist, then MariaDB uses encryption key `1` for these purposes instead.

When [encrypting InnoDB tables](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/), the key that is used to encrypt tables [can be changed](/kb/en/innodb-xtradb-encryption-keys/).

When [encrypting Aria tables](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/aria-encryption/), the key that is used to encrypt tables [cannot currently be changed](/kb/en/aria-encryption-keys/).

## Key Rotation

Encryption key rotation is optional in MariaDB Server. Key rotation is only supported if the backend key management service (KMS) supports key rotation, and if the corresponding key management and encryption plugin for MariaDB also supports key rotation. When a key management and encryption plugin supports key rotation, users can opt to rotate one or more encryption keys, which creates a new version of each rotated encryption key.

Key rotation allows users to improve data security in the following ways:

- If the server is configured to automatically re-encrypt table data with the newer version of the encryption key after the key is rotated, then that prevents an encryption key from being used for long periods of time.
- If the server is configured to simultaneously encrypt table data with multiple versions of the encryption key after the key is rotated, then that prevents all data from being leaked if a single encryption key version is compromised.

The [InnoDB storage engine](/columns-storage-engines-and-plugins/storage-engines/innodb/) has [background encryption threads](/kb/en/innodb-background-encryption-threads/) that can [automatically re-encrypt pages when key rotations occur](/kb/en/innodb-background-encryption-threads/#background-operations).

The [Aria storage engine](/columns-storage-engines-and-plugins/storage-engines/aria/) does [not currently have a similar mechanism to re-encrypt pages in the background when key rotations occur](/kb/en/aria-encryption-keys/#key-rotation).

### Support for Key Rotation in Encryption Plugins

#### Encryption Plugins with Key Rotation Support

- The [AWS Key Management Service (KMS)](https://aws.amazon.com/kms/) supports encryption key rotation, and the corresponding [AWS Key Management Plugin](/kb/en/aws-key-management-encryption-plugin/) also supports encryption key rotation.

- The [eperi Gateway for Databases](https://eperi.com/database-encryption/) supports encryption key rotation, and the corresponding [Eperi Key Management Plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/eperi-key-management-encryption-plugin/) also supports encryption key rotation.

#### Encryption Plugins without Key Rotation Support

- The [File Key Management Plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/file-key-management-encryption-plugin/) does not support encryption key rotation, because it does not use a backend key management service (KMS).

## Encryption Plugin API

New key management and encryption plugins can be developed using the [encryption plugin API](/kb/en/encryption-plugin-api/).