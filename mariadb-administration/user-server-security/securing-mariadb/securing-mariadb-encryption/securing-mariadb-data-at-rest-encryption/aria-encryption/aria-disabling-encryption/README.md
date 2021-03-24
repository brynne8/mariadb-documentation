# Aria Disabling Encryption

The process involved in safely disabling data-at-rest encryption for your Aria tables is very similar to that of enabling encryption.  To disable, you need to set the relevant system variables and then rebuild each table into an unencrypted state.

Don't remove the [Encryption Key Management](key-management-encryption-plugins) plugin from your configuration file until you have unencrypted all tables in your database.  MariaDB cannot read encrypted tables without the relevant encryption key.

## Disabling Encryption on User-created Tables

With tables that the user creates, you can disable encryption by setting the <a undefined>aria_encrypt_tables</a> system variable to `OFF`.  Once this is set, MariaDB no longer encrypts new tables created with the Aria storage engine.

```sql
SET GLOBAL aria_encrypt_tables = OFF;
```

Unlike [InnoDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/), Aria does not currently use background encryption threads.  Before removing the [Encryption Key Management](key-management-encryption-plugins) plugin from the configuration file, you first need to manually rebuild each table to an unencrypted state.

To find the encrypted tables, query the Information Schema, filtering the <a undefined>TABLES</a> table for those that use the Aria storage engine and the `PAGE` <a undefined>ROW_FORMAT</a>.

```sql
SELECT TABLE_SCHEMA, TABLE_NAME
FROM information_schema.TABLES
WHERE ENGINE = 'Aria'
  AND ROW_FORMAT = 'PAGE'
  AND TABLE_SCHEMA != 'information_schema';
```

Each table in the result-set was potentially written to disk in an encrypted state.  Before removing the configuration for the encryption keys, you need to rebuild each of these to an unencrypted state.  This can be done with an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement.

```sql
ALTER TABLE test.aria_table ENGINE = Aria ROW_FORMAT = PAGE;
```

Once all of the Aria tables are rebuilt, they're safely unencrypted.

## Disabling Encryption for Internal On-disk Temporary Tables

MariaDB routinely creates internal temporary tables.  When these temporary tables are written to disk and the <a undefined>aria_used_for_temp_tables</a> system variable is set to `ON`, MariaDB uses the Aria storage engine.

To decrypt these tables, set the <a undefined>encrypt_tmp_disk_tables</a> to `OFF`.  Once set, all internal temporary tables that are created from that point on are written unencrypted to disk.