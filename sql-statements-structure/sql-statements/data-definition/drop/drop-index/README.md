# DROP INDEX

## Syntax

```sql
DROP INDEX [IF EXISTS] index_name ON tbl_name 
    [WAIT n |NOWAIT]
```

## Description

`DROP INDEX` drops the [index](/replication/optimization-and-tuning/optimization-and-indexes) named `index_name` from the table `tbl_name`.
This statement is mapped to an `ALTER TABLE` statement to drop the
index.

If another connection is using the table, a [metadata lock](/sql-statements-structure/sql-statements/transactions/metadata-locking) is active, and this statement will wait until the lock is released. This is also true for non-transactional tables.

See [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table).

Another shortcut, [CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index), allows the creation of an index.

To remove the primary key, ``PRIMARY`` must be specified as index_name. Note that [the quotes](/sql-statements-structure/sql-language-structure/identifier-qualifiers) are necessary, because `PRIMARY` is a keyword.

## Privileges

Executing the `DROP INDEX` statement requires the <a undefined>INDEX</a> privilege for the table or the database.

## Online DDL

Online DDL is used by default with InnoDB, when the drop index operation supports it.

See [InnoDB Online DDL Overview](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/innodb-online-ddl-overview) for more information on online DDL with [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb).

## `DROP INDEX IF EXISTS ...`

If the `IF EXISTS` clause is used, then MariaDB will return a warning instead of an error if the index does not exist.

## `WAIT/NOWAIT`

##### MariaDB starting with [10.3.0](/kb/en/mariadb-1030-release-notes/)

Set the lock wait timeout. See [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait).

## Progress Reporting

MariaDB provides progress reporting for `DROP INDEX` statement for clients
that support the new progress reporting protocol. For example, if you were using the [mysql](/clients-utilities/mysql-client/mysql-command-line-client) client, then the progress report might look like this::

## See Also

- [Getting Started with Indexes](/replication/optimization-and-tuning/optimization-and-indexes/getting-started-with-indexes)
- [CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index)
- [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table)