# InnoDB Background Encryption Threads

InnoDB and XtraDB perform some encryption and decryption operations with background encryption threads.  The <a undefined>innodb_encryption_threads</a> system variable controls the number of threads that the storage engine uses for encryption-related background operations, including encrypting and decrypting pages after key rotations or configuration changes, and [scrubbing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-data-scrubbing/) data to permanently delete it.

## Background Operations

InnoDB and XtraDB perform the following encryption and decryption operations using background encryption threads:

- When [rotating encryption keys](/kb/en/encryption-key-management/#key-rotation), InnoDB's background encryption threads re-encrypt pages that use key versions older than <a undefined>innodb_encryption_rotate_key_age</a> to the new key version.
- When changing the <a undefined>innodb_encrypt_tables</a> system variable to `FORCE`, InnoDB's background encryption threads encrypt the [system](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/) tablespace and any [file-per-table](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/) tablespaces that have the <a undefined>ENCRYPTED</a> table option set to `DEFAULT`.
- When changing the <a undefined>innodb_encrypt_tables</a> system variable to `OFF`, InnoDB's background encryption threads decrypt the [system](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/) tablespace and any [file-per-table](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/) tablespacs that have the <a undefined>ENCRYPTED</a> table option set to `DEFAULT`.

The <a undefined>innodb_encryption_rotation_iops</a> system variable can be used to configure how many I/O operations you want to allow for the operations performed by InnoDB's background encryption threads.

Whenever you change the value on the <a undefined>innodb_encrypt_tables</a> system variable, InnoDB's background encryption threads perform the necessary encryption or decryption operations.  Because of this, you must have a non-zero value set for the <a undefined>innodb_encryption_threads</a> system variable. InnoDB also considers these operations to be key rotations internally. Because of this, you must have a non-zero value set for the <a undefined>innodb_encryption_rotate_key_age</a> system variable. For more information, see [disabling key rotations](#disabling-background-key-rotation-operations).

## Non-background Operations

InnoDB and XtraDB perform the following encryption and decryption operations <strong>without</strong> using background encryption threads:

- When a [file-per-table](innodb-file-per-table-tablspaces) tablespaces and using [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) to manually set the <a undefined>ENCRYPTED</a> table option to `YES`, InnoDB does <strong>not</strong> use background threads to encrypt the tablespaces.
- Similarly, when using [file-per-table](innodb-file-per-table-tablspaces) tablespaces and using [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) to manually set the <a undefined>ENCRYPTED</a> table option to `NO`, InnoDB does <strong>not</strong> use background threads to decrypt the tablespaces.

In these cases, InnoDB performs the encryption or decryption operation using the server thread for the client 
connection that executes the statement.  This means that you can update encryption on [file-per-table](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/) tablespaces with an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement, even when the <a undefined>innodb_encryption_threads</a> and/or the <a undefined>innodb_rotate_key_age</a> system variables are set to `0`.

InnoDB and XtraDB do not permit manual encryption changes to tables in the [system](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/) tablespace using [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/). Encryption of the [system](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/) tablespace can only be configured by setting the value of the <a undefined>innodb_encrypt_tables</a> system variable. This means that when you want to encrypt or decrypt the [system](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/) tablespace, you must also set a non-zero value for the <a undefined>innodb_encryption_threads</a> system variable, and you must also set the <a undefined>innodb_system_rotate_key_age</a> system variable to `1` to ensure that the system tablespace is properly encrypted or decrypted by the background threads. See [MDEV-14398](https://jira.mariadb.org/browse/MDEV-14398) for more information.

## Checking the Status of Background Operations

InnoDB records the status of background encryption operations in the <a undefined>INNODB_TABLESPACES_ENCRYPTION</a> table in the [information_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database.

For example, to see which InnoDB tablespaces are currently being decrypted or encrypted on by background encryption, you can check which InnoDB tablespaces have the `ROTATING_OR_FLUSHING` column set to `1`:

```sql
SELECT SPACE, NAME
FROM information_schema.INNODB_TABLESPACES_ENCRYPTION
WHERE ROTATING_OR_FLUSHING = 1;
```

And to see how many InnoDB tablespaces are currently being decrypted or encrypted by background encryption threads, you can call the [COUNT()](/built-in-functions/aggregate-functions/count/) aggregate function.

```sql
SELECT COUNT(*) AS 'encrypting' 
FROM information_schema.INNODB_TABLESPACES_ENCRYPTION
WHERE ROTATING_OR_FLUSHING = 1;
```

And to see how many InnoDB tablespaces are currently being decrypted or encrypted by background encryption threads, while comparing that to the total number of InnoDB tablespaces and the total number of encrypted InnoDB tablespaces, you can join the table with the [INNODB_SYS_TABLESPACES](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_sys_tablespaces-table/) table in the [information_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database:

```sql
/* information_schema.INNODB_TABLESPACES_ENCRYPTION does not always have rows for all tablespaces,
  so let's join it with information_schema.INNODB_SYS_TABLESPACES */
WITH tablespace_ids AS (
   SELECT SPACE
   FROM information_schema.INNODB_SYS_TABLESPACES ist
   UNION
   /* information_schema.INNODB_SYS_TABLESPACES doesn't have a row for the system tablespace (MDEV-20802) */
   SELECT 0 AS SPACE
)
SELECT NOW() as 'time', 
   'tablespaces', COUNT(*) AS 'tablespaces', 
   'encrypted', SUM(IF(ite.ENCRYPTION_SCHEME IS NOT NULL, ite.ENCRYPTION_SCHEME, 0)) AS 'encrypted', 
   'encrypting', SUM(IF(ite.ROTATING_OR_FLUSHING IS NOT NULL, ite.ROTATING_OR_FLUSHING, 0)) AS 'encrypting'
FROM tablespace_ids
LEFT JOIN information_schema.INNODB_TABLESPACES_ENCRYPTION ite
   ON tablespace_ids.SPACE = ite.SPACE
```