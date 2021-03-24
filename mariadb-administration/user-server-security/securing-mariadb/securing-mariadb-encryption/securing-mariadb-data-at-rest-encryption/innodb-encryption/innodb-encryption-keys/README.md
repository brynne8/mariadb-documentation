# InnoDB Encryption Keys

InnoDB uses [encryption key management](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) plugins to support the use of multiple [encryption keys](/kb/en/encryption-key-management/#using-multiple-encryption-keys).

## Encryption Keys

Each encryption key has a 32-bit integer that serves as a key identifier.

The default key is set using the [innodb_default_encryption_key_id](/kb/en/innodb-system-variables/#innodb_default_encryption_key_id) system variable.

Encryption keys can also be specified with the [ENCRYPTION_KEY_ID](/kb/en/create-table/#encryption_key_id) table option for tables that use [file-per-table](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/) tablespaces.

InnoDB encrypts the [temporary tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-temporary-tablespaces/) using the encryption key with the ID `1`.

InnoDB encrypts the [Redo Log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) using the encryption key with the ID `1`.

### Keys with Manually Encrypted Tablespaces

With tables that use [manually](/kb/en/innodb-enabling-encryption/#enabling-encryption-for-manually-encrypted-tablespaces) enabled encryption, one way to set the specific encryption key for the table is to use the [ENCRYPTION_KEY_ID](/kb/en/create-table/#encryption_key_id) table option. For example:

```sql
CREATE TABLE tab1 (
   id int PRIMARY KEY,
   str varchar(50)
) ENCRYPTED=YES ENCRYPTION_KEY_ID=100;

SELECT NAME, ENCRYPTION_SCHEME, CURRENT_KEY_ID
FROM information_schema.INNODB_TABLESPACES_ENCRYPTION
WHERE NAME='db1/tab1';
+----------+-------------------+----------------+
| NAME     | ENCRYPTION_SCHEME | CURRENT_KEY_ID |
+----------+-------------------+----------------+
| db1/tab1 |                 1 |            100 |
+----------+-------------------+----------------+
```

If the [ENCRYPTION_KEY_ID](/kb/en/create-table/#encryption_key_id) table option is not set for a table that uses [manually](/kb/en/innodb-enabling-encryption/#enabling-encryption-for-manually-encrypted-tablespaces) enabled encryption, then it will inherit the value from the [innodb_default_encryption_key_id](/kb/en/innodb-system-variables/#innodb_default_encryption_key_id) system variable. For example:

```sql
SET SESSION innodb_default_encryption_key_id=100;

CREATE TABLE tab1 (
   id int PRIMARY KEY,
   str varchar(50)
) ENCRYPTED=YES;

SELECT NAME, ENCRYPTION_SCHEME, CURRENT_KEY_ID
FROM information_schema.INNODB_TABLESPACES_ENCRYPTION
WHERE NAME='db1/tab1';
+----------+-------------------+----------------+
| NAME     | ENCRYPTION_SCHEME | CURRENT_KEY_ID |
+----------+-------------------+----------------+
| db1/tab1 |                 1 |            100 |
+----------+-------------------+----------------+
```

### Keys with Automatically Encrypted Tablespaces

With tables that use [automatically](/kb/en/innodb-enabling-encryption/#enabling-encryption-for-automatically-encrypted-tablespaces) enabled encryption, one way to set the specific encryption key for the table is to use the [innodb_default_encryption_key_id](/kb/en/innodb-system-variables/#innodb_default_encryption_key_id) system variable. For example:

```sql
SET GLOBAL innodb_encryption_threads=4;

SET GLOBAL innodb_encrypt_tables=ON;

SET SESSION innodb_default_encryption_key_id=100;

CREATE TABLE tab1 (
   id int PRIMARY KEY,
   str varchar(50)
);

SELECT NAME, ENCRYPTION_SCHEME, CURRENT_KEY_ID
FROM information_schema.INNODB_TABLESPACES_ENCRYPTION
WHERE NAME='db1/tab1';
+----------+-------------------+----------------+
| NAME     | ENCRYPTION_SCHEME | CURRENT_KEY_ID |
+----------+-------------------+----------------+
| db1/tab1 |                 1 |            100 |
+----------+-------------------+----------------+
```

InnoDB tables that are part of the [system](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/) tablespace can only be encrypted using the encryption key set by the [innodb_default_encryption_key_id](/kb/en/innodb-system-variables/#innodb_default_encryption_key_id) system variable.

If the table is in a [file-per-table](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/) tablespace, and if [innodb_encrypt_tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables) is set to `ON` or `FORCE`, and if [innodb_encryption_threads](/kb/en/innodb-system-variables/#innodb_encryption_threads) is set to a value greater than `0`, then you can also set the specific encryption key for the table by using the [ENCRYPTION_KEY_ID](/kb/en/create-table/#encryption_key_id) table option. For example:

```sql
SET GLOBAL innodb_encryption_threads=4;

SET GLOBAL innodb_encrypt_tables=ON;

CREATE TABLE tab1 (
   id int PRIMARY KEY,
   str varchar(50)
) ENCRYPTION_KEY_ID=100;

SELECT NAME, ENCRYPTION_SCHEME, CURRENT_KEY_ID
    -> FROM information_schema.INNODB_TABLESPACES_ENCRYPTION
    -> WHERE NAME='db1/tab1';
+----------+-------------------+----------------+
| NAME     | ENCRYPTION_SCHEME | CURRENT_KEY_ID |
+----------+-------------------+----------------+
| db1/tab1 |                 1 |            100 |
+----------+-------------------+----------------+
```

However, if [innodb_encrypt_tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables) is set to `OFF` or if [innodb_encryption_threads](/kb/en/innodb-system-variables/#innodb_encryption_threads) is set to `0`, then this will not work. See [InnoDB Encryption Troubleshooting: Setting Encryption Key ID For an Unencrypted Table](/kb/en/innodb-encryption-troubleshooting/#setting-encryption-key-id-for-an-unencrypted-table) for more information.

## Key Rotation

Some [key management and encryption plugins](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) allow you to automatically rotate and version your encryption keys. If a plugin support key rotation, and if it rotates the encryption keys, then InnoDB's [background encryption threads](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/innodb-background-encryption-threads/) can re-encrypt InnoDB pages that use the old key version with the new key version.

You can set the maximum age for an encryption key using the [innodb_encryption_rotate_key_age](/kb/en/innodb-system-variables/#innodb_encryption_rotate_key_age) system variable. When this variable is set to a non-zero value, background encryption threads constantly check pages to determine if any page is encrypted with a key version that's too old.  When the key version is too old, any page encrypted with the older version of the key is automatically re-encrypted in the background to use a more current version of the key.  Bear in mind, this constant checking can sometimes result in high CPU usage.

Key rotation for the InnoDB [Redo Log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) is only supported in [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/) and later.  For more information, see [MDEV-12041](https://jira.mariadb.org/browse/MDEV-12041).

In order for key rotation to work, both the backend key management service (KMS) and the corresponding [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) have to support key rotation. See [Encryption Key Management: Support for Key Rotation in Encryption Plugins](/kb/en/encryption-key-management/#support-for-key-rotation-in-encryption-plugins) to determine which plugins currently support key rotation.

### Disabling Background Key Rotation Operations

In the event that you encounter issues with background key encryption, you can disable it by setting the [innodb_encryption_rotate_key_age](/kb/en/innodb-system-variables/#innodb_encryption_rotate_key_age) system variable to `0`.  You may find this useful when the constant key version checks lead to excessive CPU usage.  It's also useful in cases where your encryption key management plugin does not support key rotation, (such as with the [file_key_management](/kb/en/encryption-key-management/#file-key-management-encryption-plugin) plugin).  For more information, see [MDEV-14180](https://jira.mariadb.org/browse/MDEV-14180).

There are, however, issues that can arise when the background key rotation is disabled.

#### Pending Encryption Operations

Prior to [MariaDB 10.2.24](/kb/en/mariadb-10224-release-notes/), [MariaDB 10.3.15](/kb/en/mariadb-10315-release-notes/), and [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/), when you update the value on the [innodb_encrypt_tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables) system variable InnoDB internally treats the subsequent [background operations](/kb/en/innodb-background-encryption-threads/#background-operations) to encrypt and decrypt tablespaces as background key rotations. See [MDEV-14398](https://jira.mariadb.org/browse/MDEV-14398) for more information.

In older versions of MariaDB, if you have recently changed the value of the [innodb_encrypt_tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables) system variable, then you must ensure that any pending background encryption or decryption operations are complete before disabling key rotation.  You can check the status of background encryption operations by querying the [INNODB_TABLESPACES_ENCRYPTION](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_tablespaces_encryption-table/) table in the [information_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database.

See [InnoDB Background Encryption Threads: Checking the Status of Background Operations](/kb/en/innodb-background-encryption-threads/#checking-the-status-of-background-operations) for some example queries.

Otherwise, in older versions of MariaDB, if you disable key rotation while there are background encryption threads at work, it may result in unencrypted tables that you want encrypted or vice versa.

For more information, see [MDEV-14398](https://jira.mariadb.org/browse/MDEV-14398).