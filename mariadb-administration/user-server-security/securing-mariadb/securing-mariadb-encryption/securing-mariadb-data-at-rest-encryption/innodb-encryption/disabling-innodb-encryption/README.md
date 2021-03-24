# Disabling InnoDB Encryption

The process involved in safely disabling encryption for your InnoDB tables is a little more complicated than that of [enabling encryption](/kb/en/enabling-innodb-encryption/).  Turning off the relevant system variables doesn't decrypt the tables.  If you turn it off and remove the encryption key management plugin, it'll render the encrypted data inaccessible.

In order to safely disable encryption, you first need to decrypt the tablespaces and the Redo Log, then turn off the system variables.  The specifics of this process depends on whether you are using automatic or manual encryption of the InnoDB tablespaces.

### Disabling Encryption for Automatically Encrypted Tablespaces

When an InnoDB tablespace has the [ENCRYPTED](/kb/en/create-table/#encrypted) table option set to `DEFAULT` and the [innodb_encrypt_tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables) system variable is set to `ON` or `FORCE`, the tablespace's encryption is automatically managed by the background encryption threads. When you want to disable encryption for these tablespaces, you must ensure that the background encryption threads decrypt the tablespaces before removing the encryption keys. Otherwise, the tablespace remains encrypted and becomes inaccessible once you've removed the keys.

To safely decrypt the tablespaces, first, set the [innodb_encrypt_tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables) system variable to `OFF`:

```sql
SET GLOBAL innodb_encrypt_tables = OFF;
```

Next, set the [innodb_encryption_threads](/kb/en/innodb-system-variables/#innodb_encryption_threads) system variable to a non-zero value:

```sql
SET GLOBAL innodb_encryption_threads = 4;
```

Then, set the [innodb_encryption_rotate_key_age](/kb/en/innodb-system-variables/#innodb_encryption_rotate_key_age) system variable to `1`:

```sql
SET GLOBAL innodb_encryption_rotate_key_age = 1;
```

Once set, any InnoDB tablespaces that have the [ENCRYPTED](/kb/en/create-table/#encrypted) table option set to `DEFAULT` will be [decrypted](/kb/en/innodb-background-encryption-threads/#background-operations) in the background by the InnoDB [background encryption threads](/kb/en/innodb-background-encryption-threads/#background-encryption-threads).

#### Decryption Status

You can [check the status](/kb/en/innodb-background-encryption-threads/#checking-the-status-of-background-operations) of the decryption process using the [INNODB_TABLESPACES_ENCRYPTION](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_tablespaces_encryption-table/) table in the [information_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database.

```sql
SELECT COUNT(*) AS "Number of Encrypted Tablespaces"
FROM information_schema.INNODB_TABLESPACES_ENCRYPTION
WHERE ENCRYPTION_SCHEME != 0
   OR ROTATING_OR_FLUSHING != 0; 
```

This query shows the number of InnoDB tablespaces that currently using background encryption threads.  Once the count reaches 0, then all of your InnoDB tablespaces are unencrypted.  Be sure to also remove encryption on the [Redo Log](#disabling-encryption-for-the-redo-log) and the [Aria](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/aria-encryption/) storage engine before removing the encryption key management settings from your configuration file.

### Disabling Encryption for Manually Encrypted Tablespaces

In the case of manually encrypted InnoDB tablespaces, (that is, those where the [ENCRYPTED](/kb/en/create-table/#encrypted) table option is set to `YES`), you must issue an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement to decrypt each tablespace before removing the encryption keys.  Otherwise, the tablespace remains encrypted and becomes inaccessible without the keys.

First, query the Information Schema [TABLES](/kb/en/information-schema-tables-table/) table to find the encrypted tables.  This can be done with a `WHERE` clause filtering the `CREATE_OPTIONS` column.

```sql
SELECT TABLE_SCHEMA AS "Database", TABLE_NAME AS "Table"
FROM information_schema.TABLES
WHERE ENGINE='InnoDB' 
      AND CREATE_OPTIONS LIKE '%`ENCRYPTED`=YES%';
```

For each table in the result-set, issue an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement, setting the [ENCRYPTED](/kb/en/create-table/#encrypted) table option to `NO`.

```sql
SELECT NAME, ENCRYPTION_SCHEME, CURRENT_KEY_ID
FROM information_schema.INNODB_TABLESPACES_ENCRYPTION
WHERE NAME='db1/tab1';
+----------+-------------------+----------------+
| NAME     | ENCRYPTION_SCHEME | CURRENT_KEY_ID |
+----------+-------------------+----------------+
| db1/tab1 |                 1 |            100 |
+----------+-------------------+----------------+

ALTER TABLE tab1
   ENCRYPTED=NO;

SELECT NAME, ENCRYPTION_SCHEME, CURRENT_KEY_ID
FROM information_schema.INNODB_TABLESPACES_ENCRYPTION
WHERE NAME='db1/tab1';
+----------+-------------------+----------------+
| NAME     | ENCRYPTION_SCHEME | CURRENT_KEY_ID |
+----------+-------------------+----------------+
| db1/tab1 |                 0 |            100 |
+----------+-------------------+----------------+
```

Once you have removed encryption from all the tables, your InnoDB deployment is unencrypted.  Be sure to also remove encryption from the [Redo Log](#disabling-encryption-for-the-redo-log) as well as [Aria](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/aria-encryption/) and any other storage engines that support encryption before removing the encryption key management settings from your configuration file.

InnoDB does not permit manual encryption changes to tables in the [system](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/) tablespace using [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/). Encryption of the [system](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/) tablespace can only be configured by setting the value of the [innodb_encrypt_tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables) system variable. This means that when you want to encrypt or decrypt the [system](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/) tablespace, you must also set a non-zero value for the [innodb_encryption_threads](/kb/en/innodb-system-variables/#innodb_encryption_threads) system variable, and you must also set the [innodb_system_rotate_key_age](/kb/en/innodb-system-variables/#innodb_encryption_rotate_key_age) system variable to `1` to ensure that the system tablespace is properly encrypted or decrypted by the background threads. See [MDEV-14398](https://jira.mariadb.org/browse/MDEV-14398) for more information.

### Disabling Encryption for Temporary Tablespaces

The [innodb_encrypt_temporary_tables](/kb/en/innodb-system-variables/#innodb_encrypt_temporary_tables) system variable controls the configuration of encryption for the [temporary tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-temporary-tablespaces/). To disable it, remove the system variable from your server's [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), and then restart the server.

### Disabling Encryption for the Redo Log

InnoDB uses the [Redo Log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) in crash recovery. By default, these events are written to file in an unencrypted state. In removing data-at-rest encryption for InnoDB, be sure to also disable encryption for the Redo Log before removing encryption key settings.  Otherwise the Redo Log can become inaccessible without the encryption keys.

First, check the value of the [innodb_fast_shutdown](/kb/en/innodb-system-variables/#innodb_fast_shutdown) system variable with the [SHOW VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables/) statement. For example:

```sql
SHOW VARIABLES LIKE 'innodb_fast_shutdown';
+----------------------+-------+
| Variable_name        | Value |
+----------------------+-------+
| innodb_fast_shutdown |     2 |
+----------------------+-------+
```

When the value is set to `2`, InnoDB performs an unclean shutdown, so it will need the [Redo Log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) at the next server startup. Ensure that the variable is set to `0`, `1`, or `3`. For performance reasons, `1` is usually the best option. It can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL innodb_fast_shutdown = 1;
```

Then, set the [innodb_encrypt_log](/kb/en/innodb-system-variables/#innodb_encrypt_log) system variable to `OFF` in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). Once this is done, [restart](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/) the MariaDB Server. When the Server comes back online, it begins writing unencrypted data to the Redo Log.

### See Also

- [Enabling InnoDB encryption](/kb/en/enabling-innodb-encryption/)