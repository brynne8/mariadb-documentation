# InnoDB Online DDL Operations with the NOCOPY Alter Algorithm

## Supported Operations by Inheritance

When the <a undefined>ALGORITHM</a> clause is set to `NOCOPY`, the supported operations are a superset of the operations that are supported when the <a undefined>ALGORITHM</a> clause is set to `INSTANT`.

Therefore, when the <a undefined>ALGORITHM</a> clause is set to `NOCOPY`, some operations are supported by inheritance. See the following additional pages for more information about these supported operations:

- [InnoDB Online DDL Operations with ALGORITHM=INSTANT](/kb/en/innodb-online-ddl-operations-with-algorithminstant/)

## Column Operations

### `ALTER TABLE ... ADD COLUMN`

In [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/) and later, InnoDB supports adding columns to a table with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: ALTER TABLE ... ADD COLUMN](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#alter-table-add-column) for more information.

This applies to <a undefined>ALTER TABLE ... ADD COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... DROP COLUMN`

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, InnoDB supports dropping columns from a table with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: ALTER TABLE ... DROP COLUMN](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#alter-table-drop-column) for more information.

This applies to <a undefined>ALTER TABLE ... DROP COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... MODIFY COLUMN`

This applies to <a undefined>ALTER TABLE ... MODIFY COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

#### Reordering Columns

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, InnoDB supports reordering columns within a table with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: Reordering Columns](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#reordering-columns) for more information.

#### Changing the Data Type of a Column

InnoDB does <strong>not</strong> support modifying a column's data type with <a undefined>ALGORITHM</a> set to `NOCOPY` in most cases. There are a few exceptions in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: Changing the Data Type of a Column](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#changing-the-data-type-of-a-column) for more information.

#### Changing a Column to NULL

In [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later, InnoDB supports modifying a column to allow <a undefined>NULL</a> values with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: Changing a Column to NULL](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#changing-a-column-to-null) for more information.

#### Changing a Column to NOT NULL

InnoDB does <strong>not</strong> support modifying a column to <strong>not</strong> allow <a undefined>NULL</a> values with <a undefined>ALGORITHM</a> set to `NOCOPY`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) ROW_FORMAT=REDUNDANT;

SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab MODIFY COLUMN c varchar(50) NOT NULL;
ERROR 1845 (0A000): ALGORITHM=NOCOPY is not supported for this operation. Try ALGORITHM=INPLACE
```

#### Adding a New `ENUM` Option

InnoDB supports adding a new [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum/) option to a column with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: Adding a New ENUM Option](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#adding-a-new-enum-option) for more information.

#### Adding a New `SET` Option

InnoDB supports adding a new [SET](/columns-storage-engines-and-plugins/data-types/string-data-types/set-data-type/) option to a column with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: Adding a New SET Option](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#adding-a-new-set-option) for more information.

#### Removing System Versioning from a Column

In [MariaDB 10.3.8](/kb/en/mariadb-1038-release-notes/) and later, InnoDB supports removing [system versioning](/sql-statements-structure/temporal-tables/system-versioned-tables/) from a column with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: Removing System Versioning from a Column](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#removing-system-versioning-from-a-column) for more information.

### `ALTER TABLE ... ALTER COLUMN`

This applies to <a undefined>ALTER TABLE ... ALTER COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

#### Setting a Column's Default Value

InnoDB supports modifying a column's <a undefined>DEFAULT</a> value with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: Setting a Column's Default Value](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#setting-a-columns-default-value) for more information.

#### Removing a Column's Default Value

InnoDB supports removing a column's  <a undefined>DEFAULT</a> value with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: Removing a Column's Default Value](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#removing-a-columns-default-value) for more information.

### `ALTER TABLE ... CHANGE COLUMN`

InnoDB supports renaming a column with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: ALTER TABLE ... CHANGE COLUMN](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#alter-table-change-column) for more information.

This applies to <a undefined>ALTER TABLE ... CHANGE COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

## Index Operations

### `ALTER TABLE ... ADD PRIMARY KEY`

InnoDB does <strong>not</strong> support adding a primary key to a table with <a undefined>ALGORITHM</a> set to `NOCOPY`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int,
   b varchar(50),
   c varchar(50)
);

SET SESSION sql_mode='STRICT_TRANS_TABLES';
SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab ADD PRIMARY KEY (a);
ERROR 1845 (0A000): ALGORITHM=NOCOPY is not supported for this operation. Try ALGORITHM=INPLACE
```

This applies to <a undefined>ALTER TABLE ... ADD PRIMARY KEY</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... DROP PRIMARY KEY`

InnoDB does <strong>not</strong> support dropping a primary key with <a undefined>ALGORITHM</a> set to `NOCOPY`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab DROP PRIMARY KEY;
ERROR 1846 (0A000): ALGORITHM=NOCOPY is not supported. Reason: Dropping a primary key is not allowed without also adding a new primary key. Try ALGORITHM=COPY
```

This applies to <a undefined>ALTER TABLE ... DROP PRIMARY KEY</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... ADD INDEX` and `CREATE INDEX`

This applies to <a undefined>ALTER TABLE ... ADD INDEX</a> and [CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index/) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

#### Adding a Plain Index

InnoDB supports adding a plain index to a table with <a undefined>ALGORITHM</a> set to `NOCOPY`.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab ADD INDEX b_index (b);
Query OK, 0 rows affected (0.009 sec)
```

And this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='NOCOPY';
CREATE INDEX b_index ON tab (b);
Query OK, 0 rows affected (0.009 sec)
```

#### Adding a Fulltext Index

InnoDB supports adding a [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) index to a table with <a undefined>ALGORITHM</a> set to `NOCOPY`.

However, there are some limitations, such as:

- Adding a [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) index to a table that does not have a user-defined `FTS_DOC_ID` column will require the table to be rebuilt once. When the table is rebuilt, the system adds a hidden `FTS_DOC_ID` column. This initial operation will have to be performed with <a undefined>ALGORITHM</a> set to `INPLACE`.From that point forward, adding additional [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) indexes to the same table will not require the table to be rebuilt, and <a undefined>ALGORITHM</a> can be set to `NOCOPY`.

- Only one [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) index may be added at a time when <a undefined>ALGORITHM</a> is set to `NOCOPY`.

This operation supports a read-only locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `SHARED`. When this strategy is used, read-only concurrent DML is permitted.

For example, this succeeds, but the first operation requires the table to be rebuilt <a undefined>ALGORITHM</a> set to `INPLACE`, so that the hidden `FTS_DOC_ID` column can be added:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD FULLTEXT INDEX b_index (b);
Query OK, 0 rows affected (0.043 sec)

SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab ADD FULLTEXT INDEX c_index (c);
Query OK, 0 rows affected (0.017 sec)
```

And this succeeds in the same way as above:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
CREATE FULLTEXT INDEX b_index ON tab (b);
Query OK, 0 rows affected (0.048 sec)

SET SESSION alter_algorithm='NOCOPY';
CREATE FULLTEXT INDEX c_index ON tab (c);
Query OK, 0 rows affected (0.016 sec)
```

But this second command fails, because only one [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) index can be added at a time:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50),
   d varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD FULLTEXT INDEX b_index (b);
Query OK, 0 rows affected (0.041 sec)

SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab ADD FULLTEXT INDEX c_index (c), ADD FULLTEXT INDEX d_index (d);
ERROR 1846 (0A000): ALGORITHM=NOCOPY is not supported. Reason: InnoDB presently supports one FULLTEXT index creation at a time. Try ALGORITHM=COPY
```

#### Adding a Spatial Index

InnoDB supports adding a [SPATIAL](/sql-statements-structure/geographic-geometric-features/spatial-index/) index to a table with <a undefined>ALGORITHM</a> set to `NOCOPY`.

This operation supports a read-only locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `SHARED`. When this strategy is used, read-only concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c GEOMETRY NOT NULL
);

SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab ADD SPATIAL INDEX c_index (c);
Query OK, 0 rows affected (0.005 sec)
```

And this succeeds in the same way as above:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c GEOMETRY NOT NULL
);

SET SESSION alter_algorithm='NOCOPY';
CREATE SPATIAL INDEX c_index ON tab (c);
Query OK, 0 rows affected (0.005 sec)
```

### `ALTER TABLE ... DROP INDEX` and `DROP INDEX`

InnoDB supports dropping indexes from a table with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: ALTER TABLE ... DROP INDEX and DROP INDEX](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#alter-table-drop-index-and-drop-index) for more information.

This applies to <a undefined>ALTER TABLE ... DROP INDEX</a> and [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index/) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... ADD FOREIGN KEY`

InnoDB does supports adding foreign key constraints to a table with <a undefined>ALGORITHM</a> set to `NOCOPY`. In order to add a new foreign key constraint to a table with <a undefined>ALGORITHM</a> set to `NOCOPY`, the <a undefined>foreign_key_checks</a> system variable needs to be set to `OFF`. If it is set to `ON`, then `ALGORITHM=COPY` is required.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this fails:

```sql
CREATE OR REPLACE TABLE tab1 (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50),
   d int
);

CREATE OR REPLACE TABLE tab2 (
   a int PRIMARY KEY,
   b varchar(50)
);

SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab1 ADD FOREIGN KEY tab2_fk (d) REFERENCES tab2 (a);
ERROR 1846 (0A000): ALGORITHM=NOCOPY is not supported. Reason: Adding foreign keys needs foreign_key_checks=OFF. Try ALGORITHM=COPY
```

But this succeeds:

```sql
CREATE OR REPLACE TABLE tab1 (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50),
   d int
);

CREATE OR REPLACE TABLE tab2 (
   a int PRIMARY KEY,
   b varchar(50)
);

SET SESSION foreign_key_checks=OFF;
SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab1 ADD FOREIGN KEY tab2_fk (d) REFERENCES tab2 (a);
Query OK, 0 rows affected (0.011 sec)
```

This applies to <a undefined>ALTER TABLE ... ADD FOREIGN KEY</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... DROP FOREIGN KEY`

InnoDB supports dropping foreign key constraints from a table with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: ALTER TABLE ... DROP FOREIGN KEY](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#alter-table-drop-foreign-key) for more information.

This applies to <a undefined>ALTER TABLE ... DROP FOREIGN KEY</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

## Table Operations

### `ALTER TABLE ... AUTO_INCREMENT=...`

InnoDB supports changing a table's [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) value with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: ALTER TABLE ... AUTO_INCREMENT=...](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#alter-table-auto_increment) for more information.

This applies to <a undefined>ALTER TABLE ... AUTO_INCREMENT=...</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... ROW_FORMAT=...`

InnoDB does <strong>not</strong> support changing a table's [row format](/kb/en/innodb-storage-formats/) with <a undefined>ALGORITHM</a> set to `NOCOPY`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) ROW_FORMAT=DYNAMIC;

SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab ROW_FORMAT=COMPRESSED;
ERROR 1846 (0A000): ALGORITHM=NOCOPY is not supported. Reason: Changing table options requires the table to be rebuilt. Try ALGORITHM=INPLACE
```

This applies to <a undefined>ALTER TABLE ... ROW_FORMAT=...</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... KEY_BLOCK_SIZE=...`

InnoDB does <strong>not</strong> support changing a table's <a undefined>KEY_BLOCK_SIZE</a> with <a undefined>ALGORITHM</a> set to `NOCOPY`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) ROW_FORMAT=COMPRESSED
  KEY_BLOCK_SIZE=4;

SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab KEY_BLOCK_SIZE=2;
ERROR 1846 (0A000): ALGORITHM=NOCOPY is not supported. Reason: Changing table options requires the table to be rebuilt. Try ALGORITHM=INPLACE
```

This applies to <a undefined>KEY_BLOCK_SIZE=...</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... PAGE_COMPRESSED=1` and `ALTER TABLE ... PAGE_COMPRESSION_LEVEL=...`

In [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/) and later, InnoDB supports setting a table's <a undefined>PAGE_COMPRESSED</a> value to `1` with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

InnoDB does <strong>not</strong> support changing a table's <a undefined>PAGE_COMPRESSED</a> value from `1` to `0` with <a undefined>ALGORITHM</a> set to `NOCOPY`.

In these versions, InnoDB also supports changing a table's <a undefined>PAGE_COMPRESSION_LEVEL</a> value with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause is set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: ALTER TABLE ... PAGE_COMPRESSED=1 and ALTER TABLE ... PAGE_COMPRESSION_LEVEL=...](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#alter-table-page_compressed1-and-alter-table-page_compression_level) for more information.

This applies to <a undefined>ALTER TABLE ... PAGE_COMPRESSED=...</a> and <a undefined>ALTER TABLE ... PAGE_COMPRESSION_LEVEL=...</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... DROP SYSTEM VERSIONING`

InnoDB does <strong>not</strong> support dropping [system versioning](/sql-statements-structure/temporal-tables/system-versioned-tables/) from a table with <a undefined>ALGORITHM</a> set to `NOCOPY`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) WITH SYSTEM VERSIONING;

SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab DROP SYSTEM VERSIONING;
ERROR 1845 (0A000): ALGORITHM=NOCOPY is not supported for this operation. Try ALGORITHM=INPLACE
```

This applies to <a undefined>ALTER TABLE ... DROP SYSTEM VERSIONING</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... DROP CONSTRAINT`

In [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/) and later, InnoDB supports dropping a <a undefined>CHECK</a> constraint from a table with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: ALTER TABLE ... DROP CONSTRAINT](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#alter-table-drop-constraint) for more information.

This applies to <a undefined>ALTER TABLE ... DROP CONSTRAINT</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... FORCE`

InnoDB does <strong>not</strong> support forcing a table rebuild with <a undefined>ALGORITHM</a> set to `NOCOPY`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab FORCE;
ERROR 1845 (0A000): ALGORITHM=NOCOPY is not supported for this operation. Try ALGORITHM=INPLACE
```

This applies to <a undefined>ALTER TABLE ... FORCE</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... ENGINE=InnoDB`

InnoDB does <strong>not</strong> support forcing a table rebuild with <a undefined>ALGORITHM</a> set to `NOCOPY`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='NOCOPY';
ALTER TABLE tab ENGINE=InnoDB;
ERROR 1845 (0A000): ALGORITHM=NOCOPY is not supported for this operation. Try ALGORITHM=INPLACE
```

This applies to <a undefined>ALTER TABLE ... ENGINE=InnoDB</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `OPTIMIZE TABLE ...`

InnoDB does <strong>not</strong> support optimizing a table with with <a undefined>ALGORITHM</a> set to `NOCOPY`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SHOW GLOBAL VARIABLES WHERE Variable_name IN('innodb_defragment', 'innodb_optimize_fulltext_only');
+-------------------------------+-------+
| Variable_name                 | Value |
+-------------------------------+-------+
| innodb_defragment             | OFF   |
| innodb_optimize_fulltext_only | OFF   |
+-------------------------------+-------+
2 rows in set (0.001 sec)

SET SESSION alter_algorithm='NOCOPY';
OPTIMIZE TABLE tab;
+---------+----------+----------+-----------------------------------------------------------------------------+
| Table   | Op       | Msg_type | Msg_text                                                                    |
+---------+----------+----------+-----------------------------------------------------------------------------+
| db1.tab | optimize | note     | Table does not support optimize, doing recreate + analyze instead           |
| db1.tab | optimize | error    | ALGORITHM=NOCOPY is not supported for this operation. Try ALGORITHM=INPLACE |
| db1.tab | optimize | status   | Operation failed                                                            |
+---------+----------+----------+-----------------------------------------------------------------------------+
3 rows in set, 1 warning (0.002 sec)
```

This applies to [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... RENAME TO` and `RENAME TABLE ...`

InnoDB supports renaming a table with <a undefined>ALGORITHM</a> set to `NOCOPY` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: ALTER TABLE ... RENAME TO and RENAME TABLE ...](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#alter-table-rename-to-and-rename-table) for more information.

This applies to <a undefined>ALTER TABLE ... RENAME TO</a> and [RENAME TABLE](/sql-statements-structure/sql-statements/data-definition/rename-table/) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

## Limitations

### Limitations Related to Generated (Virtual and Persistent/Stored) Columns

[Generated columns](/sql-statements-structure/sql-statements/data-definition/create/generated-columns/) do not currently support online DDL for all of the same operations that are supported for "real" columns.

See [Generated (Virtual and Persistent/Stored) Columns: Statement Support](/kb/en/generated-columns/#statement-support) for more information on the limitations.