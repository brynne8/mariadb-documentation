# CREATE TABLE

## Syntax

```sql
CREATE [OR REPLACE] [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
    (create_definition,...) [table_options    ]... [partition_options]
CREATE [OR REPLACE] [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
    [(create_definition,...)] [table_options   ]... [partition_options]
    select_statement
CREATE [OR REPLACE] [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
   { LIKE old_table_name | (LIKE old_table_name) }
select_statement:
    [IGNORE | REPLACE] [AS] SELECT ...   (Some legal select statement)```

## Description

Use the `CREATE TABLE` statement to create a table with the given name.

In its most basic form, the `CREATE TABLE` statement provides a table name
followed by a list of columns, indexes, and constraints. By default, the table
is created in the default database. Specify a database with `<em>db_name</em>.<em>tbl_name</em>`.
If you quote the table name, you must quote the database name and table name
separately as ``<em>db_name</em>`.`<em>tbl_name</em>``. This is particularly useful for [CREATE TABLE ... SELECT](#create-table-select), because it allows to create a table into a database, which contains data from other databases. See [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers).

If a table with the same name exists, error 1050 results. Use [IF NOT EXISTS](#create-table-if-not-exists)
to suppress this error and issue a note instead. Use [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings)
to see notes.

The `CREATE TABLE` statement automatically commits the current transaction,
except when using the [TEMPORARY](#create-temporary-table) keyword.

For valid identifiers to use as table names, see [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names).

<strong>Note:</strong> if the default_storage_engine is set to ColumnStore then it needs setting on all UMs. Otherwise when the tables using the default engine are replicated across UMs they will use the wrong engine. You should therefore not use this option as a session variable with ColumnStore.

[Microsecond precision](/built-in-functions/date-time-functions/microseconds-in-mariadb) can be between 0-6. If no precision is specified it is assumed to be 0, for backward compatibility reasons.

## Privileges

Executing the `CREATE TABLE` statement requires the [CREATE](/kb/en/grant/#table-privileges) privilege for the table or the database.

## CREATE OR REPLACE

##### MariaDB starting with [10.0.8](/kb/en/mariadb-1008-release-notes/)

The `OR REPLACE` clause was added in [MariaDB 10.0.8](/kb/en/mariadb-1008-release-notes/).

If the `OR REPLACE` clause is used and if the table already exists, then instead of returning an error, the server will drop the existing table and replace it with the newly defined table.

This syntax was originally added to make [replication](/replication) more robust if it has to rollback and repeat statements such as `CREATE ... SELECT` on slaves.

```sql
CREATE OR REPLACE TABLE table_name (a int);
```

is basically the same as:

```sql
DROP TABLE IF EXISTS table_name;
CREATE TABLE table_name (a int);
```

with the following exceptions:

- If <code class="highlight fixed" style="white-space:pre-wrap">table_name</code> was locked with [LOCK TABLES](/kb/en/lock-tables-and-unlock-tables/) it will continue to be locked after the statement.
- Temporary tables are only dropped if the <code class="highlight fixed" style="white-space:pre-wrap">TEMPORARY</code> keyword was used.  (With [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table),  temporary tables are preferred to be dropped before normal tables).

### Things to be Aware of With CREATE OR REPLACE

- The table is dropped first (if it existed), after that the <code class="highlight fixed" style="white-space:pre-wrap">CREATE</code> is done. Because of this, if the <code class="highlight fixed" style="white-space:pre-wrap">CREATE</code> fails, then the table will not exist anymore after the statement.  If the table was used with <code class="highlight fixed" style="white-space:pre-wrap">LOCK TABLES</code> it will be unlocked.
- One can't use <code class="highlight fixed" style="white-space:pre-wrap">OR REPLACE</code> together with <code class="highlight fixed" style="white-space:pre-wrap">IF EXISTS</code>.
- Slaves in replication will by default use <code class="highlight fixed" style="white-space:pre-wrap">CREATE OR REPLACE</code> when replicating <code class="highlight fixed" style="white-space:pre-wrap">CREATE</code> statements that don''t use <code class="highlight fixed" style="white-space:pre-wrap">IF EXISTS</code>. This can be changed by setting the variable [slave-ddl-exec-mode](/kb/en/replication-and-binary-log-server-system-variables/#slave_ddl_exec_mode) to <code class="highlight fixed" style="white-space:pre-wrap">STRICT</code>.

## CREATE TABLE IF NOT EXISTS

If the `IF NOT EXISTS` clause is used, then the table will only be created if a table with the same name does not already exist. If the table already exists, then a warning will be triggered by default.

## CREATE TEMPORARY TABLE

Use the `TEMPORARY` keyword to create a temporary table that is only available to the current session. Temporary tables are dropped when the session ends. Temporary table names are specific to the session. They will not conflict with other temporary tables from other sessions even if they share the same name. They will shadow names of non-temporary tables or views, if they are identical. A temporary table can have the same name as a non-temporary table which is located in the same database. In that case, their name will reference the temporary table when used in SQL statements. You must have the [CREATE TEMPORARY TABLES](/kb/en/grant/#database-privileges) privilege on the database to create temporary tables. If no storage engine is specified, the [default_tmp_storage_engine](/kb/en/server-system-variables/#default_tmp_storage_engine) setting will determine the engine.

## CREATE TABLE ... LIKE

Use the `LIKE` clause instead of a full table definition to create a table with the same definition as another table, including columns, indexes, and table options. Foreign key definitions, as well as any DATA DIRECTORY or INDEX DIRECTORY table options specified on the original table, will not be created.

## CREATE TABLE ... SELECT

You can create a table containing data from other tables using the `CREATE ... SELECT` statement. Columns will be created in the table for each field returned by the `SELECT` query.

You can also define some columns normally and add other columns from a `SELECT`. You can also create columns in the normal way and assign them some values using the query, this is done to force a certain type or other field characteristics. The columns that are not named in the query will be placed before the others. For example:

```sql
CREATE TABLE test (a INT NOT NULL, b CHAR(10)) ENGINE=MyISAM
    SELECT 5 AS b, c, d FROM another_table;
```

Remember that the query just returns data. If you want to use the same indexes, or the same columns attributes (`[NOT] NULL`, `DEFAULT`, `AUTO_INCREMENT`) in the new table, you need to specify them manually. Types and sizes are not automatically preserved if no data returned by the `SELECT` requires the full size, and `VARCHAR` could be converted into `CHAR`. The [CAST()](/built-in-functions/string-functions/cast) function can be used to forcee the new table to use certain types.

Aliases (`AS`) are taken into account, and they should always be used when you `SELECT` an expression (function, arithmetical operation, etc).

If an error occurs during the query, the table will not be created at all.

If the new table has a primary key or `UNIQUE` indexes, you can use the [IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/ignore) or `REPLACE` keywords to handle duplicate key errors during the query. `IGNORE` means that the newer values must not be inserted an identical value exists in the index. `REPLACE` means that older values must be overwritten.

If the columns in the new table are more than the rows returned by the query, the columns populated by the query will be placed after other columns. Note that if the strict `SQL_MODE` is on, and the columns that are not names in the query do not have a `DEFAULT` value, an error will raise and no rows will be copied.

[Concurrent inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts) are not used during the execution of a `CREATE ... SELECT`.

If the table already exists, an error similar to the following will be returned:

```sql
ERROR 1050 (42S01): Table 't' already exists
```

If the `IF NOT EXISTS` clause is used and the table exists, a note will be produced instead of an error.

To insert rows from a query into an existing table, [INSERT ... SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select) can be used.

## Column Definitions

```sql
create_definition:
  { col_name column_definition | index_definition | period_definition | CHECK (expr) }
column_definition:
  data_type
    [NOT NULL | NULL] [DEFAULT default_value | (expression)]
    [ON UPDATE [NOW | CURRENT_TIMESTAMP] [(precision)]]
    [AUTO_INCREMENT] [ZEROFILL] [UNIQUE [KEY] | [PRIMARY] KEY]
    [INVISIBLE] [{WITH|WITHOUT} SYSTEM VERSIONING]
    [COMMENT 'string'] [REF_SYSTEM_ID = value]
    [reference_definition]
  | data_type [GENERATED ALWAYS] 
  AS { { ROW {START|END} } | { (expression) [VIRTUAL | PERSISTENT | STORED] } }
      [UNIQUE [KEY]] [COMMENT 'string']
constraint_definition:
   CONSTRAINT [constraint_name] CHECK (expression)```

<strong>Note:</strong> MariaDB accepts the `REFERENCES` clause in `ALTER TABLE` and `CREATE TABLE` column definitions, but that syntax does nothing. MariaDB simply parses it without returning any error or warning, for compatibility with other DBMS's. Before [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) this was also true for `CHECK` constraints. Only the syntax for indexes described below creates foreign keys.

Each definition either creates a column in the table or specifies and index or
constraint on one or more columns. See [Indexes](#indexes) below for details
on creating indexes.

Create a column by specifying a column name and a data type, optionally
followed by column options. See [Data Types](/columns-storage-engines-and-plugins/data-types) for a full list
of data types allowed in MariaDB.

### NULL and NOT NULL

Use the `NULL` or `NOT NULL` options to specify that values in the column
may or may not be `NULL`, respectively. By default, values may be `NULL`. See also [NULL Values in MariaDB](/kb/en/null-values-in-mariadb/).

### DEFAULT Column Option

##### MariaDB starting with [10.2.1](/kb/en/mariadb-1021-release-notes/)

The `DEFAULT` clause was enhanced in [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/). Some enhancements include

- [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text) columns now support `DEFAULT`.
- The `DEFAULT` clause can now be used with an expression or function.

Specify a default value using the `DEFAULT` clause. If you don't specify `DEFAULT` then the following rules apply:

- If the column is not defined with `NOT NULL`, `AUTO_INCREMENT` or `TIMESTAMP`, an explicit `DEFAULT NULL` will be added.
Note that in MySQL and in MariaDB before 10.1.6, you may get an explicit `DEFAULT` for primary key parts, if not specified with NOT NULL.

The default value will be used if you [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert) a row without specifying a value for that column, or if you specify [DEFAULT](/built-in-functions/secondary-functions/information-functions/default) for that column.
Before [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) you couldn't usually provide an expression or function to evaluate at
insertion time. You had to provide a constant default value instead. The one
exception is that you may use [CURRENT_TIMESTAMP](/built-in-functions/date-time-functions/now) as
the default value for a [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp) column to use the current
timestamp at insertion time.

##### MariaDB starting with [10.0.1](/kb/en/mariadb-1001-release-notes/)

[CURRENT_TIMESTAMP](/built-in-functions/date-time-functions/now) may also be used as
the default value for a [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime)

From [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) you can use most functions in `DEFAULT`.  Expressions should have parentheses around them. If you use a non deterministic function in `DEFAULT` then all inserts to the table will be [replicated](/replication) in [row mode](/kb/en/binary-log-formats/#row-based). You can even refer to earlier columns in the `DEFAULT` expression:

```sql
CREATE TABLE t1 (a int DEFAULT (1+1), b int DEFAULT (a+1));
CREATE TABLE t2 (a bigint primary key DEFAULT UUID_SHORT());
```

The `DEFAULT` clause cannot contain any [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions) or [subqueries](/kb/en/subqueries/), and a column used in the clause must already have been defined earlier in the statement.

Since [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/), it is possible to assign [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob) or [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text) columns a `DEFAULT` value. In earlier versions, assigning a default to these columns was not possible.

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

Starting from 10.3.3 you can also use DEFAULT ([NEXT VALUE FOR sequence](/sql-statements-structure/sequences/sequence-functions/next-value-for-sequence_name))

### AUTO_INCREMENT Column Option

Use [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment) to create a column whose value can
can be set automatically from a simple counter. You can only use `AUTO_INCREMENT`
on a column with an integer type. The column must be a key, and there can only be
one `AUTO_INCREMENT` column in a table. If you insert a row without specifying
a value for that column (or if you specify `0`, `NULL`, or [DEFAULT](/built-in-functions/secondary-functions/information-functions/default)
as the value), the actual value will be taken from the counter, with each insertion
incrementing the counter by one. You can still insert a value explicitly. If you
insert a value that is greater than the current counter value, the counter is
set based on the new value. An `AUTO_INCREMENT` column is implicitly `NOT NULL`.
Use [LAST_INSERT_ID](/built-in-functions/secondary-functions/information-functions/last_insert_id) to get the [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment) value
most recently used by an [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert) statement.

### ZEROFILL Column Option

If the `ZEROFILL` column option is specified for a column using a [numeric](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview) data type, then the column will be set to `UNSIGNED` and the spaces used by default to pad the field are replaced with zeros. `ZEROFILL` is ignored in expressions or as part of a [UNION](/kb/en/union/). `ZEROFILL` is a non-standard MySQL and MariaDB enhancement.

### PRIMARY KEY Column Option

Use `PRIMARY KEY` (or just `KEY`) to make a column a primary key. A primary key is a special type of a unique key. There can be at most one primary key per table, and it is implicitly `NOT NULL`.

Specifying a column as a unique key creates a unique index on that column. See the [Index Definitions](#index-definitions) section below for more information.

### UNIQUE KEY Column Option

Use `UNIQUE KEY` (or just `UNIQUE`) to specify that all values in the column
must be distinct from each other. Unless the column is `NOT NULL`, there may be
multiple rows with `NULL` in the column.

Specifying a column as a unique key creates a unique index on that column. See the [Index Definitions](#index-definitions) section below for more information.

### COMMENT Column Option

You can provide a comment for each column using the `COMMENT` clause. The maximum length is 1024 characters (it was 255 characters before [MariaDB 5.5](/kb/en/what-is-mariadb-55/)). Use
the [SHOW FULL COLUMNS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-columns) statement to see column comments.

### REF_SYSTEM_ID

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

`REF_SYSTEM_ID` can be used to specify Spatial Reference System IDs for spatial data type columns.

### Generated Columns

A generated column is a column in a table that cannot explicitly be set to a specific value in a [DML query](/sql-statements-structure/sql-statements/data-manipulation). Instead, its value is automatically generated based on an expression. This expression might generate the value based on the values of other columns in the table, or it might generate the value by calling [built-in functions](/built-in-functions) or [user-defined functions (UDFs)](/programming-customizing-mariadb/user-defined-functions).

There are two types of generated columns:

- `PERSISTENT` or `STORED`: This type's value is actually stored in the table.
- `VIRTUAL`: This type's value is not stored at all. Instead, the value is generated dynamically when the table is queried. This type is the default.

Generated columns are also sometimes called computed columns or virtual columns.

For a complete description about generated columns and their limitations, see [Generated (Virtual and Persistent/Stored) Columns](/sql-statements-structure/sql-statements/data-definition/create/generated-columns).

### COMPRESSED

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

Certain columns may be compressed. See [Storage-Engine Independent Column Compression](/replication/optimization-and-tuning/optimization-and-tuning-compression/storage-engine-independent-column-compression).

### INVISIBLE

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

Columns may be made invisible, and hidden in certain contexts. See [Invisible Columns](/sql-statements-structure/sql-statements/data-definition/create/invisible-columns).

### WITH SYSTEM VERSIONING Column Option

##### MariaDB starting with [10.3.4](/kb/en/mariadb-1034-release-notes/)

Columns may be explicitly marked as included from system versioning. See [System-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables) for details.

### WITHOUT SYSTEM VERSIONING Column Option

##### MariaDB starting with [10.3.4](/kb/en/mariadb-1034-release-notes/)

Columns may be explicitly marked as excluded from system versioning. See [System-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables) for details.

## Index Definitions

```sql
index_definition:
    {INDEX|KEY} [index_name] [index_type] (index_col_name,...) [index_option] ...
  | {FULLTEXT|SPATIAL} [INDEX|KEY] [index_name] (index_col_name,...) [index_option] ...
  | [CONSTRAINT [symbol]] PRIMARY KEY [index_type] (index_col_name,...) [index_option] ...
  | [CONSTRAINT [symbol]] UNIQUE [INDEX|KEY] [index_name] [index_type] (index_col_name,...) [index_option] ...
  | [CONSTRAINT [symbol]] FOREIGN KEY [index_name] (index_col_name,...) reference_definition
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
reference_definition:
    REFERENCES tbl_name (index_col_name,...)
      [MATCH FULL | MATCH PARTIAL | MATCH SIMPLE]
      [ON DELETE reference_option]
      [ON UPDATE reference_option]
reference_option:
    RESTRICT | CASCADE | SET NULL | NO ACTION```

`INDEX` and `KEY` are synonyms.

Index names are optional, if not specified an automatic name will be assigned. Index name are needed to drop indexes and appear in error messages when a constraint is violated.

### Index Categories

#### Plain Indexes

Plain indexes are regular indexes that are not unique, and are not acting as a primary key or a foreign key. They are also not the "specialized" `FULLTEXT` or `SPATIAL` indexes.

See [Getting Started with Indexes: Plain Indexes](/kb/en/getting-started-with-indexes/#plain-indexes) for more information.

#### PRIMARY KEY

For `PRIMARY KEY` indexes, you can specify a name for the index, but it is ignored, and the name of the index is always `PRIMARY`. From [MariaDB 10.3.18](/kb/en/mariadb-10318-release-notes/) and [MariaDB 10.4.8](/kb/en/mariadb-1048-release-notes/), a warning is explicitly issued if a name is specified. Before then, the name was silently ignored.

See [Getting Started with Indexes: Primary Key](/kb/en/getting-started-with-indexes/#primary-key) for more information.

#### UNIQUE

The `UNIQUE` keyword means that the index will not accept duplicated values, except for NULLs. An error will raise if you try to insert duplicate values in a UNIQUE index.

For `UNIQUE` indexes, you can specify a name for the constraint, using the `CONSTRAINT` keyword. That name will be used in error messages.

See [Getting Started with Indexes: Unique Index](/kb/en/getting-started-with-indexes/#unique-index) for more information.

#### FOREIGN KEY

For `FOREIGN KEY` indexes, a reference definition must be provided.

For `FOREIGN KEY` indexes, you can specify a name for the constraint, using the `CONSTRAINT` keyword. That name will be used in error messages.

First, you have to specify the name of the target (parent) table and a column or a column list which must be indexed and whose values must match to the foreign key's values. The `MATCH` clause is accepted to improve the compatibility with other DBMS's, but has no meaning in MariaDB. The `ON DELETE` and `ON UPDATE` clauses specify what must be done when a `DELETE` (or a `REPLACE`) statements attempts to delete a referenced row from the parent table, and when an `UPDATE` statement attempts to modify the referenced foreign key columns in a parent table row, respectively. The following options are allowed:

- `RESTRICT`: The delete/update operation is not performed.  The statement terminates with a 1451 error (SQLSTATE '2300').
- `NO ACTION`: Synonym for `RESTRICT`.
- `CASCADE`: The delete/update operation is performed in both tables.
- `SET NULL`: The update or delete goes ahead in the parent table, and the corresponding foreign key fields in the child table are set to `NULL`.  (They must not be defined as `NOT NULL` for this to succeed).
- `SET DEFAULT`: This option is currently implemented only for the PBXT storage engine, which is disabled by default and no longer maintained. It sets the child table's foreign key fields to their `DEFAULT` values when the referenced parent table key entries are updated or deleted.

If either clause is omitted, the default behavior for the omitted clause is `RESTRICT`.

See [Foreign Keys](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys) for more information.

#### FULLTEXT

Use the `FULLTEXT` keyword to create full-text indexes.

See [Full-Text Indexes](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes) for more information.

#### SPATIAL

Use the `SPATIAL` keyword to create geometric indexes.

See [SPATIAL INDEX](/sql-statements-structure/geographic-geometric-features/spatial-index) for more information.

### Index Options

#### KEY_BLOCK_SIZE Index Option

The `KEY_BLOCK_SIZE` index option is similar to the [KEY_BLOCK_SIZE](#key_block_size) table option.

With the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) storage engine, if you specify a non-zero value for the `KEY_BLOCK_SIZE` table option for the whole table, then the table will implicitly be created with the [ROW_FORMAT](#row_format) table option set to `COMPRESSED`. However, this does not happen if you just set the `KEY_BLOCK_SIZE` index option for one or more indexes in the table. The [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) storage engine ignores the `KEY_BLOCK_SIZE` index option. However, the [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table) statement may still report it for the index.

For information about the `KEY_BLOCK_SIZE` index option, see the [KEY_BLOCK_SIZE](#key_block_size) table option below.

#### Index Types

Each storage engine supports some or all index types. See [Storage Engine Index Types](/replication/optimization-and-tuning/optimization-and-indexes/storage-engine-index-types) for details on permitted index types for each storage engine.

Different index types are optimized for different kind of operations:

- `BTREE` is the default type, and normally is the best choice. It is supported by all storage engines. It can be used to compare a column's value with a value using the =, &gt;, &gt;=, &lt;, &lt;=, `BETWEEN`, and `LIKE` operators. `BTREE` can also be used to find `NULL` values. Searches against an index prefix are possible.
- `HASH` is only supported by the MEMORY storage engine. `HASH` indexes can only be used for =, &lt;=, and &gt;= comparisons. It can not be used for the `ORDER BY` clause. Searches against an index prefix are not possible.
- `RTREE` is the default for [`SPATIAL`](/kb/en/spatial/) indexes, but if the storage engine does not support it `BTREE` can be used.

Index columns names are listed between parenthesis. After each column, a prefix length can be specified. If no length is specified, the whole column will be indexed. `ASC` and `DESC` can be specified for compatibility with are DBMS's, but have no meaning in MariaDB.

#### WITH PARSER Index Option

The `WITH PARSER` index option only applies to [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes) indexes and contains the fulltext parser name. The fulltext parser must be an installed plugin.

#### COMMENT Index Option

A comment of up to 1024 characters is permitted with the `COMMENT` index option.

The `COMMENT` index option allows you to specify a comment with user-readable text describing what the index is for. This information is not used by the server itself.

#### CLUSTERING Index Option

The `CLUSTERING` index option is only valid for tables using the [Tokudb](/columns-storage-engines-and-plugins/storage-engines/tokudb) storage engine.

## Periods

##### MariaDB starting with [10.3.4](/kb/en/mariadb-1034-release-notes/)

```sql
period_definition:
    PERIOD FOR SYSTEM_TIME (start_column_name, end_column_name)```

MariaDB supports a subset of the standard syntax for periods. At the moment it's only used for creating [System-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables). Both columns must be created, must be either of a `TIMESTAMP(6)` or `BIGINT UNSIGNED` type, and be generated as `ROW START` and `ROW END` accordingly. See [System-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables) for details.

The table must also have the `WITH SYSTEM VERSIONING` clause.

## Constraint Expressions

##### MariaDB starting with [10.2.1](/kb/en/mariadb-1021-release-notes/)

[MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) introduced new ways to define a constraint.

Note: Before [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/), constraint expressions were accepted in the syntax but ignored.

[MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) introduced two ways to define a constraint:

- `CHECK(expression)` given as part of a column definition.
- `CONSTRAINT [constraint_name] CHECK (expression)`

Before a row is inserted or updated, all constraints are evaluated in the order they are defined. If any constraints fails, then the row will not be updated.
One can use most deterministic functions in a constraint, including [UDFs](/programming-customizing-mariadb/user-defined-functions).

```sql
create table t1 (a int check(a>0) ,b int check (b> 0), constraint abc check (a>b));
```

If you use the second format and you don't give a name to the constraint, then the constraint will get a auto generated name. This is done so that you can later delete the constraint with [ALTER TABLE DROP constraint_name](/sql-statements-structure/sql-statements/data-definition/alter/alter-table).

One can disable all constraint expression checks by setting the variable `check_constraint_checks` to `OFF`. This is useful for example when loading a table that violates some constraints that you want to later find and fix in SQL.

See [CONSTRAINT](/sql-statements-structure/sql-statements/data-definition/constraint) for more information.

## Table Options

For each individual table you create (or alter), you can set some table options. The general syntax for setting options is:

&lt;OPTION_NAME&gt; = &lt;option_value&gt;, [&lt;OPTION_NAME&gt; = &lt;option_value&gt; ...]

The equal sign is optional.

Some options are supported by the server and can be used for all tables, no matter what storage engine they use; other options can be specified for all storage engines, but have a meaning only for some engines. Also, engines can [extend `CREATE TABLE` with new options](/kb/en/extending-create-table/).

If the `IGNORE_BAD_TABLE_OPTIONS` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) is enabled, wrong table options generate a warning; otherwise, they generate an error.

```sql
table_option:    
    [STORAGE] ENGINE [=] engine_name
  | AUTO_INCREMENT [=] value
  | AVG_ROW_LENGTH [=] value
  | [DEFAULT] CHARACTER SET [=] charset_name
  | CHECKSUM [=] {0 | 1}
  | [DEFAULT] COLLATE [=] collation_name
  | COMMENT [=] 'string'
  | CONNECTION [=] 'connect_string'
  | DATA DIRECTORY [=] 'absolute path to directory'
  | DELAY_KEY_WRITE [=] {0 | 1}
  | ENCRYPTED [=] {YES | NO}
  | ENCRYPTION_KEY_ID [=] value
  | IETF_QUOTES [=] {YES | NO}
  | INDEX DIRECTORY [=] 'absolute path to directory'
  | INSERT_METHOD [=] { NO | FIRST | LAST }
  | KEY_BLOCK_SIZE [=] value
  | MAX_ROWS [=] value
  | MIN_ROWS [=] value
  | PACK_KEYS [=] {0 | 1 | DEFAULT}
  | PAGE_CHECKSUM [=] {0 | 1}
  | PAGE_COMPRESSED [=] {0 | 1}
  | PAGE_COMPRESSION_LEVEL [=] {0 .. 9}
  | PASSWORD [=] 'string'
  | ROW_FORMAT [=] {DEFAULT|DYNAMIC|FIXED|COMPRESSED|REDUNDANT|COMPACT|PAGE}
  | SEQUENCE [=] {0|1}
  | STATS_AUTO_RECALC [=] {DEFAULT|0|1}
  | STATS_PERSISTENT [=] {DEFAULT|0|1}
  | STATS_SAMPLE_PAGES [=] {DEFAULT|value}
  | TABLESPACE tablespace_name
  | TRANSACTIONAL [=]  {0 | 1}
  | UNION [=] (tbl_name[,tbl_name]...)
  | WITH SYSTEM VERSIONING```

### [STORAGE] ENGINE

`[STORAGE] ENGINE` specifies a [storage engine](/kb/en/mariadb-storage-engines/) for the table. If this option is not used, the default storage engine is used instead. That is, the `storage_engine` session option value if it is set, or the value specified for the [--default-storage-engine](/kb/en/mysqld-options-full-list/) mysqld startup options, or InnoDB. If the specified storage engine is not installed and active, the default value will be used, unless the `NO_ENGINE_SUBSTITUTION` [SQL MODE](/mariadb-administration/variables-and-modes/sql-mode) is set (default since [MariaDB 10.0](/kb/en/what-is-mariadb-100/)). This is only true for `CREATE TABLE`, not for `ALTER TABLE`. For a list of storage engines that are present in your server, issue a [SHOW ENGINES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engines).

### AUTO_INCREMENT

`AUTO_INCREMENT` specifies the initial value for the [`AUTO_INCREMENT`](/columns-storage-engines-and-plugins/data-types/auto_increment) primary key. This works for MyISAM, Aria, InnoDB/XtraDB, MEMORY, and ARCHIVE tables. You can change this option with `ALTER TABLE`, but in that case the new value must be higher than the highest value which is present in the `AUTO_INCREMENT` column. If the storage engine does not support this option, you can insert (and then delete) a row having the wanted value - 1 in the `AUTO_INCREMENT` column.

### AVG_ROW_LENGTH

`AVG_ROW_LENGTH` is the average rows size. It only applies to tables using [MyISAM](/kb/en/myisam/) and [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine) storage engines that have the [ROW_FORMAT](#row_format) table option set to `FIXED` format.

MyISAM uses `MAX_ROWS` and `AVG_ROW_LENGTH` to decide the maximum size of a table (default: 256TB, or the maximum file size allowed by the system).

### [DEFAULT] CHARACTER SET/CHARSET

`[DEFAULT] CHARACTER SET` (or `[DEFAULT] CHARSET`) is used to set a default character set for the table. This is the character set used for all columns where an explicit character set is not specified. If this option is omitted or `DEFAULT` is specified, database's default character set will be used. See [Setting Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/setting-character-sets-and-collations) for details on setting the [character sets](/kb/en/data-types-character-sets-and-collations/).

### CHECKSUM/TABLE_CHECKSUM

`CHECKSUM` (or `TABLE_CHECKSUM`) can be set to 1 to maintain a live checksum for all table's rows. This makes write operations slower, but [`CHECKSUM TABLE`](/sql-statements-structure/sql-statements/table-statements/checksum-table) will be very fast. This option is only supported for [MyISAM](/kb/en/myisam/) and [Aria tables](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine).

### [DEFAULT] COLLATE

`[DEFAULT] COLLATE` is used to set a default collation for the table. This is the collation used for all columns where an explicit character set is not specified. If this option is omitted or `DEFAULT` is specified, database's default option will be used. See [Setting Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/setting-character-sets-and-collations) for details on setting the [collations](/kb/en/data-types-character-sets-and-collations/)

### COMMENT

`COMMENT` is a comment for the table. Maximum length is 2048 characters (before [mariaDB 5.5](/kb/en/what-is-mariadb-55/) it was 60 characters). Also used to define table parameters when creating a [Spider](/columns-storage-engines-and-plugins/storage-engines/spider) table.

### CONNECTION

`CONNECTION` is used to specify a server name or a connection string for a [Spider](/columns-storage-engines-and-plugins/storage-engines/spider), [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect), [Federated or FederatedX table](/columns-storage-engines-and-plugins/storage-engines/federatedx-storage-engine/about-federatedx).

### DATA DIRECTORY/INDEX DIRECTORY

`DATA DIRECTORY` and `INDEX DIRECTORY` were only supported for MyISAM and Aria, before [MariaDB 5.5](/kb/en/what-is-mariadb-55/). Since 5.5, DATA DIRECTORY has also been supported by InnoDB if the [innodb_file_per_table](/kb/en/innodb-system-variables/#innodb_file_per_table) server system variable is enabled, but only in CREATE TABLE, not in [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table). So, carefully choose a path for InnoDB tables at creation time, because it cannot be changed without dropping and re-creating the table. These options specify the paths for data files and index files, respectively. If these options are omitted, the database's directory will be used to store data files and index files. Note that these table options do not work for [partitioned](/kb/en/managing-mariadb-partitioning/) tables (use the partition options instead), or if the server has been invoked with the [`--skip-symbolic-links` startup option](/kb/en/mysqld-options-full-list/). To avoid the overwriting of old files with the same name that could be present in the directories, you can use [the `--keep_files_on_create` option](/kb/en/mysqld-options-full-list/) (an error will be issued if files already exist). These options are ignored if the `NO_DIR_IN_CREATE` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) is enabled (useful for replication slaves). Also note that symbolic links cannot be used for InnoDB tables.

`DATA DIRECTORY` works by creating symlinks from where the table would normally have been (inside the [datadir](/kb/en/server-system-variables/#datadir)) to where the option specifies. For security reasons, to avoid bypassing the privilege system, the server does not permit symlinks inside the datadir. Therefore, `DATA DIRECTORY` cannot be used to specify a location inside the datadir. An attempt to do so will result in an error `1210 (HY000) Incorrect arguments to DATA DIRECTORY`.

### DELAY_KEY_WRITE

`DELAY_KEY_WRITE` is supported by MyISAM and Aria, and can be set to 1 to speed up write operations. In that case, when data are modified, the indexes are not updated until the table is closed. Writing the changes to the index file altogether can be much faster. However, note that this option is applied only if the `delay_key_write` server variable is set to 'ON'. If it is 'OFF' the delayed index writes are always disabled, and if it is 'ALL' the delayed index writes are always used, disregarding the value of `DELAY_KEY_WRITE`.

### ENCRYPTED

##### MariaDB starting with [10.1.4](/kb/en/mariadb-1014-release-notes/)

The `ENCRYPTED` table option was added in [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

The `ENCRYPTED` table option can be used to manually set the encryption status of an [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) table. See [InnoDB / XtraDB Encryption](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption) for more information.

Aria does not currently support the `ENCRYPTED` table option. See [MDEV-18049](https://jira.mariadb.org/browse/MDEV-18049) about that.

See [Data-at-Rest Encryption](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption) for more information.

### ENCRYPTION_KEY_ID

##### MariaDB starting with [10.1.4](/kb/en/mariadb-1014-release-notes/)

The `ENCRYPTION_KEY_ID` table option was added in [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

The `ENCRYPTION_KEY_ID` table option can be used to manually set the encryption key of an [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) table. See [InnoDB / XtraDB Encryption](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption) for more information.

Aria does not currently support the `ENCRYPTION_KEY_ID` table option. See [MDEV-18049](https://jira.mariadb.org/browse/MDEV-18049) about that.

See [Data-at-Rest Encryption](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption) for more information.

### IETF_QUOTES

##### MariaDB starting with [10.1.8](/kb/en/mariadb-1018-release-notes/)

The IETF_QUOTES option was added in [MariaDB 10.1.8](/kb/en/mariadb-1018-release-notes/)

For the [CSV](/columns-storage-engines-and-plugins/storage-engines/csv) storage engine, the `IETF_QUOTES` option, when set to `YES`, enables IETF-compatible parsing of embedded quote and comma characters. Enabling this option for a table improves compatibility with other tools that use CSV, but is not compatible with MySQL CSV tables, or MariaDB CSV tables created without this option. Disabled by default.

### INSERT_METHOD

`INSERT_METHOD` is only used with [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge) tables. This option determines in which underlying table the new rows should be inserted. If you set it to 'NO' (which is the default) no new rows can be added to the table (but you will still be able to perform `INSERT`s directly against the underlying tables). `FIRST` means that the rows are inserted into the first table, and `LAST` means that thet are inserted into the last table.

### KEY_BLOCK_SIZE

`KEY_BLOCK_SIZE` is used to determine the size of key blocks, in bytes or kilobytes. However, this value is just a hint, and the storage engine could modify or ignore it. If `KEY_BLOCK_SIZE` is set to 0, the storage engine's default value will be used.

With the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) storage engine, if you specify a non-zero value for the `KEY_BLOCK_SIZE` table option for the whole table, then the table will implicitly be created with the [ROW_FORMAT](#row_format) table option set to `COMPRESSED`.

### MIN_ROWS/MAX_ROWS

`MIN_ROWS` and `MAX_ROWS` let the storage engine know how many rows you are planning to store as a minimum and as a maximum. These values will not be used as real limits, but they help the storage engine to optimize the table. `MIN_ROWS` is only used by MEMORY storage engine to decide the minimum memory that is always allocated. `MAX_ROWS` is used to decide the minimum size for indexes.

### PACK_KEYS

`PACK_KEYS` can be used to determine whether the indexes will be compressed. Set it to 1 to compress all keys. With a value of 0, compression will not be used. With the `DEFAULT` value, only long strings will be compressed. Uncompressed keys are faster.

### PAGE_CHECKSUM

`PAGE_CHECKSUM` is only applicable to [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) tables, and determines whether indexes and data should use page checksums for extra safety.

### PAGE_COMPRESSED

`PAGE_COMPRESSED` is used to enable [InnoDB page compression](/kb/en/compression/) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables.

### PAGE_COMPRESSION_LEVEL

`PAGE_COMPRESSION_LEVEL` is used to set the compression level for [InnoDB page compression](/kb/en/compression/) for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables. The table must also have the [PAGE_COMPRESSED](#page_compressed) table option set to `1`.

Valid values for `PAGE_COMPRESSION_LEVEL` are 1 (the best speed) through 9 (the best compression), .

### PASSWORD

`PASSWORD` is unused.

### RAID_TYPE

`RAID_TYPE` is an obsolete option, as the raid support has been disabled since MySQL 5.0.

### ROW_FORMAT

The `ROW_FORMAT` table option specifies the row format for the data file. Possible values are engine-dependent.

#### Supported MyISAM Row Formats

For [MyISAM](/kb/en/myisam/), the supported row formats are:

- `FIXED`
- `DYNAMIC`
- `COMPRESSED`

The `COMPRESSED` row format can only be set by the [myisampack](/clients-utilities/myisam-clients-and-utilities/myisampack) command line tool.

See [MyISAM Storage Formats](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/myisam-storage-formats) for more information.

#### Supported Aria Row Formats

For [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine), the supported row formats are:

- `PAGE`
- `FIXED`
- `DYNAMIC`.

See [Aria Storage Formats](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-formats) for more information.

#### Supported InnoDB Row Formats

For [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb), the supported row formats are:

- `COMPACT`
- `REDUNDANT`
- `COMPRESSED`
- `DYNAMIC`.

If the `ROW_FORMAT` table option is set to `FIXED` for an InnoDB table, then the server will either return an error or a warning depending on the value of the [innodb_strict_mode](/kb/en/innodb-system-variables/#innodb_strict_mode) system variable. If the [innodb_strict_mode](/kb/en/innodb-system-variables/#innodb_strict_mode) system variable is set to `OFF`, then a warning is issued, and MariaDB will create the table using the default row format for the specific MariaDB server version. If the [innodb_strict_mode](/kb/en/innodb-system-variables/#innodb_strict_mode) system variable is set to `ON`, then an error will be raised.

See [InnoDB Storage Formats](/kb/en/innodb-storage-formats/) for more information.

#### Other Storage Engines and ROW_FORMAT

Other storage engines do not support the `ROW_FORMAT` table option.

### SEQUENCE

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

If the table is a [sequence](/sql-statements-structure/sequences), then it will have the `SEQUENCE` set to `1`.

### STATS_AUTO_RECALC

`STATS_AUTO_RECALC` is available only in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)+. It indicates whether to automatically recalculate persistent statistics (see `STATS_PERSISTENT`, below) for an InnoDB table.
If set to `1`, statistics will be recalculated when more than 10% of the data has changed. When set to `0`, stats will be recalculated only when an [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table) is run. If set to `DEFAULT`, or left out, the value set by the [innodb_stats_auto_recalc](/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_auto_recalc) system variable applies. See [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics).

### STATS_PERSISTENT

`STATS_PERSISTENT` is available only in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)+. It indicates whether the InnoDB statistics created by [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table) will remain on disk or not. It can be set to `1` (on disk), `0` (not on disk, the pre-MariaDB 10 behavior), or `DEFAULT` (the same as leaving out the option), in which case the value set by the [innodb_stats_persistent](/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_persistent) system variable will apply. Persistent statistics stored on disk allow the statistics to survive server restarts, and provide better query plan stability. See [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics).

### STATS_SAMPLE_PAGES

`STATS_SAMPLE_PAGES`  is available only in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)+. It indicates how many pages are used to sample index statistics. If 0 or DEFAULT, the default value, the [innodb_stats_sample_pages](/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_sample_pages) value is used. See [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics).

### TRANSACTIONAL

`TRANSACTIONAL` is only applicable for Aria tables. In future Aria tables created with this option will be fully transactional, but currently this provides a form of crash protection. See [Aria Storage Engine](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine) for more details.

### UNION

`UNION` must be specified when you create a MERGE table. This option contains a comma-separated list of MyISAM tables which are accessed by the new table. The list is enclosed between parenthesis. Example: `UNION = (t1,t2)`

### WITH SYSTEM VERSIONING

`WITH SYSTEM VERSIONING` is used for creating [System-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables).

## Partitions

```sql
partition_options:
    PARTITION BY
        { [LINEAR] HASH(expr)
        | [LINEAR] KEY(column_list)
        | RANGE(expr)
        | LIST(expr)
        | SYSTEM_TIME [INTERVAL time_quantity time_unit] [LIMIT num] }
    [PARTITIONS num]
    [SUBPARTITION BY
        { [LINEAR] HASH(expr)
        | [LINEAR] KEY(column_list) }
      [SUBPARTITIONS num]
    ]
    [(partition_definition [, partition_definition] ...)]
partition_definition:
    PARTITION partition_name
        [VALUES {LESS THAN {(expr) | MAXVALUE} | IN (value_list)}]
        [[STORAGE] ENGINE [=] engine_name]
        [COMMENT [=] 'comment_text' ]
        [DATA DIRECTORY [=] 'data_dir']
        [INDEX DIRECTORY [=] 'index_dir']
        [MAX_ROWS [=] max_number_of_rows]
        [MIN_ROWS [=] min_number_of_rows]
        [TABLESPACE [=] tablespace_name]
        [NODEGROUP [=] node_group_id]
        [(subpartition_definition [, subpartition_definition] ...)]
subpartition_definition:
    SUBPARTITION logical_name
        [[STORAGE] ENGINE [=] engine_name]
        [COMMENT [=] 'comment_text' ]
        [DATA DIRECTORY [=] 'data_dir']
        [INDEX DIRECTORY [=] 'index_dir']
        [MAX_ROWS [=] max_number_of_rows]
        [MIN_ROWS [=] min_number_of_rows]
        [TABLESPACE [=] tablespace_name]
        [NODEGROUP [=] node_group_id]```

If the `PARTITION BY` clause is used, the table will be [partitioned](/kb/en/managing-mariadb-partitioning/). A partition method must be explicitly indicated for partitions and subpartitions. Partition methods are:

- `[LINEAR] HASH` creates a hash key which will be used to read and write rows. The partition function can be any valid SQL expression which returns an `INTEGER` number. Thus, it is possible to use the `HASH` method on an integer column, or on functions which accept integer columns as an argument. However, `VALUES LESS THAN` and `VALUES IN` clauses can not be used with `HASH`. An example:

```sql
CREATE TABLE t1 (a INT, b CHAR(5), c DATETIME)
    PARTITION BY HASH ( YEAR(c) );
```

`[LINEAR] HASH` can be used for subpartitions, too.

- `[LINEAR] KEY` is similar to `HASH`, but the index has an even distribution of data. Also, the expression can only be a column or a list of columns. `VALUES LESS THAN` and `VALUES IN` clauses can not be used with `KEY`.
- [RANGE](/mariadb-administration/partitioning-tables/partitioning-types/range-partitioning-type) partitions the rows using on a range of values, using the `VALUES LESS THAN` operator. `VALUES IN` is not allowed with `RANGE`. The partition function can be any valid SQL expression which returns a single value.
- [LIST](/kb/en/list-partitioning/) assigns partitions based on a table's column with a restricted set of possible values. It is similar to `RANGE`, but `VALUES IN` must be used for at least 1 columns, and `VALUES LESS THAN` is disallowed.
- `SYSTEM_TIME` partitioning is used for [System-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables) to store historical data separately from current data.

Only `HASH` and `KEY` can be used for subpartitions, and they can be `[LINEAR]`.

It is possible to define up to 1024 partitions and subpartitions.

The number of defined partitions can be optionally specified as `PARTITION count`. This can be done to avoid specifying all partitions individually. But you can also declare each individual partition and, additionally, specify a `PARTITIONS count` clause; in the case, the number of `PARTITION`s must equal count.

Also see [Partitioning Types Overview](/mariadb-administration/partitioning-tables/partitioning-types/partitioning-types-overview).

## Sequences

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

`CREATE TABLE` can also be used to create a [SEQUENCE](/sql-statements-structure/sequences). See [CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence) and [Sequence Overview](/sql-statements-structure/sequences/sequence-overview).

## Examples

```sql
create table if not exists test (
a bigint auto_increment primary key,
name varchar(128) charset utf8,
key name (name(32))
) engine=InnoDB default charset latin1;
```

This example shows a couple of things:

- Usage of <code class="highlight fixed" style="white-space:pre-wrap">IF NOT EXISTS</code>; If the table already existed, it will not be created.  There will not be any error for the client, just a warning.
- How to create a <code class="highlight fixed" style="white-space:pre-wrap">PRIMARY KEY</code> that is [automatically generated](/columns-storage-engines-and-plugins/data-types/auto_increment).
- How to specify a table-specific [character set](/kb/en/data-types-character-sets-and-collations/) and another for a column.
- How to create an index (<code class="highlight fixed" style="white-space:pre-wrap">name</code>) that is only partly indexed (to save space).

The following clauses will work from [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) only.

```sql
CREATE TABLE t1(
  a int DEFAULT (1+1),
  b int DEFAULT (a+1),
  expires DATETIME DEFAULT(NOW() + INTERVAL 1 YEAR),
  x BLOB DEFAULT USER()
);
```

## See Also

- [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names)
- [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table)
- [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table)
- [Character Sets and Collations](/kb/en/character-sets-and-collations/)
- [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table)
- Storage engines can add their own [attributes for columns, indexes and tables](/columns-storage-engines-and-plugins/storage-engines/storage-engines-storage-engine-development/engine-defined-new-tablefieldindex-attributes).
- Variable [slave-ddl-exec-mode](/kb/en/replication-and-binary-log-server-system-variables/#slave_ddl_exec_mode).