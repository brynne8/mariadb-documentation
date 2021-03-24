# Aria Enabling Encryption

In order to enable data-at-rest encryption for tables using the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine, you first need to configure the server to use an [Encryption Key Management](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) plugin.  Once this is done, you can enable encryption by setting the relevant system variables.

## Encrypting User-created Tables

With tables that the user creates, you can enable encryption by setting the <a undefined>aria_encrypt_tables</a> system variable to `ON`, then restart the Server.  Once this is set, Aria automatically enables encryption on all tables you create after with the <a undefined>ROW_FORMAT</a> table option set to `PAGE`.

Currently, Aria does not support encryption on tables where the <a undefined>ROW_FORMAT</a> table option is set to the `FIXED` or `DYNAMIC` values.

Unlike InnoDB, Aria does not support the <a undefined>ENCRYPTED</a> table option (see [MDEV-18049](https://jira.mariadb.org/browse/MDEV-18049) about that). Encryption for Aria can only be enabled globally using the <a undefined>aria_encrypt_tables</a> system variable.

### Encrypting Existing Tables

In cases where you have existing Aria tables that you would like to encrypt, the process is a little more complicated.  Unlike InnoDB, Aria does not utilize [background encryption threads](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/innodb-background-encryption-threads/) to automatically perform encryption changes (see [MDEV-18971](https://jira.mariadb.org/browse/MDEV-18971) about that).  Therefore, to encrypt existing tables, you need to identify each table that needs to be encrypted, and then you need to manually rebuild each table.

First, set the <a undefined>aria_encrypt_tables</a> system variable to encrypt new tables.

```sql
SET GLOBAL aria_encrypt_tables=ON;
```

Identify Aria tables that have the <a undefined>ROW_FORMAT</a> table option set to `PAGE`.

```sql
SELECT TABLE_SCHEMA, TABLE_NAME 
FROM information_schema.TABLES 
WHERE ENGINE='Aria' 
  AND ROW_FORMAT='PAGE'
  AND TABLE_SCHEMA != 'information_schema';
```

For each table in the result-set, issue an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement to rebuild the table.

```sql
ALTER TABLE test.aria_table ENGINE=Aria ROW_FORMAT=PAGE;
```

This statement causes Aria to rebuild the table using the <a undefined>ROW_FORMAT</a> table option.  In the process, with the new default setting, it encrypts the table when it writes to disk.

## Encrypting Internal On-disk Temporary Tables

During the execution of queries, MariaDB routinely creates internal temporary tables.  These internal temporary tables initially use the [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) storage engine, which is entirely stored in memory.  When the table size exceeds the allocation defined by the <a undefined>max_heap_table_size</a> system variable, MariaDB writes the data to disk using another storage engine.  If you have the <a undefined>aria_used_for_temp_tables</a> set to `ON`, MariaDB uses Aria in writing the internal temporary tables to disk.

Encryption for internal temporary tables is handled separately from encryption for user-created tables.  To enable encryption for these tables, set the <a undefined>encrypt_tmp_disk_tables</a> system variable to `ON`.  Once set, all internal temporary tables that are written to disk using Aria are automatically encrypted.

## Manually Encrypting Tables

Currently, Aria does not support manually encrypting tables through the <a undefined>ENCRYPTED</a> and <a undefined>ENCRYPTION_KEY_ID</a> table options.  For more information, see [MDEV-18049](https://jira.mariadb.org/browse/MDEV-18049).

In cases where you want to encrypt tables manually or set the specific encryption key, use [InnoDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/).