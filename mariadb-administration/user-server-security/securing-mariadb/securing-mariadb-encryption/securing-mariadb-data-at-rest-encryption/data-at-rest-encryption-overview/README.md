# Data-at-Rest Encryption Overview

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

Encryption of tables and tablespaces was added in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/). There were substantial changes made in [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/), and the description below applies only to [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/) and later

## Overview

Having tables encrypted makes it almost impossible for someone to access or
steal a hard disk and get access to the original data. MariaDB got Data-at-Rest Encryption with [MariaDB 10.1](/kb/en/what-is-mariadb-101/). This functionality is also known as "Transparent Data Encryption (TDE)".

This assumes that encryption keys are stored on another system.

Using encryption has an overhead of roughly <em>3-5%</em>.

## Which Storage Engines Does MariaDB Encryption Support?

MariaDB encryption is fully supported for the [XtraDB and InnoDB](/kb/en/xtradb-and-innodb/)
storage engines. Encryption is also supported for the Aria storage
engine, but only for tables created with `ROW_FORMAT=PAGE` (the default), and for the binary log (replication log).

MariaDB allows the user to configure flexibly what to encrypt.  In XtraDB or
InnoDB, one can choose to encrypt:

- everything — all tablespaces (with all tables)
- individual tables
- everything, excluding individual tables

Additionally, one can choose to encrypt XtraDB/InnoDB log files (recommended).

## Limitations

These limitations exist in the data-at-rest encryption implementation in [MariaDB 10.1](/kb/en/what-is-mariadb-101/):

- Only <strong>data</strong> and only <strong>at rest</strong> is encrypted. Metadata (for example `.frm` files) and data sent to the client are not encrypted (but see [Secure Connections](/kb/en/secure-connections/)).
- Only the MariaDB server knows how to decrypt the data, in particular
<ul start="1"><li>[mysqlbinlog](/clients-utilities/mysqlbinlog/) can read encrypted binary logs only when --read-from-remote-server is used ([MDEV-8813](https://jira.mariadb.org/browse/MDEV-8813)).
</li><li>[Percona XtraBackup](/kb/en/percona-xtrabackup/) cannot back up instances that use encrypted InnoDB. However, MariaDB's fork, [MariaDB Backup](/kb/en/mariadb-backup/), can back up encrypted instances.
</li></ul>
- The disk-based [Galera gcache](https://galeracluster.com/library/documentation/state-transfer.html#write-set-cache-gcache) is not encrypted in the community version of MariaDB Server ([MDEV-9639](https://jira.mariadb.org/browse/MDEV-9639)). However, this file is encrypted in [MariaDB Enterprise Server 10.4](https://mariadb.com/docs/features/mariadb-enterprise-server/).
- The [Audit plugin](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/) cannot create encrypted output. Send it to syslog and configure the protection there instead.
- File-based [general query log](/mariadb-administration/server-monitoring-logs/general-query-log/) and [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) cannot be encrypted ([MDEV-9639](https://jira.mariadb.org/browse/MDEV-9639)).
- The Aria log is not encrypted ([MDEV-8587](https://jira.mariadb.org/browse/MDEV-8587)). This affects only non-temporary Aria tables though.
- The MariaDB [error log](/mariadb-administration/server-monitoring-logs/error-log/) is not encrypted. The error log can contain query text and data in some cases, including crashes, assertion failures, and cases where InnoDB/XtraDB write monitor output to the log to aid in debugging. It can be sent to syslog too, if needed.

## Encryption Key Management

MariaDB's data-at-rest encryption requires the use of a [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/). These plugins are responsible both for the management of encryption keys and for the actual encryption and decryption of data.

MariaDB supports the use of [multiple encryption keys](/kb/en/encryption-key-management/#using-multiple-encryption-keys). Each encryption key uses a 32-bit integer as a key identifier. If the specific plugin supports [key rotation](/kb/en/encryption-key-management/#rotating-keys), then encryption keys can also be rotated, which creates a new version of the encryption key.

How MariaDB manages encryption keys depends on which encryption key management solution you choose. Currently, MariaDB has three options:

- [File Key Management Plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/file-key-management-encryption-plugin/)
- [AWS Key Management Plugin](/kb/en/aws-key-management-encryption-plugin/)
- [Eperi Key Management Plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/eperi-key-management-encryption-plugin/)

Once you have an key management and encryption plugin set up and configured for your server, you can begin using encryption options to better secure your data.

## Encrypting Data

Encryption occurs whenever MariaDB writes pages to disk. Encrypting table data requires that you install a [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/), such as the [File Key Management](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/file-key-management-encryption-plugin/) plugin.  Once you have a plugin set up and configured, you can enable encryption for your InnoDB and Aria tables.

### Encrypting Table Data

MariaDB supports data-at-rest encryption for InnoDB and Aria storage engines. Additionally, it supports encrypting the [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) and internal on-disk temporary tables that use the Aria storage engine..

- [Encrypting Data for InnoDB/XtraDB](/kb/en/encrypting-data-for-innodb-xtradb/)
- [Encrypting Data for Aria](/kb/en/encrypting-data-for-aria/)

### Encrypting Temporary Files

MariaDB also creates temporary files on disk. For example, a binary log cache will be written to a temporary file if the binary log cache exceeds <a undefined>binlog_cache_size</a> or <a undefined>binlog_stmt_cache_size</a>, and temporary files are also often used for filesorts during query execution. Since [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/), these temporary files can also be encrypted if [encrypt_tmp_files=ON](/kb/en/server-system-variables/#encrypt_tmp_files) is set.

Since [MariaDB 10.1.27](/kb/en/mariadb-10127-release-notes/), [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/) and [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/), temporary files created internally by InnoDB, such as those used for merge sorts and row logs can also be encrypted if [innodb_encrypt_log=ON](/kb/en/xtradbinnodb-server-system-variables/#innodb_encrypt_log) is set. These files are encrypted regardless of whether the tables involved are encrypted or not, and regardless of whether [encrypt_tmp_files](/kb/en/server-system-variables/#encrypt_tmp_files) is set or not.

### Encrypting Binary Logs

Since [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/), MariaDB can also encrypt [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/) (including [relay logs](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/)).

- [Encrypting Binary Logs](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/encrypting-binary-logs/)

## Encryption and Page Compression

Data-at-rest encryption and [InnoDB page compression](/kb/en/compression/) can be used
together. When they are used together, data is first compressed, and then it is encrypted. In
this case you save space and still have your data protected.

## Thanks

- Tablespace encryption was donated to the MariaDB project by Google.
- Per-table encryption and key identifier support was donated to the MariaDB project by [eperi](http://eperi.de/en).

We are grateful to these companies for their support of MariaDB!

## See Also

- [Encryption functions](/kb/en/encryption-functions/)
- [DES_DECRYPT()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/des_decrypt/)
- [DES_ENCRYPT()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/des_encrypt/)
- A [blog post about table encryption](https://mariadb.com/blog/table-and-tablespace-encryption-mariadb-101/) with benchmark results