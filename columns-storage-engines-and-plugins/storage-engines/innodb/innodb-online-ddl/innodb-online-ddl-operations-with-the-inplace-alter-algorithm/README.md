# InnoDB Online DDL Operations with the INPLACE Alter Algorithm

## Supported Operations by Inheritance

When the <a undefined>ALGORITHM</a> clause is set to `INPLACE`, the supported operations are a superset of the operations that are supported when the <a undefined>ALGORITHM</a> clause is set to `NOCOPY`. Similarly, when the <a undefined>ALGORITHM</a> clause is set to `NOCOPY`, the supported operations are a superset of the operations that are supported when the <a undefined>ALGORITHM</a> clause is set to `INSTANT`.

Therefore, when the <a undefined>ALGORITHM</a> clause is set to `INPLACE`, some operations are supported by inheritance. See the following additional pages for more information about these supported operations:

- [InnoDB Online DDL Operations with ALGORITHM=NOCOPY](/kb/en/innodb-online-ddl-operations-with-algorithmnocopy/)
- [InnoDB Online DDL Operations with ALGORITHM=INSTANT](/kb/en/innodb-online-ddl-operations-with-algorithminstant/)

## Column Operations

### `ALTER TABLE ... ADD COLUMN`

InnoDB supports adding columns to a table with <a undefined>ALGORITHM</a> set to `INPLACE`.

The table is rebuilt, which means that all of the data is reorganized substantially, and the indexes are rebuilt. As a result, the operation is quite expensive.

With the exception of adding an [auto-increment](/columns-storage-engines-and-plugins/data-types/auto_increment) column, this operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD COLUMN c varchar(50);
Query OK, 0 rows affected (0.006 sec)
```

This applies to <a undefined>ALTER TABLE ... ADD COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... DROP COLUMN`

InnoDB supports dropping columns from a table with <a undefined>ALGORITHM</a> set to `INPLACE`.

The table is rebuilt, which means that all of the data is reorganized substantially, and the indexes are rebuilt. As a result, the operation is quite expensive.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab DROP COLUMN c;
Query OK, 0 rows affected (0.021 sec)
```

This applies to <a undefined>ALTER TABLE ... DROP COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... MODIFY COLUMN`

This applies to <a undefined>ALTER TABLE ... MODIFY COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

#### Reordering Columns

InnoDB supports reordering columns within a table with <a undefined>ALGORITHM</a> set to `INPLACE`.

The table is rebuilt, which means that all of the data is reorganized substantially, and the indexes are rebuilt. As a result, the operation is quite expensive.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab MODIFY COLUMN c varchar(50) AFTER a;
Query OK, 0 rows affected (0.022 sec)
```

#### Changing the Data Type of a Column

InnoDB does <strong>not</strong> support modifying a column's data type with <a undefined>ALGORITHM</a> set to `INPLACE` in most cases. There are some exceptions:

- In [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and later, InnoDB supports increasing the length of `VARCHAR` columns with <a undefined>ALGORITHM</a> set to `INPLACE`, unless it would require changing the number of bytes requires to represent the column's length. A `VARCHAR` column that is between 0 and 255 bytes in size requires 1 byte to represent its length, while a `VARCHAR` column that is 256 bytes or longer requires 2 bytes to represent its length. This means that the length of a column cannot be increased with <a undefined>ALGORITHM</a> set to `INPLACE` if the original length was less than 256 bytes, and the new length is 256 bytes or more.

- In [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later, InnoDB supports increasing the length of `VARCHAR` columns with <a undefined>ALGORITHM</a> set to `INPLACE` in the cases where the operation supports having the <a undefined>ALGORITHM</a> clause set to `INSTANT`.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: Changing the Data Type of a Column](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#changing-the-data-type-of-a-column) for more information.

For example, this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab MODIFY COLUMN c int;
ERROR 1846 (0A000): ALGORITHM=INPLACE is not supported. Reason: Cannot change column type INPLACE. Try ALGORITHM=COPY
```

But this succeeds in [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and later, because the original length of the column is less than 256 bytes, and the new length is still less than 256 bytes:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) CHARACTER SET=latin1;

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab MODIFY COLUMN c varchar(100);
Query OK, 0 rows affected (0.005 sec)
```

But this fails in [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and later, because the original length of the column is less than 256 bytes, and the new length is greater than 256 bytes:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(255)
) CHARACTER SET=latin1;

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab MODIFY COLUMN c varchar(256);
ERROR 1846 (0A000): ALGORITHM=INPLACE is not supported. Reason: Cannot change column type INPLACE. Try ALGORITHM=COPY
```

#### Changing a Column to NULL

InnoDB supports modifying a column to allow <a undefined>NULL</a> values with <a undefined>ALGORITHM</a> set to `INPLACE`.

The table is rebuilt, which means that all of the data is reorganized substantially, and the indexes are rebuilt. As a result, the operation is quite expensive.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50) NOT NULL
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab MODIFY COLUMN c varchar(50) NULL;
Query OK, 0 rows affected (0.021 sec)
```

#### Changing a Column to NOT NULL

InnoDB supports modifying a column to <strong>not</strong> allow <a undefined>NULL</a> values with <a undefined>ALGORITHM</a> set to `INPLACE`. It is required for [strict mode](/kb/en/sql-mode/#strict-mode) to be enabled in [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode). The operation will fail if the column contains any `NULL` values. Changes that would interfere with referential integrity are also not permitted.

The table is rebuilt, which means that all of the data is reorganized substantially, and the indexes are rebuilt. As a result, the operation is quite expensive.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab MODIFY COLUMN c varchar(50) NOT NULL;
Query OK, 0 rows affected (0.021 sec)
```

#### Adding a New `ENUM` Option

InnoDB supports adding a new [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum) option to a column with <a undefined>ALGORITHM</a> set to `INPLACE`. In order to add a new [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum) option with <a undefined>ALGORITHM</a> set to `INPLACE`, the following requirements must be met:

- It must be added to the end of the list.
- The storage requirements must not change.

This operation only changes the table's metadata, so the table does not have to be rebuilt..

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c ENUM('red', 'green')
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab MODIFY COLUMN c ENUM('red', 'green', 'blue');
Query OK, 0 rows affected (0.004 sec)
```

But this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c ENUM('red', 'green')
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab MODIFY COLUMN c ENUM('red', 'blue', 'green');
ERROR 1846 (0A000): ALGORITHM=INPLACE is not supported. Reason: Cannot change column type INPLACE. Try ALGORITHM=COPY
```

#### Adding a New `SET` Option

InnoDB supports adding a new [SET](/columns-storage-engines-and-plugins/data-types/string-data-types/set-data-type) option to a column with <a undefined>ALGORITHM</a> set to `INPLACE`. In order to add a new [SET](/columns-storage-engines-and-plugins/data-types/string-data-types/set-data-type) option with <a undefined>ALGORITHM</a> set to `INPLACE`, the following requirements must be met:

- It must be added to the end of the list.
- The storage requirements must not change.

This operation only changes the table's metadata, so the table does not have to be rebuilt..

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c SET('red', 'green')
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab MODIFY COLUMN c SET('red', 'green', 'blue');
Query OK, 0 rows affected (0.004 sec)
```

But this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c SET('red', 'green')
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab MODIFY COLUMN c SET('red', 'blue', 'green');
ERROR 1846 (0A000): ALGORITHM=INPLACE is not supported. Reason: Cannot change column type INPLACE. Try ALGORITHM=COPY
```

#### Removing System Versioning from a Column

In [MariaDB 10.3.8](/kb/en/mariadb-1038-release-notes/) and later, InnoDB supports removing [system versioning](/sql-statements-structure/temporal-tables/system-versioned-tables) from a column with <a undefined>ALGORITHM</a> set to `INPLACE`. In order for this to work, the <a undefined>system_versioning_alter_history</a> system variable must be set to `KEEP`. See [MDEV-16330](https://jira.mariadb.org/browse/MDEV-16330) for more information.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50) WITH SYSTEM VERSIONING
);

SET SESSION system_versioning_alter_history='KEEP';
SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab MODIFY COLUMN c varchar(50) WITHOUT SYSTEM VERSIONING;
Query OK, 0 rows affected (0.005 sec)
```

### `ALTER TABLE ... ALTER COLUMN`

This applies to <a undefined>ALTER TABLE ... ALTER COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

#### Setting a Column's Default Value

InnoDB supports modifying a column's <a undefined>DEFAULT</a> value with <a undefined>ALGORITHM</a> set to `INPLACE`.

This operation only changes the table's metadata, so the table does not have to be rebuilt.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.
For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ALTER COLUMN c SET DEFAULT 'No value explicitly provided.';
Query OK, 0 rows affected (0.005 sec)
```

#### Removing a Column's Default Value

InnoDB supports removing a column's  <a undefined>DEFAULT</a> value with <a undefined>ALGORITHM</a> set to `INPLACE`.

This operation only changes the table's metadata, so the table does not have to be rebuilt.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50) DEFAULT 'No value explicitly provided.'
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ALTER COLUMN c DROP DEFAULT;
Query OK, 0 rows affected (0.005 sec)
```

### `ALTER TABLE ... CHANGE COLUMN`

InnoDB supports renaming a column with <a undefined>ALGORITHM</a> set to `INPLACE`, unless the column's data type or attributes changed in addition to the name.

This operation only changes the table's metadata, so the table does not have to be rebuilt.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab CHANGE COLUMN c str varchar(50);
Query OK, 0 rows affected (0.006 sec)
```

But this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab CHANGE COLUMN c num int;
ERROR 1846 (0A000): ALGORITHM=INPLACE is not supported. Reason: Cannot change column type INPLACE. Try ALGORITHM=COPY
```

This applies to <a undefined>ALTER TABLE ... CHANGE COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

## Index Operations

### `ALTER TABLE ... ADD PRIMARY KEY`

InnoDB supports adding a primary key to a table with <a undefined>ALGORITHM</a> set to `INPLACE`.

If the new primary key column is not defined as <a undefined>NOT NULL</a>, then it is highly recommended for [strict mode](/kb/en/sql-mode/#strict-mode) to be enabled in [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode). Otherwise, `NULL` values will be silently converted to the default value for the given data type, which is probably not the desired behavior in this scenario.

The table is rebuilt, which means that all of the data is reorganized substantially, and the indexes are rebuilt. As a result, the operation is quite expensive.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int,
   b varchar(50),
   c varchar(50)
);

SET SESSION sql_mode='STRICT_TRANS_TABLES';
SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD PRIMARY KEY (a);
Query OK, 0 rows affected (0.021 sec)
```

But this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int,
   b varchar(50),
   c varchar(50)
);

INSERT INTO tab VALUES (NULL, NULL, NULL);

SET SESSION sql_mode='STRICT_TRANS_TABLES';
SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD PRIMARY KEY (a);
ERROR 1265 (01000): Data truncated for column 'a' at row 1
```

And this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int,
   b varchar(50),
   c varchar(50)
);

INSERT INTO tab VALUES (1, NULL, NULL);
INSERT INTO tab VALUES (1, NULL, NULL);

SET SESSION sql_mode='STRICT_TRANS_TABLES';
SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD PRIMARY KEY (a);
ERROR 1062 (23000): Duplicate entry '1' for key 'PRIMARY'
```

This applies to <a undefined>ALTER TABLE ... ADD PRIMARY KEY</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... DROP PRIMARY KEY`

InnoDB does <strong>not</strong> support dropping a primary key with <a undefined>ALGORITHM</a> set to `INPLACE` in most cases.

If you try to do so, then you will see an error. InnoDB only supports this operation with <a undefined>ALGORITHM</a> set to `COPY`. Concurrent DML is *not* permitted.

However, there is an exception. If you are dropping a primary key, and adding a new one at the same time, then that operation can be performed with <a undefined>ALGORITHM</a> set to `INPLACE`. This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab DROP PRIMARY KEY;
ERROR 1846 (0A000): ALGORITHM=INPLACE is not supported. Reason: Dropping a primary key is not allowed without also adding a new primary key. Try ALGORITHM=COPY
```

But this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION sql_mode='STRICT_TRANS_TABLES';
SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab DROP PRIMARY KEY, ADD PRIMARY KEY (b);
Query OK, 0 rows affected (0.020 sec)
```

This applies to <a undefined>ALTER TABLE ... DROP PRIMARY KEY</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... ADD INDEX` and `CREATE INDEX`

This applies to <a undefined>ALTER TABLE ... ADD INDEX</a> and [CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

#### Adding a Plain Index

InnoDB supports adding a plain index to a table with <a undefined>ALGORITHM</a> set to `INPLACE`. The table is not rebuilt.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD INDEX b_index (b);
Query OK, 0 rows affected (0.010 sec)
```

And this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
CREATE INDEX b_index ON tab (b);
Query OK, 0 rows affected (0.011 sec)
```

#### Adding a Fulltext Index

InnoDB supports adding a [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes) index to a table with <a undefined>ALGORITHM</a> set to `INPLACE`. The table is not rebuilt in some cases.

However, there are some limitations, such as:

- Adding a [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes) index to a table that does not have a user-defined `FTS_DOC_ID` column will require the table to be rebuilt once. When the table is rebuilt, the system adds a hidden `FTS_DOC_ID` column. From that point forward, adding additional [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes) indexes to the same table will not require the table to be rebuilt when <a undefined>ALGORITHM</a> is set to `INPLACE`.

- Only one [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes) index may be added at a time when <a undefined>ALGORITHM</a> is set to `INPLACE`.

- If a table has more than one [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes) index, then it cannot be rebuilt by any [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) operations when <a undefined>ALGORITHM</a> is set to `INPLACE`.

- If a table has a [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes) index, then it cannot be rebuilt by any [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) operations when the <a undefined>LOCK</a> clause is set to `NONE`.

This operation supports a read-only locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `SHARED`. When this strategy is used, read-only concurrent DML is permitted.

For example, this succeeds, but requires the table to be rebuilt, so that the hidden `FTS_DOC_ID` column can be added:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD FULLTEXT INDEX b_index (b);
Query OK, 0 rows affected (0.055 sec)
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
Query OK, 0 rows affected (0.041 sec)
```

And this succeeds, and the second command does not require the table to be rebuilt:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD FULLTEXT INDEX b_index (b);
Query OK, 0 rows affected (0.043 sec)

ALTER TABLE tab ADD FULLTEXT INDEX c_index (c);
Query OK, 0 rows affected (0.017 sec)
```

But this second command fails, because only one [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes) index can be added at a time:

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

ALTER TABLE tab ADD FULLTEXT INDEX c_index (c), ADD FULLTEXT INDEX d_index (d);
ERROR 1846 (0A000): ALGORITHM=INPLACE is not supported. Reason: InnoDB presently supports one FULLTEXT index creation at a time. Try ALGORITHM=COPY
```

And this third command fails, because a table cannot be rebuilt when it has more than one [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes) index:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD FULLTEXT INDEX b_index (b);
Query OK, 0 rows affected (0.040 sec)

ALTER TABLE tab ADD FULLTEXT INDEX c_index (c);
Query OK, 0 rows affected (0.015 sec)

ALTER TABLE tab FORCE;
ERROR 1846 (0A000): ALGORITHM=INPLACE is not supported. Reason: InnoDB presently supports one FULLTEXT index creation at a time. Try ALGORITHM=COPY
```

#### Adding a Spatial Index

InnoDB supports adding a [SPATIAL](/sql-statements-structure/geographic-geometric-features/spatial-index) index to a table with <a undefined>ALGORITHM</a> set to `INPLACE`.

However, there are some limitations, such as:

- If a table has a [SPATIAL](/sql-statements-structure/geographic-geometric-features/spatial-index) index, then it cannot be rebuilt by any [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) operations when the <a undefined>LOCK</a> clause is set to `NONE`.

This operation supports a read-only locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `SHARED`. When this strategy is used, read-only concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c GEOMETRY NOT NULL
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD SPATIAL INDEX c_index (c);
Query OK, 0 rows affected (0.006 sec)
```

And this succeeds in the same way as above:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c GEOMETRY NOT NULL
);

SET SESSION alter_algorithm='INPLACE';
CREATE SPATIAL INDEX c_index ON tab (c);
Query OK, 0 rows affected (0.006 sec)
```

### `ALTER TABLE ... DROP INDEX` and `DROP INDEX`

InnoDB supports dropping indexes from a table with <a undefined>ALGORITHM</a> set to `INPLACE`.

This operation only changes the table's metadata, so the table does not have to be rebuilt.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50),
   INDEX b_index (b)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab DROP INDEX b_index;
```

And this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50),
   INDEX b_index (b)
);

SET SESSION alter_algorithm='INPLACE';
DROP INDEX b_index ON tab;
```

This applies to <a undefined>ALTER TABLE ... DROP INDEX</a> and [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... ADD FOREIGN KEY`

InnoDB supports adding foreign key constraints to a table with <a undefined>ALGORITHM</a> set to `INPLACE`. In order to add a new foreign key constraint to a table with <a undefined>ALGORITHM</a> set to `INPLACE`, the <a undefined>foreign_key_checks</a> system variable needs to be set to `OFF`. If it is set to `ON`, then `ALGORITHM=COPY` is required.

This operation only changes the table's metadata, so the table does not have to be rebuilt.

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

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab1 ADD FOREIGN KEY tab2_fk (d) REFERENCES tab2 (a);
ERROR 1846 (0A000): ALGORITHM=INPLACE is not supported. Reason: Adding foreign keys needs foreign_key_checks=OFF. Try ALGORITHM=COPY
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
SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab1 ADD FOREIGN KEY tab2_fk (d) REFERENCES tab2 (a);
Query OK, 0 rows affected (0.011 sec)
```

This applies to <a undefined>ALTER TABLE ... ADD FOREIGN KEY</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... DROP FOREIGN KEY`

InnoDB supports dropping foreign key constraints from a table with <a undefined>ALGORITHM</a> set to `INPLACE`.

This operation only changes the table's metadata, so the table does not have to be rebuilt.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab2 (
   a int PRIMARY KEY,
   b varchar(50)
);

CREATE OR REPLACE TABLE tab1 (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50),
   d int,
   FOREIGN KEY tab2_fk (d) REFERENCES tab2 (a)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab1 DROP FOREIGN KEY tab2_fk;
Query OK, 0 rows affected (0.005 sec)
```

This applies to <a undefined>ALTER TABLE ... DROP FOREIGN KEY</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

## Table Operations

### `ALTER TABLE ... AUTO_INCREMENT=...`

InnoDB supports changing a table's [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment) value with <a undefined>ALGORITHM</a> set to `INPLACE`. This operation should finish instantly. The table is not rebuilt.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab AUTO_INCREMENT=100;
Query OK, 0 rows affected (0.004 sec)
```

This applies to <a undefined>ALTER TABLE ... AUTO_INCREMENT=...</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... ROW_FORMAT=...`

InnoDB supports changing a table's [row format](/kb/en/innodb-storage-formats/) with <a undefined>ALGORITHM</a> set to `INPLACE`.

The table is rebuilt, which means that all of the data is reorganized substantially, and the indexes are rebuilt. As a result, the operation is quite expensive.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) ROW_FORMAT=DYNAMIC;

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ROW_FORMAT=COMPRESSED;
Query OK, 0 rows affected (0.025 sec)
```

This applies to <a undefined>ALTER TABLE ... ROW_FORMAT=...</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... KEY_BLOCK_SIZE=...`

InnoDB supports changing a table's <a undefined>KEY_BLOCK_SIZE</a> with <a undefined>ALGORITHM</a> set to `INPLACE`.

The table is rebuilt, which means that all of the data is reorganized substantially, and the indexes are rebuilt. As a result, the operation is quite expensive.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) ROW_FORMAT=COMPRESSED
  KEY_BLOCK_SIZE=4;

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab KEY_BLOCK_SIZE=2;
Query OK, 0 rows affected (0.021 sec)
```

This applies to <a undefined>KEY_BLOCK_SIZE=...</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... PAGE_COMPRESSED=...` and `ALTER TABLE ... PAGE_COMPRESSION_LEVEL=...`

In [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/) and later, InnoDB supports setting a table's <a undefined>PAGE_COMPRESSED</a> value to `1` with <a undefined>ALGORITHM</a> set to `INPLACE`. InnoDB also supports changing a table's <a undefined>PAGE_COMPRESSED</a> value from `1` to `0` with <a undefined>ALGORITHM</a> set to `INPLACE`.

In these versions, InnoDB also supports changing a table's <a undefined>PAGE_COMPRESSION_LEVEL</a> value with <a undefined>ALGORITHM</a> set to `INPLACE`.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

See [MDEV-16328](https://jira.mariadb.org/browse/MDEV-16328) for more information.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab PAGE_COMPRESSED=1;
Query OK, 0 rows affected (0.006 sec)
```

And this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) PAGE_COMPRESSED=1;

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab PAGE_COMPRESSED=0;
Query OK, 0 rows affected (0.020 sec)
```

And this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) PAGE_COMPRESSED=1
  PAGE_COMPRESSION_LEVEL=5;

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab PAGE_COMPRESSION_LEVEL=4;
Query OK, 0 rows affected (0.006 sec)
```

This applies to <a undefined>PAGE_COMPRESSED=...</a> and <a undefined>PAGE_COMPRESSION_LEVEL=...</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... DROP SYSTEM VERSIONING`

InnoDB supports dropping [system versioning](/sql-statements-structure/temporal-tables/system-versioned-tables) from a table with <a undefined>ALGORITHM</a> set to `INPLACE`.

This operation supports the read-only locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `SHARED`. When this strategy is used, read-only concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) WITH SYSTEM VERSIONING;

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab DROP SYSTEM VERSIONING;
```

This applies to <a undefined>ALTER TABLE ... DROP SYSTEM VERSIONING</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... DROP CONSTRAINT`

In [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/) and later, InnoDB supports dropping a <a undefined>CHECK</a> constraint from a table with <a undefined>ALGORITHM</a> set to `INPLACE`. See [MDEV-16331](https://jira.mariadb.org/browse/MDEV-16331) for more information.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50),
   CONSTRAINT b_not_empty CHECK (b != '')
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab DROP CONSTRAINT b_not_empty;
Query OK, 0 rows affected (0.004 sec)
```

This applies to <a undefined>ALTER TABLE ... DROP CONSTRAINT</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... FORCE`

InnoDB supports forcing a table rebuild with <a undefined>ALGORITHM</a> set to `INPLACE`.

The table is rebuilt, which means that all of the data is reorganized substantially, and the indexes are rebuilt. As a result, the operation is quite expensive.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab FORCE;
Query OK, 0 rows affected (0.022 sec)
```

This applies to <a undefined>ALTER TABLE ... FORCE</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... ENGINE=InnoDB`

InnoDB supports forcing a table rebuild with <a undefined>ALGORITHM</a> set to `INPLACE`.

The table is rebuilt, which means that all of the data is reorganized substantially, and the indexes are rebuilt. As a result, the operation is quite expensive.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ENGINE=InnoDB;
Query OK, 0 rows affected (0.022 sec)
```

This applies to <a undefined>ALTER TABLE ... ENGINE=InnoDB</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `OPTIMIZE TABLE ...`

InnoDB supports optimizing a table with with <a undefined>ALGORITHM</a> set to `INPLACE`.

If the <a undefined>innodb_defragment</a> system variable is set to `OFF`, and if the <a undefined>innodb_optimize_fulltext_only</a> system variable is also set to `OFF`, then `OPTIMIZE TABLE` will be equivalent to `ALTER TABLE … FORCE`.

The table is rebuilt, which means that all of the data is reorganized substantially, and the indexes are rebuilt. As a result, the operation is quite expensive.

If either of the previously mentioned system variables is set to `ON`, then `OPTIMIZE TABLE` will optimize some data without rebuilding the table. However, the file size will not be reduced.

For example, this succeeds:

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

SET SESSION alter_algorithm='INPLACE';
OPTIMIZE TABLE tab;
+---------+----------+----------+-------------------------------------------------------------------+
| Table   | Op       | Msg_type | Msg_text                                                          |
+---------+----------+----------+-------------------------------------------------------------------+
| db1.tab | optimize | note     | Table does not support optimize, doing recreate + analyze instead |
| db1.tab | optimize | status   | OK                                                                |
+---------+----------+----------+-------------------------------------------------------------------+
2 rows in set (0.026 sec)
```

And this succeeds, but the table is not rebuilt:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET GLOBAL innodb_defragment=ON;
SHOW GLOBAL VARIABLES WHERE Variable_name IN('innodb_defragment', 'innodb_optimize_fulltext_only');
+-------------------------------+-------+
| Variable_name                 | Value |
+-------------------------------+-------+
| innodb_defragment             | ON    |
| innodb_optimize_fulltext_only | OFF   |
+-------------------------------+-------+

SET SESSION alter_algorithm='INPLACE';
OPTIMIZE TABLE tab;
+---------+----------+----------+----------+
| Table   | Op       | Msg_type | Msg_text |
+---------+----------+----------+----------+
| db1.tab | optimize | status   | OK       |
+---------+----------+----------+----------+
1 row in set (0.004 sec)
```

This applies to [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### `ALTER TABLE ... RENAME TO` and `RENAME TABLE ...`

InnoDB supports renaming a table with <a undefined>ALGORITHM</a> set to `INPLACE`.

This operation only changes the table's metadata, so the table does not have to be rebuilt.

This operation supports the exclusive locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `EXCLUSIVE`. When this strategy is used, concurrent DML is <strong>not</strong> permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab RENAME TO old_tab;
Query OK, 0 rows affected (0.011 sec)
```

And this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
RENAME TABLE tab TO old_tab;
```

This applies to <a undefined>ALTER TABLE ... RENAME TO</a> and [RENAME TABLE](/sql-statements-structure/sql-statements/data-definition/rename-table) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

## Limitations

### Limitations Related to Fulltext Indexes

- If a table has more than one [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes) index, then it cannot be rebuilt by any [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) operations when <a undefined>ALGORITHM</a> is set to `INPLACE`.

- If a table has a [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes) index, then it cannot be rebuilt by any [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) operations when the <a undefined>LOCK</a> clause is set to `NONE`.

### Limitations Related to Spatial Indexes

- If a table has a [SPATIAL](/sql-statements-structure/geographic-geometric-features/spatial-index) index, then it cannot be rebuilt by any [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) operations when the <a undefined>LOCK</a> clause is set to `NONE`.

### Limitations Related to Generated (Virtual and Persistent/Stored) Columns

[Generated columns](/sql-statements-structure/sql-statements/data-definition/create/generated-columns) do not currently support online DDL for all of the same operations that are supported for "real" columns.

See [Generated (Virtual and Persistent/Stored) Columns: Statement Support](/kb/en/generated-columns/#statement-support) for more information on the limitations.