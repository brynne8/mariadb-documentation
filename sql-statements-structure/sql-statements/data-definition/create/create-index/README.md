# CREATE INDEX

## Syntax

```sql
CREATE [OR REPLACE] [UNIQUE|FULLTEXT|SPATIAL] INDEX 
  [IF NOT EXISTS] index_name
    [index_type]
    ON tbl_name (index_col_name,...)
    [WAIT n | NOWAIT]
    [index_option]
    [algorithm_option | lock_option] ...

index_col_name:
    col_name [(length)] [ASC | DESC]

index_type:
    USING {BTREE | HASH | RTREE}

index_option:
    KEY_BLOCK_SIZE [=] value
  | index_type
  | WITH PARSER parser_name
  | COMMENT 'string'

algorithm_option:
    ALGORITHM [=] {DEFAULT|INPLACE|COPY|NOCOPY|INSTANT}

lock_option:
    LOCK [=] {DEFAULT|NONE|SHARED|EXCLUSIVE}
```

## Description

CREATE INDEX is mapped to an ALTER TABLE statement to create [indexes](/replication/optimization-and-tuning/optimization-and-indexes).
See [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table). CREATE INDEX cannot be used to create a
PRIMARY KEY; use ALTER TABLE instead.

If another connection is using the table, a [metadata lock](/sql-statements-structure/sql-statements/transactions/metadata-locking) is active, and this statement will wait until the lock is released. This is also true for non-transactional tables.

Another shortcut, [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index), allows the removal of an index.

For valid identifiers to use as index names, see [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names).

Note that KEY_BLOCK_SIZE is currently ignored in CREATE INDEX, although it is included in the output of [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table).

## Privileges

Executing the `CREATE INDEX` statement requires the <a undefined>INDEX</a> privilege for the table or the database.

## Online DDL

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later, online DDL is supported with the <a undefined>ALGORITHM</a> and <a undefined>LOCK</a> clauses.

See [InnoDB Online DDL Overview](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/innodb-online-ddl-overview) for more information on online DDL with [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb).

## `CREATE OR REPLACE INDEX ...`

##### MariaDB starting with [10.1.4](/kb/en/mariadb-1014-release-notes/)

The `OR REPLACE` clause was added in [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/).

If the `OR REPLACE` clause is used and if the index already exists, then instead of returning an error, the server will drop the existing index and replace it with the newly defined index.

## `CREATE INDEX IF NOT EXISTS ...`

If the `IF NOT EXISTS` clause is used, then the index will only be created if an index with the same name does not already exist. If the index already exists, then a warning will be triggered by default.

## Index Definitions

See [CREATE TABLE: Index Definitions](/kb/en/create-table/#index-definitions) for information about index definitions.

## `WAIT/NOWAIT`

##### MariaDB starting with [10.3.0](/kb/en/mariadb-1030-release-notes/)

Set the lock wait timeout. See [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait).

## `ALGORITHM`

See [ALTER TABLE: ALGORITHM](/kb/en/alter-table/#algorithm) for more information.

## `LOCK`

See [ALTER TABLE: LOCK](/kb/en/alter-table/#lock) for more information.

## Progress Reporting

MariaDB provides progress reporting for `CREATE INDEX` statement for clients
that support the new progress reporting protocol. For example, if you were using the [mysql](/clients-utilities/mysql-client/mysql-command-line-client) client, then the progress report might look like this::

```sql
CREATE INDEX ON tab (num);;
Stage: 1 of 2 'copy to tmp table'    46% of stage
```

The progress report is also shown in the output of the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist) statement and in the contents of the <a undefined>information_schema.PROCESSLIST</a> table.

See [Progress Reporting](/kb/en/progress-reporting/) for more information.

## WITHOUT OVERLAPS

##### MariaDB starting with [10.5.3](/kb/en/mariadb-1053-release-notes/)

The [WITHOUT OVERLAPS](/kb/en/application-time-periods/#without-overlaps) clause allows one to constrain a primary or unique index such that [application-time periods](/sql-statements-structure/temporal-tables/application-time-periods) cannot overlap.

## Examples

Creating a unique index:

```sql
CREATE UNIQUE INDEX HomePhone ON Employees(Home_Phone);
```

OR REPLACE and IF NOT EXISTS:

```sql
CREATE INDEX xi ON xx5 (x);
Query OK, 0 rows affected (0.03 sec)

CREATE INDEX xi ON xx5 (x);
ERROR 1061 (42000): Duplicate key name 'xi'

CREATE OR REPLACE INDEX xi ON xx5 (x);
Query OK, 0 rows affected (0.03 sec)

CREATE INDEX IF NOT EXISTS xi ON xx5 (x);
Query OK, 0 rows affected, 1 warning (0.00 sec)

SHOW WARNINGS;
+-------+------+-------------------------+
| Level | Code | Message                 |
+-------+------+-------------------------+
| Note  | 1061 | Duplicate key name 'xi' |
+-------+------+-------------------------+
```

From [MariaDB 10.5.3](/kb/en/mariadb-1053-release-notes/), creating a unique index for an [application-time period table](/sql-statements-structure/temporal-tables/application-time-periods) with a [WITHOUT OVERLAPS](/kb/en/application-time-periods/#without-overlaps) constraint:

```sql
CREATE UNIQUE INDEX u ON rooms (room_number, p WITHOUT OVERLAPS);
```

## See Also

- [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names)
- [Getting Started with Indexes](/replication/optimization-and-tuning/optimization-and-indexes/getting-started-with-indexes)
- [What is an Index?](/kb/en/what-is-an-index/)
- [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table)
- [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index)
- [SPATIAL INDEX](/sql-statements-structure/geographic-geometric-features/spatial-index)
- [Full-text Indexes](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes)
- [WITHOUT OVERLAPS](/kb/en/application-time-periods/#without-overlaps)