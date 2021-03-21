# Encrypting Binary Logs

MariaDB Server can encrypt the server's [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/) and [relay logs](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/). This ensures that your binary logs are only accessible through MariaDB.

## Basic Configuration

Since [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/), MariaDB can also encrypt [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/) (including [relay logs](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/)). Encryption of binary logs is configured by the <a undefined>encrypt_binlog</a> system variable.

Users of data-at-rest encryption will also need to have a [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) configured. Some examples are [File Key Management Plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/file-key-management-encryption-plugin/) and [AWS Key Management Plugin](/kb/en/aws-key-management-encryption-plugin/).

```sql
[mariadb]
...

# File Key Management
plugin_load_add = file_key_management
file_key_management_filename = /etc/mysql/encryption/keyfile.enc
file_key_management_filekey = FILE:/etc/mysql/encryption/keyfile.key
file_key_management_encryption_algorithm = AES_CTR

# Binary Log Encryption
encrypt_binlog=ON
```

## Encryption Keys

[Key management and encryption plugins](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) support [using multiple encryption keys](/kb/en/encryption-key-management/#using-multiple-encryption-keys). Each encryption key can be defined with a different 32-bit integer as a key identifier.

MariaDB uses the encryption key with ID 1 to encrypt [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/).

### Key Rotation

Some [key management and encryption plugins](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) allow you to automatically rotate and version your encryption keys. If a plugin support key rotation, and if it rotates the encryption keys, then InnoDB's [background encryption threads](/kb/en/innodb-background-encryption-threads/) can re-encrypt InnoDB pages that use the old key version with the new key version. However, the binary log does <strong>not</strong> have a similar mechanism, which means that existing binary logs remain encrypted with the older key version, but new binary logs will be encrypted with the new key version. For more information, see [MDEV-20098](https://jira.mariadb.org/browse/MDEV-20098).

In order for key rotation to work, both the backend key management service (KMS) and the corresponding [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) have to support key rotation. See [Encryption Key Management: Support for Key Rotation in Encryption Plugins](/kb/en/encryption-key-management/#support-for-key-rotation-in-encryption-plugins) to determine which plugins currently support key rotation.

## Enabling Encryption

Encryption of binary logs can be enabled by doing the following process.

- First, stop the server.

- Then, set <a undefined>encrypt_binlog=ON</a> in the MariaDB configuration file.

- Then, start the server.

From that point forward, any new [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/) will be encrypted. To delete old unencrypted [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/), you can use [RESET MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-master/) or <a undefined>PURGE BINARY LOGS</a>.

## Disabling Encryption

Encryption of [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/) can be disabled by doing the following process.

- First, stop the server.

- Then, set <a undefined>encrypt_binlog=OFF</a> in the MariaDB configuration file.

- Then, start the server.

From that point forward, any new [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/) will be unencrypted. If you would like the server to continue to have access to old encrypted [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/), then make sure to keep your [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) loaded.

## Understanding Binlog Encryption

When starting with binary log encryption, MariaDB Server logs a `Format_descriptor_log_event` and a `START_ENCRYPTION_EVENT`, then encrypts all subsequent events for the binary log.

Each event's header and footer are created and processed to produce encrypted blocks.  These encrypted blocks are produced before transactions are committed and before the events are flushed to the binary log.  As such, they exist in an encrypted state in memory buffers and in the `IO_CACHE` files for user connections.

### Effects of Data-at-Rest Encryption on Replication

When using encrypted binary logs with [replication](/replication/), it is completely supported to have different encryption keys on the master and slave. The master decrypts encrypted binary log events as it reads them from disk, and before its [binary log dump thread](/kb/en/replication-threads/#binary-log-dump-thread) sends them to the slave, so the slave actually receives the unencrypted binary log events.

If you want to ensure that binary log events are encrypted as they are transmitted between the master and slave, then you will have to use [TLS with the replication connection](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/replication-with-secure-connections/).

### Effects of Data-at-Rest Encryption on mysqlbinlog

[mysqlbinlog](/clients-utilities/mysqlbinlog/) does not currently have the ability to decrypt encrypted [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/) on its own (see [MDEV-8813](https://jira.mariadb.org/browse/MDEV-8813) about that). In order to use mysqlbinlog with encrypted [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/), you have to use the [--read-from-remote-server](/clients-utilities/mysqlbinlog/mysqlbinlog-options/) command-line option, so that the server can decrypt the [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/) for mysqlbinlog.

Note, using `--read-from-remote-server` option on versions of the `mysqlbinlog` utility that do not have the [MDEV-20574](https://jira.mariadb.org/browse/MDEV-20574) fix, can corrupt binlog positions when the binary log is encrypted.