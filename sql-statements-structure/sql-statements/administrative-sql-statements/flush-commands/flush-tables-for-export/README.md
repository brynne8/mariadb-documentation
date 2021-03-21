# FLUSH TABLES FOR EXPORT

## Syntax

```sql
FLUSH TABLES table_name [, table_name] FOR EXPORT
```

## Description

`FLUSH TABLES ... FOR EXPORT` flushes changes to the specified tables to disk so that binary copies can be made while the server is still running. This works for [Archive](/columns-storage-engines-and-plugins/storage-engines/archive), [Aria](/columns-storage-engines-and-plugins/storage-engines/aria), [CSV](/columns-storage-engines-and-plugins/storage-engines/csv), [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb), [MyISAM](/kb/en/myisam/), [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge), and [XtraDB](/kb/en/xtradb/) tables.

The table is read locked until one has issued [UNLOCK TABLES](/kb/en/lock-tables-and-unlock-tables/).

If a storage engine does not support `FLUSH TABLES FOR EXPORT`, a 1031 error ([SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate) 'HY000') is produced.

If `FLUSH TABLES ... FOR EXPORT` is in effect in the session, the following statements will produce an error if attempted:

- `FLUSH TABLES WITH READ LOCK`
- `FLUSH TABLES ... WITH READ LOCK`
- `FLUSH TABLES ... FOR EXPORT`
- Any statement trying to update any table

If any of the following statements is in effect in the session, attempting ` FLUSH TABLES ... FOR EXPORT` will produce an error.

- `FLUSH TABLES ... WITH READ LOCK`
- `FLUSH TABLES ... FOR EXPORT`
- `LOCK TABLES ... READ`
- `LOCK TABLES ... WRITE`

`FLUSH FOR EXPORT` is not written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).

This statement requires the [RELOAD](/kb/en/grant/#global-privileges) and the [LOCK TABLES](/kb/en/grant/#database-privileges) privileges.

If one of the specified tables cannot be locked, none of the tables will be locked.

If a table does not exist, an error like the following will be produced:

```sql
ERROR 1146 (42S02): Table 'test.xxx' doesn't exist
```

If a table is a view, an error like the following will be produced:

```sql
ERROR 1347 (HY000): 'test.v' is not BASE TABLE
```

## Example

```sql
FLUSH TABLES test.t1 FOR EXPORT;
#  Copy files related to the table (see below)
UNLOCK TABLES;
```

For a full description, please see [copying MariaDB tables](/mariadb-administration/copying-tables-between-different-mariadb-databases-and-mariadb-servers).

## See Also

- [Copying Tables Between Different MariaDB Databases and MariaDB Servers](/mariadb-administration/copying-tables-between-different-mariadb-databases-and-mariadb-servers)
- [Copying Transportable InnoDB Tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces)
- [myisampack](/clients-utilities/myisam-clients-and-utilities/myisampack)  - Compressing the MyISAM data file for easier distribution.
- [aria_pack](/clients-utilities/aria-clients-and-utilities/aria_pack) - Compressing the Aria data file for easier distribution