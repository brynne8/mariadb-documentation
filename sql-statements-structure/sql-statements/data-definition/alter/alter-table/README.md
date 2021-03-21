# ALTER TABLE

## Syntax

```sql
ALTER [ONLINE] [IGNORE] TABLE [IF EXISTS] tbl_name
    [WAIT n | NOWAIT]
    alter_specification [, alter_specification] ...
alter_specification:
    table_option ...
  | ADD [COLUMN] [IF NOT EXISTS] col_name column_definition
        [FIRST | AFTER col_name ]
  | ADD [COLUMN] [IF NOT EXISTS] (col_name column_definition,...)
  | ADD {INDEX|KEY} [IF NOT EXISTS] [index_name]
        [index_type] (index_col_name,...) [index_option] ...
  | ADD [CONSTRAINT [symbol]] PRIMARY KEY
        [index_type] (index_col_name,...) [index_option] ...
  | ADD [CONSTRAINT [symbol]]
        UNIQUE [INDEX|KEY] [index_name]
        [index_type] (index_col_name,...) [index_option] ...
  | ADD FULLTEXT [INDEX|KEY] [index_name]
        (index_col_name,...) [index_option] ...
  | ADD SPATIAL [INDEX|KEY] [index_name]
        (index_col_name,...) [index_option] ...
  | ADD [CONSTRAINT [symbol]]
        FOREIGN KEY [IF NOT EXISTS] [index_name] (index_col_name,...)
        reference_definition
  | ADD PERIOD FOR SYSTEM_TIME (start_column_name, end_column_name)
  | ALTER [COLUMN] col_name SET DEFAULT literal | (expression)
  | ALTER [COLUMN] col_name DROP DEFAULT
  | CHANGE [COLUMN] [IF EXISTS] old_col_name new_col_name column_definition
        [FIRST|AFTER col_name]
  | MODIFY [COLUMN] [IF EXISTS] col_name column_definition
        [FIRST | AFTER col_name]
  | DROP [COLUMN] [IF EXISTS] col_name [RESTRICT|CASCADE]
  | DROP PRIMARY KEY
  | DROP {INDEX|KEY} [IF EXISTS] index_name
  | DROP FOREIGN KEY [IF EXISTS] fk_symbol
  | DROP CONSTRAINT [IF EXISTS] constraint_name
  | DISABLE KEYS
  | ENABLE KEYS
  | RENAME [TO] new_tbl_name
  | ORDER BY col_name [, col_name] ...
  | RENAME COLUMN old_col_name TO new_col_name
  | RENAME {INDEX|KEY} old_index_name TO new_index_name
  | CONVERT TO CHARACTER SET charset_name [COLLATE collation_name]
  | [DEFAULT] CHARACTER SET [=] charset_name
  | [DEFAULT] COLLATE [=] collation_name
  | DISCARD TABLESPACE
  | IMPORT TABLESPACE
  | ALGORITHM [=] {DEFAULT|INPLACE|COPY|NOCOPY|INSTANT}
  | LOCK [=] {DEFAULT|NONE|SHARED|EXCLUSIVE}
  | FORCE
  | partition_options
  | ADD PARTITION (partition_definition)
  | DROP PARTITION partition_names
  | COALESCE PARTITION number
  | REORGANIZE PARTITION [partition_names INTO (partition_definitions)]
  | ANALYZE PARTITION partition_names
  | CHECK PARTITION partition_names
  | OPTIMIZE PARTITION partition_names
  | REBUILD PARTITION partition_names
  | REPAIR PARTITION partition_names
  | EXCHANGE PARTITION partition_name WITH TABLE tbl_name
  | REMOVE PARTITIONING
  | ADD SYSTEM VERSIONING
  | DROP SYSTEM VERSIONING
index_col_name:
    col_name [(length)] [ASC | DESC]
index_type:
    USING {BTREE | HASH | RTREE}
index_option:
    KEY_BLOCK_SIZE [=] value
  | index_type
  | WITH PARSER parser_name
  | COMMENT 'string'
  | CLUSTERING={YES| NO}
table_options:
    table_option [[,] table_option] ...```

## Description

`ALTER TABLE` enables you to change the structure of an existing table.
For example, you can add or delete columns, create or destroy indexes,
change the type of existing columns, or rename columns or the table
itself. You can also change the comment for the table and the storage engine of the
table.

If another connection is using the table, a [metadata lock](/sql-statements-structure/sql-statements/transactions/metadata-locking/) is active, and this statement will wait until the lock is released. This is also true for non-transactional tables.

When adding a `UNIQUE` index on a column (or a set of columns) which have duplicated values, an error will be produced and the statement will be stopped. To suppress the error and force the creation of `UNIQUE` indexes, discarding duplicates, the [IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/ignore/) option can be specified. This can be useful if a column (or a set of columns) should be UNIQUE but it contains duplicate values; however, this technique provides no control on which rows are preserved and which are deleted. Also, note that `IGNORE` is accepted but ignored in `ALTER TABLE ... EXCHANGE PARTITION` statements.

This statement can also be used to rename a table. For details see [RENAME TABLE](/sql-statements-structure/sql-statements/data-definition/rename-table/).

When an index is created, the storage engine may use a configurable buffer in the process. Incrementing the buffer speeds up the index creation. [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) and [MyISAM](/kb/en/myisam/) allocate a buffer whose size is defined by <a undefined>aria_sort_buffer_size</a> or [myisam_sort_buffer_size](/kb/en/myisam-server-system-variables/#myisam_sort_buffer_size), also used for [REPAIR TABLE](/sql-statements-structure/sql-statements/table-statements/repair-table/). [InnoDB/XtraDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) allocates three buffers whose size is defined by [innodb_sort_buffer_size](/kb/en/innodb-system-variables/#innodb_sort_buffer_size).

## Privileges

Executing the `ALTER TABLE` statement generally requires at least the [ALTER](/kb/en/grant/#table-privileges) privilege for the table or the database..

If you are renaming a table, then it also requires the [DROP](/kb/en/grant/#table-privileges), [CREATE](/kb/en/grant/#table-privileges) and [INSERT](/kb/en/grant/#table-privileges) privileges for the table or the database as well.

## Online DDL

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later, online DDL is supported with the [ALGORITHM](#algorithm) and [LOCK](#lock) clauses.

See [InnoDB Online DDL Overview](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/innodb-online-ddl-overview/) for more information on online DDL with [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/).

### `ALTER ONLINE TABLE`

##### MariaDB starting with [10.0.11](/kb/en/mariadb-10011-release-notes/)

ALTER ONLINE TABLE has also worked for partitioned tables since [MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/).

Online `ALTER TABLE` is available by executing the following:

```sql
ALTER ONLINE TABLE ...;
```

This statement has the following semantics:

##### MariaDB starting with [10.0.12](/kb/en/mariadb-10012-release-notes/)

In [MariaDB 10.0.12](/kb/en/mariadb-10012-release-notes/) and later, this statement is equivalent to the following:

```sql
ALTER TABLE ... LOCK=NONE;
```

See the [LOCK](#lock) alter specification for more information.

##### MariaDB starting with [10.0.11](/kb/en/mariadb-10011-release-notes/)

In [MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/), this statement is equivalent to the following:

```sql
ALTER TABLE ... ALGORITHM=INPLACE;
```

See the <a undefined>ALGORITHM</a> alter specification for more information.

##### MariaDB until [10.0.10](/kb/en/mariadb-10010-release-notes/)

In [MariaDB 10.0.10](/kb/en/mariadb-10010-release-notes/) and before, this statement ensures that the `ALTER TABLE` statement does not make a copy of the table.

## WAIT/NOWAIT

##### MariaDB starting with [10.3.0](/kb/en/mariadb-1030-release-notes/)

Set the lock wait timeout. See [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait/).

## IF EXISTS

##### MariaDB starting with [10.0.2](/kb/en/mariadb-1002-release-notes/)

In [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/) and later, `IF EXISTS` and `IF NOT EXISTS` clauses were added for the following:

```sql
ADD COLUMN       [IF NOT EXISTS]
ADD INDEX        [IF NOT EXISTS]
ADD FOREIGN KEY  [IF NOT EXISTS]
ADD PARTITION    [IF NOT EXISTS]
CREATE INDEX     [IF NOT EXISTS]
DROP COLUMN      [IF EXISTS]
DROP INDEX       [IF EXISTS]
DROP FOREIGN KEY [IF EXISTS]
DROP PARTITION   [IF EXISTS]
CHANGE COLUMN    [IF EXISTS]
MODIFY COLUMN    [IF EXISTS]
DROP INDEX       [IF EXISTS]```

When `IF EXISTS` and `IF NOT EXISTS` are used in clauses, queries will not
report errors when the condition is triggered for that clause. A warning with
the same message text will be issued and the ALTER will move on to the next
clause in the statement (or end if finished).

This was done in [MDEV-318](https://jira.mariadb.org/browse/MDEV-318).

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

If this is directive is used after `ALTER ... TABLE`, one will not get an error if the table doesn't exist.

## Column Definitions

See [CREATE TABLE: Column Definitions](/kb/en/create-table/#column-definitions) for information about column definitions.

## Index Definitions

See [CREATE TABLE: Index Definitions](/kb/en/create-table/#index-definitions) for information about index definitions.

The [CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index/) and [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index/) statements can also be used to add or remove an index.

## Character Sets and Collations

```sql
CONVERT TO CHARACTER SET charset_name [COLLATE collation_name]
[DEFAULT] CHARACTER SET [=] charset_name
[DEFAULT] COLLATE [=] collation_name```

See [Setting Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/setting-character-sets-and-collations/) for details on setting the [character sets and collations](/kb/en/data-types-character-sets-and-collations/).

## Alter Specifications

### Table Options

See [CREATE TABLE: Table Options](/kb/en/create-table/#table-options) for information about table options.

### ADD COLUMN

```sql
... ADD COLUMN [IF NOT EXISTS]  (col_name column_definition,...)```

Adds a column to the table. The syntax is the same as in [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/).
If you are using <code class="highlight fixed" style="white-space:pre-wrap">IF NOT_EXISTS</code> the column will not be added if it was not there already. This is very useful when doing scripts to modify tables.

The `FIRST` and `AFTER` clauses affect the physical order of columns in the datafile. Use `FIRST` to add a column in the first (leftmost) position, or `AFTER` followed by a column name to add the new column in any other position. Note that, nowadays, the physical position of a column is usually irrelevant.

See also [Instant ADD COLUMN for InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/instant-add-column-for-innodb/).

### DROP COLUMN

```sql
... DROP COLUMN [IF EXISTS] col_name [CASCADE|RESTRICT]```

Drops the column from the table.
If you are using <code class="highlight fixed" style="white-space:pre-wrap">IF EXISTS</code> you will not get an error if the column didn't exist.
If the column is part of any index, the column will be dropped from them, except if you add a new column with identical name at the same time. The index will be dropped if all columns from the index were dropped.
If the column was used in a view or trigger, you will get an error next time the view or trigger is accessed.

##### MariaDB starting with [10.2.8](/kb/en/mariadb-1028-release-notes/)

Dropping a column that is part of a multi-column <code class="highlight fixed" style="white-space:pre-wrap">UNIQUE</code> constraint is not permitted. For example:

```sql
CREATE TABLE a (
  a int,
  b int,
  primary key (a,b)
);

ALTER TABLE x DROP COLUMN a;
[42000][1072] Key column 'A' doesn't exist in table
```

The reason is that dropping column `a` would result in the new constraint that all values in column `b` be unique. In order to drop the column, an explicit `DROP PRIMARY KEY` and `ADD PRIMARY KEY` would be required. Up until [MariaDB 10.2.7](/kb/en/mariadb-1027-release-notes/), the column was dropped and the additional constraint applied, resulting in the following structure:

```sql
ALTER TABLE x DROP COLUMN a;
Query OK, 0 rows affected (0.46 sec)

DESC x;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| b     | int(11) | NO   | PRI | NULL    |       |
+-------+---------+------+-----+---------+-------+
```

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

[MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/) supports instant DROP COLUMN. DROP COLUMN of an indexed column would imply [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index/) (and in the case of a non-UNIQUE multi-column index, possibly ADD INDEX). These will not be allowed with [ALGORITHM=INSTANT](#algorithm), but unlike before, they can be allowed with [ALGORITHM=NOCOPY](#algorithm)

<code class="highlight fixed" style="white-space:pre-wrap">RESTRICT</code> and <code class="highlight fixed" style="white-space:pre-wrap">CASCADE</code> are allowed to make porting from other database systems easier. In MariaDB, they do nothing.

### MODIFY COLUMN

Allows you to modify the type of a column. The column will be at the same place as the original column and all indexes on the column will be kept.  Note that when modifying column, you should specify all attributes for the new column.

```sql
CREATE TABLE t1 (a INT UNSIGNED AUTO_INCREMENT, PRIMARY KEY((a));
ALTER TABLE t1 MODIFY a BIGINT UNSIGNED AUTO_INCREMENT;
```

### CHANGE COLUMN

Works like `MODIFY COLUMN` except that you can also change the name of the column. The column will be at the same place as the original column and all index on the column will be kept.

```sql
CREATE TABLE t1 (a INT UNSIGNED AUTO_INCREMENT, PRIMARY KEY(a));
ALTER TABLE t1 CHANGE a b BIGINT UNSIGNED AUTO_INCREMENT;
```

### ALTER COLUMN

This lets you change column options.

```sql
CREATE TABLE t1 (a INT UNSIGNED AUTO_INCREMENT, b varchar(50), PRIMARY KEY(a));
ALTER TABLE t1 ALTER b SET DEFAULT 'hello';
```

### RENAME INDEX/KEY

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), it is possible to rename an index using the `RENAME INDEX` (or `RENAME KEY`) syntax, for example:

```sql
ALTER TABLE t1 RENAME INDEX i_old TO i_new;
```

### RENAME COLUMN

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), it is possible to rename a column using the `RENAME COLUMN` syntax, for example:

```sql
ALTER TABLE t1 RENAME COLUMN c_old TO c_new;
```

### ADD PRIMARY KEY

Add a primary key.

For `PRIMARY KEY` indexes, you can specify a name for the index, but it is silently ignored, and the name of the index is always `PRIMARY`.

See [Getting Started with Indexes: Primary Key](/kb/en/getting-started-with-indexes/#primary-key) for more information.

### DROP PRIMARY KEY

Drop a primary key.

For `PRIMARY KEY` indexes, you can specify a name for the index, but it is silently ignored, and the name of the index is always `PRIMARY`.

See [Getting Started with Indexes: Primary Key](/kb/en/getting-started-with-indexes/#primary-key) for more information.

### ADD FOREIGN KEY

Add a foreign key.

For `FOREIGN KEY` indexes, a reference definition must be provided.

For `FOREIGN KEY` indexes, you can specify a name for the constraint, using the `CONSTRAINT` keyword. That name will be used in error messages.

First, you have to specify the name of the target (parent) table and a column or a column list which must be indexed and whose values must match to the foreign key's values. The `MATCH` clause is accepted to improve the compatibility with other DBMS's, but has no meaning in MariaDB. The `ON DELETE` and `ON UPDATE` clauses specify what must be done when a `DELETE` (or a `REPLACE`) statements attempts to delete a referenced row from the parent table, and when an `UPDATE` statement attempts to modify the referenced foreign key columns in a parent table row, respectively. The following options are allowed:

- `RESTRICT`: The delete/update operation is not performed.  The statement terminates with a 1451 error (SQLSTATE '2300').
- `NO ACTION`: Synonym for `RESTRICT`.
- `CASCADE`: The delete/update operation is performed in both tables.
- `SET NULL`: The update or delete goes ahead in the parent table, and the corresponding foreign key fields in the child table are set to `NULL`.  (They must not be defined as `NOT NULL` for this to succeed).
- `SET DEFAULT`: This option is implemented only for the legacy PBXT storage engine, which is disabled by default and no longer maintained. It sets the child table's foreign key fields to their `DEFAULT` values when the referenced parent table key entries are updated or deleted.

If either clause is omitted, the default behavior for the omitted clause is `RESTRICT`.

See [Foreign Keys](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys/) for more information.

### DROP FOREIGN KEY

Drop a foreign key.

See [Foreign Keys](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys/) for more information.

### ADD INDEX

Add a plain index.

Plain indexes are regular indexes that are not unique, and are not acting as a primary key or a foreign key. They are also not the "specialized" `FULLTEXT` or `SPATIAL` indexes.

See [Getting Started with Indexes: Plain Indexes](/kb/en/getting-started-with-indexes/#plain-indexes) for more information.

### DROP INDEX

Drop a plain index.

Plain indexes are regular indexes that are not unique, and are not acting as a primary key or a foreign key. They are also not the "specialized" `FULLTEXT` or `SPATIAL` indexes.

See [Getting Started with Indexes: Plain Indexes](/kb/en/getting-started-with-indexes/#plain-indexes) for more information.

### ADD UNIQUE INDEX

Add a unique index.

The `UNIQUE` keyword means that the index will not accept duplicated values, except for NULLs. An error will raise if you try to insert duplicate values in a UNIQUE index.

For `UNIQUE` indexes, you can specify a name for the constraint, using the `CONSTRAINT` keyword. That name will be used in error messages.

See [Getting Started with Indexes: Unique Index](/kb/en/getting-started-with-indexes/#unique-index) for more information.

### DROP UNIQUE INDEX

Drop a unique index.

The `UNIQUE` keyword means that the index will not accept duplicated values, except for NULLs. An error will raise if you try to insert duplicate values in a UNIQUE index.

For `UNIQUE` indexes, you can specify a name for the constraint, using the `CONSTRAINT` keyword. That name will be used in error messages.

See [Getting Started with Indexes: Unique Index](/kb/en/getting-started-with-indexes/#unique-index) for more information.

### ADD FULLTEXT INDEX

Add a `FULLTEXT` index.

See [Full-Text Indexes](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) for more information.

### DROP FULLTEXT INDEX

Drop a `FULLTEXT` index.

See [Full-Text Indexes](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) for more information.

### ADD SPATIAL INDEX

Add a SPATIAL index.

See [SPATIAL INDEX](/sql-statements-structure/geographic-geometric-features/spatial-index/) for more information.

### DROP SPATIAL INDEX

Drop a SPATIAL index.

See [SPATIAL INDEX](/sql-statements-structure/geographic-geometric-features/spatial-index/) for more information.

### ENABLE/ DISABLE KEYS

`DISABLE KEYS` will disable all non unique keys for the table for storage engines that support this (at least MyISAM and Aria). This can be used to [speed up inserts](/replication/optimization-and-tuning/query-optimizations/how-to-quickly-insert-data-into-mariadb/) into empty tables.

`ENABLE KEYS` will enable all disabled keys.

### RENAME TO

Renames the table. See also [RENAME TABLE](/sql-statements-structure/sql-statements/data-definition/rename-table/).

### ADD CONSTRAINT

Modifies the table adding a [constraint](/sql-statements-structure/sql-statements/data-definition/constraint/) on a particular column or columns.

##### MariaDB starting with [10.2.1](/kb/en/mariadb-1021-release-notes/)

[MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) introduced new ways to define a constraint.

Note: Before [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/), constraint expressions were accepted in syntax, but ignored.

```sql
ALTER TABLE table_name 
ADD CONSTRAINT [constraint_name] CHECK(expression);```

Before a row is inserted or updated, all constraints are evaluated in the order they are defined.  If any constraint fails, then the row will not be updated.  One can use most deterministic functions in a constraint, including [UDF's](/programming-customizing-mariadb/user-defined-functions/).

```sql
CREATE TABLE account_ledger (
	id INT PRIMARY KEY AUTO_INCREMENT,
	transaction_name VARCHAR(100),
	credit_account VARCHAR(100),
	credit_amount INT,
	debit_account VARCHAR(100),
	debit_amount INT);

ALTER TABLE account_ledger 
ADD CONSTRAINT is_balanced 
    CHECK((debit_amount + credit_amount) = 0);
```

The `constraint_name` is optional.  If you don't provide one in the `ALTER TABLE` statement, MariaDB auto-generates a name for you.  This is done so that you can remove it later using <a undefined>DROP CONSTRAINT</a> clause.

You can disable all constraint expression checks by setting the variable <a undefined>check_constraint_checks</a> to `OFF`.  You may find this useful when loading a table that violates some constraints that you want to later find and fix in SQL.

To view constraints on a table, query <a undefined>information_schema.TABLE_CONSTRAINTS</a>:

```sql
SELECT CONSTRAINT_NAME, TABLE_NAME, CONSTRAINT_TYPE 
FROM information_schema.TABLE_CONSTRAINTS
WHERE TABLE_NAME = 'account_ledger';

+-----------------+----------------+-----------------+
| CONSTRAINT_NAME | TABLE_NAME     | CONSTRAINT_TYPE |
+-----------------+----------------+-----------------+
| is_balanced     | account_ledger | CHECK           |
+-----------------+----------------+-----------------+
```

### DROP CONSTRAINT

##### MariaDB starting with [10.2.22](/kb/en/mariadb-10222-release-notes/)

`DROP CONSTRAINT` for `UNIQUE` and `FOREIGN KEY` [constraints](/sql-statements-structure/sql-statements/data-definition/constraint/) was introduced in [MariaDB 10.2.22](/kb/en/mariadb-10222-release-notes/) and [MariaDB 10.3.13](/kb/en/mariadb-10313-release-notes/).

##### MariaDB starting with [10.2.1](/kb/en/mariadb-1021-release-notes/)

`DROP CONSTRAINT` for `CHECK` constraints was introduced in [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/)

Modifies the table, removing the given constraint.

```sql
ALTER TABLE table_name
DROP CONSTRAINT constraint_name;
```

When you add a constraint to a table, whether through a <a undefined>CREATE TABLE</a> or <a undefined>ALTER TABLE...ADD CONSTRAINT</a> statement, you can either set a `constraint_name` yourself, or allow MariaDB to auto-generate one for you.  To view constraints on a table, query <a undefined>information_schema.TABLE_CONSTRAINTS</a>.  For instance,

```sql
CREATE TABLE t (
   a INT,
   b INT,
   c INT,
   CONSTRAINT CHECK(a > b),
   CONSTRAINT check_equals CHECK(a = c)); 

SELECT CONSTRAINT_NAME, TABLE_NAME, CONSTRAINT_TYPE 
FROM information_schema.TABLE_CONSTRAINTS
WHERE TABLE_NAME = 't';

+-----------------+----------------+-----------------+
| CONSTRAINT_NAME | TABLE_NAME     | CONSTRAINT_TYPE |
+-----------------+----------------+-----------------+
| check_equals    | t              | CHECK           |
| CONSTRAINT_1    | t              | CHECK           |
+-----------------+----------------+-----------------+
```

To remove a constraint from the table, issue an `ALTER TABLE...DROP CONSTRAINT` statement.  For example,

```sql
ALTER TABLE t DROP CONSTRAINT is_unique;
```

### ADD SYSTEM VERSIONING

##### MariaDB starting with [10.3.4](/kb/en/mariadb-1034-release-notes/)

[System-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables/) was added in [MariaDB 10.3.4](/kb/en/mariadb-1034-release-notes/).

Add system versioning.

### DROP SYSTEM VERSIONING

##### MariaDB starting with [10.3.4](/kb/en/mariadb-1034-release-notes/)

[System-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables/) was added in [MariaDB 10.3.4](/kb/en/mariadb-1034-release-notes/).

Drop system versioning.

### ADD PERIOD FOR SYSTEM_TIME

##### MariaDB starting with [10.3.4](/kb/en/mariadb-1034-release-notes/)

[System-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables/) was added in [MariaDB 10.3.4](/kb/en/mariadb-1034-release-notes/).

### FORCE

`ALTER TABLE ... FORCE` can force MariaDB to re-build the table.

In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and before, this could only be done by setting the <a undefined>ENGINE</a> table option to its old value. For example, for an InnoDB table, one could execute the following:

```sql
ALTER TABLE tab_name ENGINE = InnoDB;
```

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later, the `FORCE` option can be used instead. For example, :

```sql
ALTER TABLE tab_name FORCE;
```

With InnoDB, the table rebuild will only reclaim unused space (i.e. the space previously used for deleted rows) if the <a undefined>innodb_file_per_table</a> system variable is set to `ON`. If the system variable is `OFF`, then the space will not be reclaimed, but it will be-re-used for new data that's later added.

### EXCHANGE PARTITION

##### MariaDB starting with [10.0.4](/kb/en/mariadb-1004-release-notes/)

`ALTER TABLE  ... EXCHANGE PARTITION` was introduced in [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

This is used to exchange the tablespace files between a partition and another table.

See [copying InnoDB's transportable tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces) for more information.

### DISCARD TABLESPACE

This is used to discard an InnoDB table's tablespace.

See [copying InnoDB's transportable tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces) for more information.

### IMPORT TABLESPACE

This is used to import an InnoDB table's tablespace. The tablespace should have been copied from its original server after executing [FLUSH TABLES FOR EXPORT](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush-tables-for-export/).

See [copying InnoDB's transportable tablespaces](/kb/en/innodb-file-per-table-tablespaces/#copying-transportable-tablespaces) for more information.

`ALTER TABLE ... IMPORT` only applies to InnoDB tables. Most other popular storage engines, such as Aria and MyISAM, will recognize their data files as soon as they've been placed in the proper directory under the datadir, and no special DDL is required to import them.

### `ALGORITHM`

In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and before, `ALTER TABLE` operations required making a temporary copy of the table, which can be slow for large tables.

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later, the `ALTER TABLE` statement supports the `ALGORITHM` clause. This clause is one of the clauses that is used to implement online DDL. `ALTER TABLE` supports several different algorithms. An algorithm can be explicitly chosen for an `ALTER TABLE` operation by setting the `ALGORITHM` clause. The supported values are:

- `ALGORITHM=DEFAULT` - This implies the default behavior for the specific statement, such as if no `ALGORITHM` clause is specified.
- `ALGORITHM=COPY`
- `ALGORITHM=INPLACE`
- `ALGORITHM=NOCOPY` - This was added in [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/).
- `ALGORITHM=INSTANT` - This was added in [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/).

See [InnoDB Online DDL Overview: ALGORITHM](/kb/en/innodb-online-ddl-overview/#algorithm) for information on how the `ALGORITHM` clause affects InnoDB.

#### `ALGORITHM=DEFAULT`

The default behavior, which occurs if `ALGORITHM=DEFAULT` is specified, or if `ALGORITHM` is not specified at all, usually only makes a copy if the operation doesn't support being done in-place at all. In this case, the most efficient available algorithm will usually be used.

However, in [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/) and before, if the value of the <a undefined>old_alter_table</a> system variable is set to `ON`, then the default behavior is to perform `ALTER TABLE` operations by making a copy of the table using the old algorithm.

In [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/) and later, the <a undefined>old_alter_table</a> system variable is deprecated. Instead, the <a undefined>alter_algorithm</a> system variable defines the default algorithm for `ALTER TABLE` operations.

#### `ALGORITHM=COPY`

`ALGORITHM=COPY` was introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/) as the name for the original [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) algorithm.

When `ALGORITHM=COPY` is set, MariaDB essentially does the following operations:

```sql
-- Create a temporary table with the new definition
CREATE TEMPORARY TABLE tmp_tab (
...
);

-- Copy the data from the original table
INSERT INTO tmp_tab
   SELECT * FROM original_tab;

-- Drop the original table
DROP TABLE original_tab;

-- Rename the temporary table, so that it replaces the original one
RENAME TABLE tmp_tab TO original_tab;
```

This algorithm is very inefficient, but it is generic, so it works for all storage engines.

If `ALGORITHM=COPY` is specified, then the copy algorithm will be used even if it is not necessary. This can result in a lengthy table copy. If multiple [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) operations are required that each require the table to be rebuilt, then it is best to specify all operations in a single [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement, so that the table is only rebuilt once.

#### `ALGORITHM=INPLACE`

`ALGORITHM=INPLACE` was introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

`ALGORITHM=COPY` can be incredibly slow, because the whole table has to be copied and rebuilt. `ALGORITHM=INPLACE` was introduced as a way to avoid this by performing operations in-place and avoiding the table copy and rebuild, when possible.

When `ALGORITHM=INPLACE` is set, the underlying storage engine uses optimizations to perform the operation while avoiding the table copy and rebuild. However, `INPLACE` is a bit of a misnomer, since some operations may still require the table to be rebuilt for some storage engines. Regardless, several operations can be performed without a full copy of the table for some storage engines.

A more accurate name would have been `ALGORITHM=ENGINE`, where `ENGINE` refers to an "engine-specific" algorithm.

If an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) operation supports `ALGORITHM=INPLACE`, then it can be performed using optimizations by the underlying storage engine, but it may rebuilt.

See [InnoDB Online DDL Operations with ALGORITHM=INPLACE](/kb/en/innodb-online-ddl-operations-with-algorithminplace/) for more.

#### `ALGORITHM=NOCOPY`

`ALGORITHM=NOCOPY` was introduced in [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/).

`ALGORITHM=INPLACE` can sometimes be surprisingly slow in instances where it has to rebuild the clustered index, because when the clustered index has to be rebuilt, the whole table has to be rebuilt. `ALGORITHM=NOCOPY` was introduced as a way to avoid this.

If an `ALTER TABLE` operation supports `ALGORITHM=NOCOPY`, then it can be performed without rebuilding the clustered index.

If `ALGORITHM=NOCOPY` is specified for an `ALTER TABLE` operation that does not support `ALGORITHM=NOCOPY`, then an error will be raised. In this case, raising an error is preferable, if the alternative is for the operation to rebuild the clustered index, and perform unexpectedly slowly.

See [InnoDB Online DDL Operations with ALGORITHM=NOCOPY](/kb/en/innodb-online-ddl-operations-with-algorithmnocopy/) for more.

#### `ALGORITHM=INSTANT`

`ALGORITHM=INSTANT` was introduced in [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/).

`ALGORITHM=INPLACE` can sometimes be surprisingly slow in instances where it has to modify data files. `ALGORITHM=INSTANT` was introduced as a way to avoid this.

If an `ALTER TABLE` operation supports `ALGORITHM=INSTANT`, then it can be performed without modifying any data files.

If `ALGORITHM=INSTANT` is specified for an `ALTER TABLE` operation that does not support `ALGORITHM=INSTANT`, then an error will be raised. In this case, raising an error is preferable, if the alternative is for the operation to modify data files, and perform unexpectedly slowly.

See [InnoDB Online DDL Operations with ALGORITHM=INSTANT](/kb/en/innodb-online-ddl-operations-with-algorithminstant/) for more.

### LOCK

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later, the `ALTER TABLE` statement supports the `LOCK` clause. This clause is one of the clauses that is used to implement online DDL. `ALTER TABLE` supports several different locking strategies. A locking strategy can be explicitly chosen for an `ALTER TABLE` operation by setting the `LOCK` clause. The supported values are:

- `DEFAULT`: Acquire the least restrictive lock on the table that is supported for the specific operation. Permit the maximum amount of concurrency that is supported for the specific operation.
- `NONE`: Acquire no lock on the table. Permit <strong>all</strong> concurrent DML. If this locking strategy is not permitted for an operation, then an error is raised.
- `SHARED`: Acquire a read lock on the table. Permit <strong>read-only</strong> concurrent DML. If this locking strategy is not permitted for an operation, then an error is raised.
- `EXCLUSIVE`: Acquire a write lock on the table. Do <strong>not</strong> permit concurrent DML.

Different storage engines support different locking strategies for different operations. If a specific locking strategy is chosen for an `ALTER TABLE` operation, and that table's storage engine does not support that locking strategy for that specific operation, then an error will be raised.

If the `LOCK` clause is not explicitly set, then the operation uses `LOCK=DEFAULT`.

<a undefined>ALTER ONLINE TABLE</a> is equivalent to `LOCK=NONE`. Therefore, the <a undefined>ALTER ONLINE TABLE</a> statement can be used to ensure that your `ALTER TABLE` operation allows all concurrent DML.

See [InnoDB Online DDL Overview: LOCK](/kb/en/innodb-online-ddl-overview/#lock) for information on how the `LOCK` clause affects InnoDB.

## Progress Reporting

MariaDB provides progress reporting for `ALTER TABLE` statement for clients
that support the new progress reporting protocol. For example, if you were using the [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) client, then the progress report might look like this::

```sql
ALTER TABLE test ENGINE=Aria;
Stage: 1 of 2 'copy to tmp table'    46% of stage
```

The progress report is also shown in the output of the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) statement and in the contents of the <a undefined>information_schema.PROCESSLIST</a> table.

See [Progress Reporting](/kb/en/progress-reporting/) for more information.

## Aborting ALTER TABLE Operations

If an `ALTER TABLE` operation is being performed and the connection is killed, the changes will be rolled back in a controlled manner. The rollback can be a slow operation as the time it takes is relative to how far the operation has progressed.

##### MariaDB starting with [10.2.13](/kb/en/mariadb-10213-release-notes/)

Aborting `ALTER TABLE ... ALGORITHM=COPY`  was made faster by removing excessive undo logging ([MDEV-11415](https://jira.mariadb.org/browse/MDEV-11415)).  This significantly shortens the time it takes to abort a running ALTER TABLE operation.

## Examples

Adding a new column:

```sql
ALTER TABLE t1 ADD x INT;
```

Dropping a column:

```sql
ALTER TABLE t1 DROP x;
```

Modifying the type of a column:

```sql
ALTER TABLE t1 MODIFY x bigint unsigned;
```

Changing the name and type of a column:

```sql
ALTER TABLE t1 CHANGE a b bigint unsigned auto_increment;
```

Combining multiple clauses in a single ALTER TABLE statement, separated by commas:

```sql
ALTER TABLE t1 DROP x, ADD x2 INT,  CHANGE y y2 INT;
```

Changing the storage engine and adding a comment:

```sql
ALTER TABLE t1 
  ENGINE = InnoDB 
  COMMENT = 'First of three tables containing usage info';
```

Rebuilding the table (the previous example will also rebuild the table if it was already InnoDB):

```sql
ALTER TABLE t1 FORCE;
```

Dropping an index:

```sql
ALTER TABLE rooms DROP INDEX u;
```

Adding a unique index:

```sql
ALTER TABLE rooms ADD UNIQUE INDEX u(room_number);
```

From [MariaDB 10.5.3](/kb/en/mariadb-1053-release-notes/), adding a primary key for an [application-time period table](/sql-statements-structure/temporal-tables/application-time-periods/) with a [WITHOUT OVERLAPS](/kb/en/application-time-periods/#without-overlaps) constraint:

```sql
ALTER TABLE rooms ADD PRIMARY KEY(room_number, p WITHOUT OVERLAPS);
```

## See Also

- [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/)
- [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/)
- [Character Sets and Collations](/kb/en/character-sets-and-collations/)
- [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/)
- [Instant ADD COLUMN for InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/instant-add-column-for-innodb/)