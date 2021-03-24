# InnoDB Encryption Troubleshooting

<br>

### Wrong Create Options

With InnoDB tables using encryption, there are several cases where a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement can throw Error 1005, due to the InnoDB error 140, `Wrong create options`.  For instance,
<br><br>

```sql
CREATE TABLE `test`.`table1` ( `id` int(4) primary key , `name` varchar(50));
ERROR 1005 (HY000): Can't create table `test`.`table1` (errno: 140 "Wrong create options")
```

When this occurs, you can usually get more information about the cause of the error by following it with a [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/) statement.

This error is known to occur in the following cases:

- Encrypting a table by setting the [ENCRYPTED](/kb/en/create-table/#encrypted) table option to `YES` when the [innodb_file_per_table](/kb/en/innodb-system-variables/#innodb_file_per_table) is set to `OFF`.In this case, [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/) would return the following:

```sql
SHOW WARNINGS;
+---------+------+---------------------------------------------------------------------+
| Level   | Code | Message                                                             |
+---------+------+---------------------------------------------------------------------+
| Warning |  140 | InnoDB: ENCRYPTED requires innodb_file_per_table                    |
| Error   | 1005 | Can't create table `db1`.`tab3` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB     |
+---------+------+---------------------------------------------------------------------+
3 rows in set (0.00 sec)
```

- Encrypting a table by setting the [ENCRYPTED](/kb/en/create-table/#encrypted) table option to `YES`, and the [innodb_default_encryption_key_id](/kb/en/innodb-system-variables/#innodb_default_encryption_key_id) system variable or the [ENCRYPTION_KEY_ID](/kb/en/create-table/#encryption_key_id) table option refers to a non-existent key identifier. In this case, [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/) would return the following:

```sql
SHOW WARNINGS;
+---------+------+---------------------------------------------------------------------+
| Level   | Code | Message                                                             |
+---------+------+---------------------------------------------------------------------+
| Warning |  140 | InnoDB: ENCRYPTION_KEY_ID 500 not available                         |
| Error   | 1005 | Can't create table `db1`.`tab3` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB     |
+---------+------+---------------------------------------------------------------------+
3 rows in set (0.00 sec)
```

- In some versions, this could happen while creating a table with the [ENCRYPTED](/kb/en/create-table/#encrypted) table option set to `DEFAULT` while the [innodb_encrypt_tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables) system variable is set to `OFF`, and the [innodb_default_encryption_key_id](/kb/en/innodb-system-variables/#innodb_default_encryption_key_id) system variable or the [ENCRYPTION_KEY_ID](/kb/en/create-table/#encryption_key_id) table option are <strong>not</strong> set to `1`. In this case, [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/) would return the following:

```sql
SHOW WARNINGS;
+---------+------+---------------------------------------------------------------------+
| Level   | Code | Message                                                             |
+---------+------+---------------------------------------------------------------------+
| Warning |  140 | InnoDB: innodb_encrypt_tables=OFF only allows ENCRYPTION_KEY_ID=1   |
| Error   | 1005 | Can't create table `db1`.`tab3` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB     |
+---------+------+---------------------------------------------------------------------+
3 rows in set (0.00 sec)
```

Starting in [MariaDB 10.1.39](/kb/en/mariadb-10139-release-notes/), [MariaDB 10.2.23](/kb/en/mariadb-10223-release-notes/), and [MariaDB 10.3.14](/kb/en/mariadb-10314-release-notes/), creating a table with the [ENCRYPTED](/kb/en/create-table/#encrypted) table option set to `DEFAULT` while the [innodb_encrypt_tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables) system variable is set to `OFF`, and the [innodb_default_encryption_key_id](/kb/en/innodb-system-variables/#innodb_default_encryption_key_id) system variable or the [ENCRYPTION_KEY_ID](/kb/en/create-table/#encryption_key_id) table option are <strong>not</strong> set to `1` will no longer fail, and it will no longer throw a warning.

For more information, see [MDEV-18601](https://jira.mariadb.org/browse/MDEV-18601).

### Setting Encryption Key ID For an Unencrypted Table

If you set the [ENCRYPTION_KEY_ID](/kb/en/create-table/#encryption_key_id) table option for a table that is unencrypted because the [innodb_encrypt_tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables) system variable is set to `OFF` and the [ENCRYPTED](/kb/en/create-table/#encrypted) table option set to `DEFAULT`, then this encryption key ID will be saved in the table's `.frm` file, but the encryption key will not be saved to the table's `.ibd` file.

As a side effect, with the current encryption design, if the [innodb_encrypt_tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables) system variable is later set to `ON`, and InnoDB goes to encrypt the table, then the [InnoDB background encryption threads](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/innodb-background-encryption-threads/) will not read this encryption key ID from the `.frm` file. Instead, the threads may encrypt the table with the encryption key with ID `1`, which is internally considered the default encryption key when no key is specified. For example:

```sql
SET GLOBAL innodb_encrypt_tables=OFF;

CREATE TABLE tab1 (
   id INT PRIMARY KEY,
   str VARCHAR(50)
) ENCRYPTION_KEY_ID=100;

SET GLOBAL innodb_encrypt_tables=ON;

SELECT NAME, ENCRYPTION_SCHEME, CURRENT_KEY_ID
FROM information_schema.INNODB_TABLESPACES_ENCRYPTION
WHERE NAME='db1/tab1';
+----------+-------------------+----------------+
| NAME     | ENCRYPTION_SCHEME | CURRENT_KEY_ID |
+----------+-------------------+----------------+
| db1/tab1 |                 1 |              1 |
+----------+-------------------+----------------+
```

A similar problem is that, if you set the [ENCRYPTION_KEY_ID](/kb/en/create-table/#encryption_key_id) table option for a table that is unencrypted because the [ENCRYPTED](/kb/en/create-table/#encrypted) table option is set to `NO`, then this encryption key ID will be saved in the table's `.frm` file, but the encryption key will not be saved to the table's `.ibd` file.

Recent versions of MariaDB will throw warnings in the case where the [ENCRYPTED](/kb/en/create-table/#encrypted) table option is set to `NO`, but they will allow the operation to succeed. For example:

```sql
CREATE TABLE tab1 (
   id INT PRIMARY KEY,
   str VARCHAR(50)
) ENCRYPTED=NO ENCRYPTION_KEY_ID=100;
Query OK, 0 rows affected, 1 warning (0.01 sec)

SHOW WARNINGS;
+---------+------+--------------------------------------------------+
| Level   | Code | Message                                          |
+---------+------+--------------------------------------------------+
| Warning |  140 | InnoDB: ENCRYPTED=NO implies ENCRYPTION_KEY_ID=1 |
+---------+------+--------------------------------------------------+
1 row in set (0.00 sec)
```

However, in this case, if you change the [ENCRYPTED](/kb/en/create-table/#encrypted) table option to `YES` or `DEFAULT` with [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/), then it will actually use the proper key. For example:

```sql
SET GLOBAL innodb_encrypt_tables=ON;

ALTER TABLE tab1 ENCRYPTED=DEFAULT;

SELECT NAME, ENCRYPTION_SCHEME, CURRENT_KEY_ID
FROM information_schema.INNODB_TABLESPACES_ENCRYPTION
WHERE NAME = 'db1/tab1';
+----------+-------------------+----------------+
| NAME     | ENCRYPTION_SCHEME | CURRENT_KEY_ID |
+----------+-------------------+----------------+
| db1/tab1 |                 1 |            100 |
+----------+-------------------+----------------+
```

For more information, see [MDEV-17230](https://jira.mariadb.org/browse/MDEV-17230), [MDEV-18601](https://jira.mariadb.org/browse/MDEV-18601), and [MDEV-19086](https://jira.mariadb.org/browse/MDEV-19086).

### Tablespaces Created on MySQL 5.1.47 or Earlier

MariaDB's data-at-rest encryption implementation re-used previously unused fields in InnoDB's buffer pool pages to identify the encryption key version and the post-encryption checksum. Prior to MySQL 5.1.48, these unused fields were not initialized in memory due to performance concerns. These fields still had zero values most of the time, but since they were not explicitly initialized, that means that these fields could have occasionally had non-zero values that could have been written into InnoDB's tablespace files. If MariaDB were to encounter an unencrypted page from a tablespace file that was created on an early version of MySQL that also had non-zero values in these fields, then it would mistakenly think that the page was encrypted.

The fix for [MDEV-12112](https://jira.mariadb.org/browse/MDEV-12112) that was included in [MariaDB 10.1.38](/kb/en/mariadb-10138-release-notes/), [MariaDB 10.2.20](/kb/en/mariadb-10220-release-notes/), and [MariaDB 10.3.12](/kb/en/mariadb-10312-release-notes/) changed the way that MariaDB distinguishes between encrypted and unencrypted pages, so that it is less likely to mistake an unencrypted page for an encrypted page.

In [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later, if [innodb_checksum_algorithm](/kb/en/innodb-system-variables/#innodb_checksum_algorithm) is set to `full_crc32` or `strict_full_crc32`, and if the table does not use [ROW_FORMAT=COMPRESSED](/kb/en/innodb-storage-formats/), then data files will be guaranteed to be zero-initialized.

For more information, see [MDEV-18097](https://jira.mariadb.org/browse/MDEV-18097).

### Spatial Indexes

[MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) introduces support for encrypting [spatial indexes](/sql-statements-structure/geographic-geometric-features/spatial-index/).  To enable, set the [innodb_checksum_algorithm](/kb/en/innodb-system-variables/#innodb_checksum_algorithm) to `full_crc32` or to `strict_full_crc32`.  Note that MariaDB only encrypts spatial indexes when the [ROW_FORMAT](/kb/en/create-table/#row_format) table option is <strong>not</strong> set to [COMPRESSED](/kb/en/innodb-storage-formats/#compressed).

In older versions of MariaDB, spatial index encryption is unsupported.  Tables that contain spatial indexes store them unencrypted.

For more information, see [MDEV-12026](https://jira.mariadb.org/browse/MDEV-12026).