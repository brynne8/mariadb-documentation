# InnoDB Online DDL Operations with the INSTANT Alter Algorithm

## Column Operations

### `ALTER TABLE ... ADD COLUMN`

In [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/) and later, InnoDB supports adding columns to a table with <a undefined>ALGORITHM</a> set to `INSTANT` if the new column is the last column in the table. See [MDEV-11369](https://jira.mariadb.org/browse/MDEV-11369) for more information. If the table has a hidden `FTS_DOC_ID` column is present, then this is not supported.

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, InnoDB supports adding columns to a table with <a undefined>ALGORITHM</a> set to `INSTANT`, regardless of where in the column list the new column is added.

When this operation is performed with <a undefined>ALGORITHM</a> set to `INSTANT`, the tablespace file will have a non-canonical storage format. See [Non-canonical Storage Format Caused by Some Operations](#non-canonical-storage-format-caused-by-some-operations) for more information.

With the exception of adding an [auto-increment](/columns-storage-engines-and-plugins/data-types/auto_increment/) column, this operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab ADD COLUMN c varchar(50);
Query OK, 0 rows affected (0.004 sec)
```

And this succeeds in [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab ADD COLUMN c varchar(50) AFTER a;
Query OK, 0 rows affected (0.004 sec)
```

This applies to <a undefined>ALTER TABLE ... ADD COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

See [Instant ADD COLUMN for InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/instant-add-column-for-innodb/) for more information.

### `ALTER TABLE ... DROP COLUMN`

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, InnoDB supports dropping columns from a table with <a undefined>ALGORITHM</a> set to `INSTANT`. See [MDEV-15562](https://jira.mariadb.org/browse/MDEV-15562) for more information.

When this operation is performed with <a undefined>ALGORITHM</a> set to `INSTANT`, the tablespace file will have a non-canonical storage format. See [Non-canonical Storage Format Caused by Some Operations](#non-canonical-storage-format-caused-by-some-operations) for more information.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab DROP COLUMN c;
Query OK, 0 rows affected (0.004 sec)
```

This applies to <a undefined>ALTER TABLE ... DROP COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... MODIFY COLUMN`

This applies to <a undefined>ALTER TABLE ... MODIFY COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

#### Reordering Columns

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, InnoDB supports reordering columns within a table with <a undefined>ALGORITHM</a> set to `INSTANT`. See [MDEV-15562](https://jira.mariadb.org/browse/MDEV-15562) for more information.

When this operation is performed with <a undefined>ALGORITHM</a> set to `INSTANT`, the tablespace file will have a non-canonical storage format. See [Non-canonical Storage Format Caused by Some Operations](#non-canonical-storage-format-caused-by-some-operations) for more information.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c varchar(50) AFTER a;
Query OK, 0 rows affected (0.004 sec)
```

#### Changing the Data Type of a Column

InnoDB does <strong>not</strong> support modifying a column's data type with <a undefined>ALGORITHM</a> set to `INSTANT` in most cases. There are some exceptions:

- InnoDB supports increasing the length of `VARCHAR` columns with <a undefined>ALGORITHM</a> set to `INSTANT`, unless it would require changing the number of bytes requires to represent the column's length. A `VARCHAR` column that is between 0 and 255 bytes in size requires 1 byte to represent its length, while a `VARCHAR` column that is 256 bytes or longer requires 2 bytes to represent its length. This means that the length of a column cannot be increased with <a undefined>ALGORITHM</a> set to `INSTANT` if the original length was less than 256 bytes, and the new length is 256 bytes or more.

- In [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later, InnoDB supports increasing the length of `VARCHAR` columns with <a undefined>ALGORITHM</a> set to `INSTANT` with no restrictions if the <a undefined>ROW_FORMAT</a> table option is set to <a undefined>REDUNDANT</a>. See [MDEV-15563](https://jira.mariadb.org/browse/MDEV-15563) for more information.

- In [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later, InnoDB also supports increasing the length of `VARCHAR` columns with <a undefined>ALGORITHM</a> set to `INSTANT` in a more limited manner if the <a undefined>ROW_FORMAT</a> table option is set to <a undefined>COMPACT</a>, <a undefined>DYNAMIC</a>, or <a undefined>COMPRESSED</a>. In this scenario, the following limitations apply:
<ul start="1"><li>The length can be increased with <a undefined>ALGORITHM</a> set to `INSTANT` if the original length of the column is 127 bytes or less, and the new length of the column is 256 bytes or more.
</li><li>The length can be increased with <a undefined>ALGORITHM</a> set to `INSTANT` if the original length of the column is 255 bytes or less, and the new length of the column is still 255 bytes or less.
</li><li>The length can be increased with <a undefined>ALGORITHM</a> set to `INSTANT` if the original length of the column is 256 bytes or more, and the new length of the column is still 256 bytes or more.
</li><li>The length can <strong>not</strong> be increased with <a undefined>ALGORITHM</a> set to `INSTANT` if the original length was between 128 bytes and 255 bytes, and the new length is 256 bytes or more.
</li><li>See [MDEV-15563](https://jira.mariadb.org/browse/MDEV-15563) for more information.
</li></ul>

The supported operations in this category support the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c int;
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: Cannot change column type INPLACE. Try ALGORITHM=COPY
```

But this succeeds because the original length of the column is less than 256 bytes, and the new length is still less than 256 bytes:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) CHARACTER SET=latin1;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c varchar(100);
Query OK, 0 rows affected (0.005 sec)
```

But this fails because the original length of the column is between 128 bytes and 255 bytes, and the new length is greater than 256 bytes:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(255)
) CHARACTER SET=latin1;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c varchar(256);
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: Cannot change column type INPLACE. Try ALGORITHM=COPY
```

But this succeeds in [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later because the table has `ROW_FORMAT=REDUNDANT`:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(200)
) ROW_FORMAT=REDUNDANT;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c varchar(300);
Query OK, 0 rows affected (0.004 sec)
```

And this succeeds in [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later because the table has `ROW_FORMAT=DYNAMIC` and the column's original length is 127 bytes or less:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(127)
) ROW_FORMAT=DYNAMIC
  CHARACTER SET=latin1;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c varchar(300);
Query OK, 0 rows affected (0.003 sec)
```

And this succeeds in [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later because the table has `ROW_FORMAT=COMPRESSED` and the column's original length is 127 bytes or less:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(127)
) ROW_FORMAT=COMPRESSED
  CHARACTER SET=latin1;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c varchar(300);
Query OK, 0 rows affected (0.003 sec)
```

But this fails even in [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later because the table has `ROW_FORMAT=DYNAMIC` and the column's original length is between 128 bytes and 255 bytes:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(128)
) ROW_FORMAT=DYNAMIC
  CHARACTER SET=latin1;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c varchar(300);
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: Cannot change column type INPLACE. Try ALGORITHM=COPY
```

#### Changing a Column to NULL

In [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later, InnoDB supports modifying a column to allow <a undefined>NULL</a> values with <a undefined>ALGORITHM</a> set to `INSTANT` if the <a undefined>ROW_FORMAT</a> table option is set to <a undefined>REDUNDANT</a>. See [MDEV-15563](https://jira.mariadb.org/browse/MDEV-15563) for more information.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50) NOT NULL
) ROW_FORMAT=REDUNDANT;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c varchar(50) NULL;
Query OK, 0 rows affected (0.004 sec)
```

#### Changing a Column to NOT NULL

InnoDB does <strong>not</strong> support modifying a column to <strong>not</strong> allow <a undefined>NULL</a> values with <a undefined>ALGORITHM</a> set to `INSTANT`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) ROW_FORMAT=REDUNDANT;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c varchar(50) NOT NULL;
ERROR 1845 (0A000): ALGORITHM=INSTANT is not supported for this operation. Try ALGORITHM=INPLACE
```

#### Adding a New `ENUM` Option

InnoDB supports adding a new [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum/) option to a column with <a undefined>ALGORITHM</a> set to `INSTANT`. In order to add a new [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum/) option with <a undefined>ALGORITHM</a> set to `INSTANT`, the following requirements must be met:

- It must be added to the end of the list.
- The storage requirements must not change.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c ENUM('red', 'green')
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c ENUM('red', 'green', 'blue');
Query OK, 0 rows affected (0.002 sec)
```

But this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c ENUM('red', 'green')
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c ENUM('red', 'blue', 'green');
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: Cannot change column type INPLACE. Try ALGORITHM=COPY
```

#### Adding a New `SET` Option

InnoDB supports adding a new [SET](/columns-storage-engines-and-plugins/data-types/string-data-types/set-data-type/) option to a column with <a undefined>ALGORITHM</a> set to `INSTANT`. In order to add a new [SET](/columns-storage-engines-and-plugins/data-types/string-data-types/set-data-type/) option with <a undefined>ALGORITHM</a> set to `INSTANT`, the following requirements must be met:

- It must be added to the end of the list.
- The storage requirements must not change.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c SET('red', 'green')
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c SET('red', 'green', 'blue');
Query OK, 0 rows affected (0.002 sec)
```

But this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c SET('red', 'green')
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c SET('red', 'blue', 'green');
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: Cannot change column type INPLACE. Try ALGORITHM=COPY
```

#### Removing System Versioning from a Column

In [MariaDB 10.3.8](/kb/en/mariadb-1038-release-notes/) and later, InnoDB supports removing [system versioning](/sql-statements-structure/temporal-tables/system-versioned-tables/) from a column with <a undefined>ALGORITHM</a> set to `INSTANT`. In order for this to work, the <a undefined>system_versioning_alter_history</a> system variable must be set to `KEEP`. See [MDEV-16330](https://jira.mariadb.org/browse/MDEV-16330) for more information.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50) WITH SYSTEM VERSIONING
);

SET SESSION system_versioning_alter_history='KEEP';
SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab MODIFY COLUMN c varchar(50) WITHOUT SYSTEM VERSIONING;
Query OK, 0 rows affected (0.004 sec)
```

### `ALTER TABLE ... ALTER COLUMN`

This applies to <a undefined>ALTER TABLE ... ALTER COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

#### Setting a Column's Default Value

InnoDB supports modifying a column's <a undefined>DEFAULT</a> value with <a undefined>ALGORITHM</a> set to `INSTANT`.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab ALTER COLUMN c SET DEFAULT 'No value explicitly provided.';
Query OK, 0 rows affected (0.003 sec)
```

#### Removing a Column's Default Value

InnoDB supports removing a column's  <a undefined>DEFAULT</a> value with <a undefined>ALGORITHM</a> set to `INSTANT`.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50) DEFAULT 'No value explicitly provided.'
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab ALTER COLUMN c DROP DEFAULT;
Query OK, 0 rows affected (0.002 sec)
```

### `ALTER TABLE ... CHANGE COLUMN`

InnoDB supports renaming a column with <a undefined>ALGORITHM</a> set to `INSTANT`, unless the column's data type or attributes changed in addition to the name.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab CHANGE COLUMN c str varchar(50);
Query OK, 0 rows affected (0.004 sec)
```

But this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab CHANGE COLUMN c num int;
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: Cannot change column type INPLACE. Try ALGORITHM=COPY
```

This applies to <a undefined>ALTER TABLE ... CHANGE COLUMN</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

## Index Operations

### `ALTER TABLE ... ADD PRIMARY KEY`

InnoDB does <strong>not</strong> support adding a primary key to a table with <a undefined>ALGORITHM</a> set to `INSTANT`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int,
   b varchar(50),
   c varchar(50)
);

SET SESSION sql_mode='STRICT_TRANS_TABLES';
SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab ADD PRIMARY KEY (a);
ERROR 1845 (0A000): ALGORITHM=INSTANT is not supported for this operation. Try ALGORITHM=INPLACE
```

This applies to <a undefined>ALTER TABLE ... ADD PRIMARY KEY</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... DROP PRIMARY KEY`

InnoDB does <strong>not</strong> support dropping a primary key with <a undefined>ALGORITHM</a> set to `INSTANT`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab DROP PRIMARY KEY;
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: Dropping a primary key is not allowed without also adding a new primary key. Try ALGORITHM=COPY
```

This applies to <a undefined>ALTER TABLE ... DROP PRIMARY KEY</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... ADD INDEX` and `CREATE INDEX`

This applies to <a undefined>ALTER TABLE ... ADD INDEX</a> and [CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index/) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

#### Adding a Plain Index

InnoDB does <strong>not</strong> support adding a plain index to a table with <a undefined>ALGORITHM</a> set to `INSTANT`.

For example, this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab ADD INDEX b_index (b);
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: ADD INDEX. Try ALGORITHM=NOCOPY
```

And this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
CREATE INDEX b_index ON tab (b);
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: ADD INDEX. Try ALGORITHM=NOCOPY
```

#### Adding a Fulltext Index

InnoDB does <strong>not</strong> support adding a [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) index to a table with <a undefined>ALGORITHM</a> set to `INSTANT`.

For example, this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD FULLTEXT INDEX b_index (b);
Query OK, 0 rows affected (0.042 sec)

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab ADD FULLTEXT INDEX c_index (c);
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: ADD INDEX. Try ALGORITHM=NOCOPY
```

And this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INPLACE';
CREATE FULLTEXT INDEX b_index ON tab (b);
Query OK, 0 rows affected (0.040 sec)

SET SESSION alter_algorithm='INSTANT';
CREATE FULLTEXT INDEX c_index ON tab (c);
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: ADD INDEX. Try ALGORITHM=NOCOPY
```

#### Adding a Spatial Index

InnoDB does <strong>not</strong> support adding a [SPATIAL](/sql-statements-structure/geographic-geometric-features/spatial-index/) index to a table with <a undefined>ALGORITHM</a> set to `INSTANT`.

For example, this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c GEOMETRY NOT NULL
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab ADD SPATIAL INDEX c_index (c);
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: ADD INDEX. Try ALGORITHM=NOCOPY
```

And this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c GEOMETRY NOT NULL
);

SET SESSION alter_algorithm='INSTANT';
CREATE SPATIAL INDEX c_index ON tab (c);
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: ADD INDEX. Try ALGORITHM=NOCOPY
```

### `ALTER TABLE ... DROP INDEX` and `DROP INDEX`

InnoDB supports dropping indexes from a table with <a undefined>ALGORITHM</a> set to `INSTANT`.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50),
   INDEX b_index (b)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab DROP INDEX b_index;
Query OK, 0 rows affected (0.008 sec)
```

And this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50),
   INDEX b_index (b)
);

SET SESSION alter_algorithm='INSTANT';
DROP INDEX b_index ON tab;
Query OK, 0 rows affected (0.007 sec)
```

This applies to <a undefined>ALTER TABLE ... DROP INDEX</a> and [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index/) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... ADD FOREIGN KEY`

InnoDB does <strong>not</strong> support adding foreign key constraints to a table with <a undefined>ALGORITHM</a> set to `INSTANT`.

For example:

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
SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab1 ADD FOREIGN KEY tab2_fk (d) REFERENCES tab2 (a);
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: ADD INDEX. Try ALGORITHM=NOCOPY
```

This applies to <a undefined>ALTER TABLE ... ADD FOREIGN KEY</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... DROP FOREIGN KEY`

InnoDB supports dropping foreign key constraints from a table with <a undefined>ALGORITHM</a> set to `INSTANT`.

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

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab1 DROP FOREIGN KEY tab2_fk; 
Query OK, 0 rows affected (0.004 sec)
```

This applies to <a undefined>ALTER TABLE ... DROP FOREIGN KEY</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

## Table Operations

### `ALTER TABLE ... AUTO_INCREMENT=...`

InnoDB supports changing a table's [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) value with <a undefined>ALGORITHM</a> set to `INSTANT`.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab AUTO_INCREMENT=100;
Query OK, 0 rows affected (0.002 sec)
```

This applies to <a undefined>ALTER TABLE ... AUTO_INCREMENT=...</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... ROW_FORMAT=...`

InnoDB does <strong>not</strong> support changing a table's [row format](/kb/en/innodb-storage-formats/) with <a undefined>ALGORITHM</a> set to `INSTANT`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) ROW_FORMAT=DYNAMIC;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab ROW_FORMAT=COMPRESSED;
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: Changing table options requires the table to be rebuilt. Try ALGORITHM=INPLACE
```

This applies to <a undefined>ALTER TABLE ... ROW_FORMAT=...</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... KEY_BLOCK_SIZE=...`

InnoDB does <strong>not</strong> support changing a table's <a undefined>KEY_BLOCK_SIZE</a> with <a undefined>ALGORITHM</a> set to `INSTANT`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) ROW_FORMAT=COMPRESSED
  KEY_BLOCK_SIZE=4;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab KEY_BLOCK_SIZE=2;
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: Changing table options requires the table to be rebuilt. Try ALGORITHM=INPLACE
```

This applies to <a undefined>KEY_BLOCK_SIZE=...</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... PAGE_COMPRESSED=1` and `ALTER TABLE ... PAGE_COMPRESSION_LEVEL=...`

In [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/) and later, InnoDB supports setting a table's <a undefined>PAGE_COMPRESSED</a> value to `1` with <a undefined>ALGORITHM</a> set to `INSTANT`. InnoDB does <strong>not</strong> support changing a table's <a undefined>PAGE_COMPRESSED</a> value from `1` to `0` with <a undefined>ALGORITHM</a> set to `INSTANT`.

In these versions, InnoDB also supports changing a table's <a undefined>PAGE_COMPRESSION_LEVEL</a> value with <a undefined>ALGORITHM</a> set to `INSTANT`.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

See [MDEV-16328](https://jira.mariadb.org/browse/MDEV-16328) for more information.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab PAGE_COMPRESSED=1;
Query OK, 0 rows affected (0.004 sec)
```

And this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) PAGE_COMPRESSED=1
  PAGE_COMPRESSION_LEVEL=5;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab PAGE_COMPRESSION_LEVEL=4;
Query OK, 0 rows affected (0.004 sec)
```

But this fails:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) PAGE_COMPRESSED=1;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab PAGE_COMPRESSED=0;
ERROR 1846 (0A000): ALGORITHM=INSTANT is not supported. Reason: Changing table options requires the table to be rebuilt. Try ALGORITHM=INPLACE
```

This applies to <a undefined>ALTER TABLE ... PAGE_COMPRESSED=...</a> and <a undefined>ALTER TABLE ... PAGE_COMPRESSION_LEVEL=...</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... DROP SYSTEM VERSIONING`

InnoDB does <strong>not</strong> support dropping [system versioning](/sql-statements-structure/temporal-tables/system-versioned-tables/) from a table with <a undefined>ALGORITHM</a> set to `INSTANT`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
) WITH SYSTEM VERSIONING;

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab DROP SYSTEM VERSIONING;
ERROR 1845 (0A000): ALGORITHM=INSTANT is not supported for this operation. Try ALGORITHM=INPLACE
```

This applies to <a undefined>ALTER TABLE ... DROP SYSTEM VERSIONING</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... DROP CONSTRAINT`

In [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/) and later, InnoDB supports dropping a <a undefined>CHECK</a> constraint from a table with <a undefined>ALGORITHM</a> set to `INSTANT`. See [MDEV-16331](https://jira.mariadb.org/browse/MDEV-16331) for more information.

This operation supports the non-locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `NONE`. When this strategy is used, all concurrent DML is permitted.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50),
   CONSTRAINT b_not_empty CHECK (b != '')
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab DROP CONSTRAINT b_not_empty;
Query OK, 0 rows affected (0.002 sec)
```

This applies to <a undefined>ALTER TABLE ... DROP CONSTRAINT</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... FORCE`

InnoDB does <strong>not</strong> support forcing a table rebuild with <a undefined>ALGORITHM</a> set to `INSTANT`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab FORCE;
ERROR 1845 (0A000): ALGORITHM=INSTANT is not supported for this operation. Try ALGORITHM=INPLACE
```

This applies to <a undefined>ALTER TABLE ... FORCE</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... ENGINE=InnoDB`

InnoDB does <strong>not</strong> support forcing a table rebuild with <a undefined>ALGORITHM</a> set to `INSTANT`.

For example:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab ENGINE=InnoDB;
ERROR 1845 (0A000): ALGORITHM=INSTANT is not supported for this operation. Try ALGORITHM=INPLACE
```

This applies to <a undefined>ALTER TABLE ... ENGINE=InnoDB</a> for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `OPTIMIZE TABLE ...`

InnoDB does <strong>not</strong> support optimizing a table with with <a undefined>ALGORITHM</a> set to `INSTANT`.

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

SET SESSION alter_algorithm='INSTANT';
OPTIMIZE TABLE tab;
+---------+----------+----------+------------------------------------------------------------------------------+
| Table   | Op       | Msg_type | Msg_text                                                                     |
+---------+----------+----------+------------------------------------------------------------------------------+
| db1.tab | optimize | note     | Table does not support optimize, doing recreate + analyze instead            |
| db1.tab | optimize | error    | ALGORITHM=INSTANT is not supported for this operation. Try ALGORITHM=INPLACE |
| db1.tab | optimize | status   | Operation failed                                                             |
+---------+----------+----------+------------------------------------------------------------------------------+
3 rows in set, 1 warning (0.002 sec)
```

This applies to [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

### `ALTER TABLE ... RENAME TO` and `RENAME TABLE ...`

InnoDB supports renaming a table with <a undefined>ALGORITHM</a> set to `INSTANT`.

This operation supports the exclusive locking strategy. This strategy can be explicitly chosen by setting the <a undefined>LOCK</a> clause to `EXCLUSIVE`. When this strategy is used, concurrent DML is <strong>not</strong> permitted.

For example, this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
ALTER TABLE tab RENAME TO old_tab;
Query OK, 0 rows affected (0.008 sec)
```

And this succeeds:

```sql
CREATE OR REPLACE TABLE tab (
   a int PRIMARY KEY,
   b varchar(50),
   c varchar(50)
);

SET SESSION alter_algorithm='INSTANT';
RENAME TABLE tab TO old_tab;
Query OK, 0 rows affected (0.008 sec)
```

This applies to <a undefined>ALTER TABLE ... RENAME TO</a> and [RENAME TABLE](/sql-statements-structure/sql-statements/data-definition/rename-table/) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables.

## Limitations

### Limitations Related to Generated (Virtual and Persistent/Stored) Columns

[Generated columns](/sql-statements-structure/sql-statements/data-definition/create/generated-columns/) do not currently support online DDL for all of the same operations that are supported for "real" columns.

See [Generated (Virtual and Persistent/Stored) Columns: Statement Support](/kb/en/generated-columns/#statement-support) for more information on the limitations.

### Non-canonical Storage Format Caused by Some Operations

Some operations cause a table's tablespace file to use a non-canonical storage format when the `INSTANT` algorithm is used. The affected operations include:

- [Adding a column.](#alter-table-add-column)
- [Dropping a column.](#alter-table-drop-column)
- [Reordering columns.](#reordering-columns)

These operations require the following non-canonical changes to the storage format:

- A hidden metadata record at the start of the clustered index is used to store each column's [DEFAULT](/built-in-functions/secondary-functions/information-functions/default/) value. This makes it possible to add new columns that have default values without rebuilding the table.
- A [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) in the hidden metadata record is used to store column mappings. This makes it possible to drop or reorder columns without rebuilding the table. This also makes it possible to add columns to any position or drop columns from any position in the table without rebuilding the table.
- If a column is dropped, old records will contain garbage in that column's former position, and new records will be written with [NULL](/columns-storage-engines-and-plugins/data-types/null-values/) values, empty strings, or dummy values.

This non-canonical storage format has the potential to incur some performance or storage overhead for all subsequent DML operations. If you notice some issues like this and you want to normalize a table's storage format to avoid this problem, then you can do so by forcing a table rebuild by executing <a undefined>ALTER TABLE ... FORCE</a> with <a undefined>ALGORITHM</a> set to `INPLACE`. For example:

```sql
SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab FORCE;
Query OK, 0 rows affected (0.008 sec)
```

However, keep in mind that there are certain scenarios where you may not be able to rebuild the table with <a undefined>ALGORITHM</a> set to `INPLACE`. See [InnoDB Online DDL Operations with ALGORITHM=INPLACE: Limitations](/kb/en/innodb-online-ddl-operations-with-algorithminplace/#limitations) for more information on those cases. If you hit one of those scenarios, but you still want to rebuild the table, then you would have to do so with <a undefined>ALGORITHM</a> set to `COPY`.

### Known Bugs

There are some known bugs that could lead to issues when an InnoDB DDL operation is performed using the <a undefined>INSTANT</a> algorithm. This algorithm will usually be chosen by default if the operation supports the algorithm.

The effect of many of these bugs is that the table seems to <em>forget</em> that its tablespace file is in the [non-canonical storage format](#non-canonical-storage-format-caused-by-some-operations).

If you are concerned that a table may be affected by one of these bugs, then your best option would be to normalize the table structure. This can be done by rebuilding the table. For example:

```sql
SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab FORCE;
Query OK, 0 rows affected (0.008 sec)
```

If you are concerned about these bugs, and you want to perform an operation that supports the <a undefined>INSTANT</a> algorithm, but you want to avoid using that algorithm, then you can set the algorithm to <a undefined>INPLACE</a> and add the `FORCE` keyword to the [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement:

```sql
SET SESSION alter_algorithm='INPLACE';
ALTER TABLE tab ADD COLUMN c varchar(50), FORCE;
```

#### Closed Bugs

- [MDEV-20066](https://jira.mariadb.org/browse/MDEV-20066): This bug could cause a table to become corrupt if a column was added instantly. It is fixed in [MariaDB 10.3.18](/kb/en/mariadb-10318-release-notes/) and [MariaDB 10.4.8](/kb/en/mariadb-1048-release-notes/).
- [MDEV-20117](https://jira.mariadb.org/browse/MDEV-20117): This bug could cause a table to become corrupt if a column was dropped instantly. It is fixed in [MariaDB 10.4.9](/kb/en/mariadb-1049-release-notes/).

#### Open Bugs

- [MDEV-18519](https://jira.mariadb.org/browse/MDEV-18519): This bug could cause a table to become corrupt if a column was added instantly.
- [MDEV-19743](https://jira.mariadb.org/browse/MDEV-19743): This bug could cause a table to become corrupt during page reorganization if a column was added instantly.
- [MDEV-19783](https://jira.mariadb.org/browse/MDEV-19783): This bug could cause a table to become corrupt if a column was added instantly.
- [MDEV-20090](https://jira.mariadb.org/browse/MDEV-20090): This bug could cause a table to become corrupt if columns were added, dropped, or reordered instantly.