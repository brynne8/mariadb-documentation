# Syntax Differences between MariaDB and SQL Server

This article contains a non-exhaustive list of syntax differences between MariaDB and SQL Server, and is written for SQL Server users that are unfamiliar with MariaDB.

## Compatibility Features

Some features are meant to improve syntax and semantics compatibility between MariaDB versions, between MariaDB and MySQL, and between MariaDB and other DBMSs. This section focuses on compatibility between MariaDB and SQL Server.

### sql_mode and old_mode

SQL semantics and syntax, in MariaDB, are affected by the [sql_mode](/kb/en/sql_mode/) variable. Its value is a comma-separated list of flags, and each of them, if specified, affects a different aspect of SQL syntax and semantics.

A particularly important flag for users familiar with SQL Server is [MSSQL](/kb/en/sql_modemssql/).

sql_mode can be changed locally, in which case it only affects the current session; or globally, in which case it will affect all new connections (but not the connections already established). sql_mode must be assigned a comma-separated list of flags.

A usage example:

```sql
# check the current global and local sql_mode values
SELECT @@global.sql_mode;
SELECT @@session.sql_mode;
# empty sql_mode for all usaers
SET GLOBAL sql_mode = '';
# add MSSQL flag to the sql_mode for the current session
SET SESSION sql_mode = CONCAT(sql_mode, ',MSSQL');
```

[old_mode](/mariadb-administration/variables-and-modes/old-mode/) is very similar to sql_mode, but its purpose is to provide compatibility with older MariaDB versions. Its flags shouldn't affect compatibility with SQL Server (though it is theoretically possible that some of them do, as a side effect).

### Executable Comments

MariaDB supports [executable comments](/kb/en/comment-syntax/#executable-comment-syntax). These are designed to write generic queries that are only executed by MariaDB, and optionally only certain versions.

The following examples show how to insert SQL code that will be ignored by SQL Server but executed by MariaDB, or some of its versions.

- Executed by MariaDB and MySQL (see below):

```sql
SELECT * FROM tab /*! FORCE INDEX (idx_a) */ WHERE a = 1 OR b = 2;
```

- Executed by MariaDB only:

```sql
SELECT * /*M! , @in_transaction */ FROM tab;
```

- Executed by MariaDB starting from version 10.0.5:

```sql
DELETE FROM user WHERE id = 100 /*!M100005 RETURNING email */;
```

As explained in the [Understanding MariaDB Architecture](/mariadb-administration/getting-installing-and-upgrading-mariadb/migrating-from-sql-server-to-mariadb/understanding-mariadb-architecture/) page, MariaDB was initially forked from MySQL. At that time, executable comments were already supported by MySQL. This is why the `/*! ... */` syntax is supported by both MariaDB and MySQL. But because MariaDB also supports specific syntax not supported by MySQL, it added the `/*M! ... */` syntax.

## Generic Syntax

Here we discuss some differences between MariaDB and SQL Server syntax that may affect any user, as well as some hints to make queries compatible with a reasonable amount of work.

### Delimiters

SQL Server uses two different terminators:

- The <em>batch terminator</em> is the `go` command. It tells Microsoft clients to send the text we typed to SQL Server.
- The <em>query terminator</em> is a semicolon (`;`) and it tells SQL Server where a query ends.

It is rarely necessary to use `;` in SQL Server. It is required for certain common table expressions, for example.

But the same doesn't apply to MariaDB. <strong>Normally, with MariaDB you only use `;`.</strong>

However, MariaDB also has some situations where you want to use a `;` but you don't want the [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) command-line client to send the query yet. This can be done in any situation, but it is particularly useful when creating [stored routines](/programming-customizing-mariadb/stored-routines/) or using [BEGIN NOT ATOMIC](/programming-customizing-mariadb/programmatic-compound-statements/begin-end/).

The reason is better explained with an example:

```sql
CREATE PROCEDURE p()
BEGIN
    SELECT * FROM t1;
    SELECT * FROM t2;
END;
```

If we enter this procedure in this way in the `mysql` client, as soon as we type the first `;` (after the first `SELECT`) and press enter, the statement will be sent. MariaDB will try to parse it, and will return an error.

To avoid this, `mysql` implements the [DELIMITER](/clients-utilities/mysql-client/delimiters/) statement. This client statement is never sent to MariaDB. Instead, the client uses it to find out when the typed query should be sent. Let's correct the above example:

```sql
DELIMITER ||

CREATE PROCEDURE p()
BEGIN
    SELECT * FROM t1;
    SELECT * FROM t2;
END;

DELIMITER ;
```

### Names

In MariaDB, most [names](/sql-statements-structure/sql-language-structure/identifier-names/) have a maximum length of 64 characters. When migrating an SQL Server database to MariaDB, check if some names exceed this limit (SQL Server maximum length is 128).

By default, MariaDB names are case-sensitive if the operating system has case-sensitive file names (Linux), and case-insensitive if the operating system is case-insensitive (Windows). SQL Server is case-insensitive by default on all operating systems.

When migrating a SQL Server database to MariaDB on Linux, to avoid problems you may want to set the [lower_case_table_names](/kb/en/server-system-variables/#lower_case_table_names) system variable to 1, making table names, database names and aliases case-insensitive.

Names can be quoted inside backtick characters (```). This character can be used in names, in which case it should be doubled. By default this is the only way to quote names.

To also enable the use of double quotes (`"`), modify sql_mode adding the `ANSI_QUOTES` flag. This is the equivalent of setting [QUOTED_IDENTIFIER](https://docs.microsoft.com/en-us/sql/t-sql/statements/set-quoted-identifier-transact-sql?view=sql-server-ver15) ON in SQL Server.

To also enable the use of SQL Server style quotes (`[` and `]`), modify sql_mode adding the `MSSQL` flag.

The case-sensitivity of stored procedures and functions is never a problem, as they are case-insensitive in SQL Server.

### Quoting Strings

In SQL Server, by default strings can only be quoted with single-quotes (`'`), and to use a double quote in a string it should be doubled (`''`). This also works by default in MariaDB.

SQL Server also allows to use double quotes (`"`) to quote strings. This works by default in MariaDB, but as mentioned before it won't work if sql_mode contains the `ANSI_QUOTES` flag.

### NULL

The default semantics of [NULL](/columns-storage-engines-and-plugins/data-types/null-values/) in SQL Server and MariaDB is the same, by default.

However, SQL Server allows one to change it globally with [SET ANSI_NULLS OFF](https://docs.microsoft.com/en-us/sql/t-sql/statements/set-ansi-nulls-transact-sql), or at database level with [ALTER DATABASE](https://docs.microsoft.com/en-us/sql/t-sql/statements/alter-database-transact-sql).

There is no way to achieve exactly the same result in MariaDB. To perform NULL-safe comparisons in MariaDB, one should replace the [=](/sql-statements-structure/operators/comparison-operators/equal/) operator with the [&lt;=&gt;](/sql-statements-structure/operators/comparison-operators/null-safe-equal/) operator.

## Data Definition Language

Here we discuss some DDL differences that database administrators will want to be aware of.

While this section is meant to highlight the most noticeable DDL differences between MariaDB and SQL Server, there are many others, both in the syntax and in the semantics. See the [ALTER](/sql-statements-structure/sql-statements/data-definition/alter/) statement documentation.

### Altering Tables Online

Altering tables online can be a problem, especially when the tables are big and we don't want to cause a disruption.

MariaDB offers the following solutions to help:

- The [ALTER TABLE ... ALGORITHM](/kb/en/alter-table/#algorithm) clause allows one to specify which algorithm should be used to run a certain operation. For example `INPLACE` tells MariaDB not to create a table copy (perhaps because we don't have enough disk space), and `INSTANT` tells MariaDB to execute the operation instantaneously. Not all algorithms are supported for certain operations. If the algorithm we've chosen cannot be used, the [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement will fail with an error.
- The [ALTER TABLE ... LOCK](/kb/en/alter-table/#alock) clause allows one to specify which lock type should be used. For example `NONE` tells MariaDB to avoid any lock on the table, and `SHARED` only allows one to acquire a share lock. If the operation requires a lock that is more strict than the one we are requesting, the [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement will fail with an error. Sometimes this happens because the `LOCK` level we want is not available for the specified `ALGORITHM`.

To find out which operations require a table copy and which lock levels are necessary, see [InnoDB Online DDL Overview](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/innodb-online-ddl-overview/).

An `ALTER TABLE` can be queued because a long-running statement (even a `SELECT`) required a [metadata lock](/sql-statements-structure/sql-statements/transactions/metadata-locking/). Since this may cause troubles, sometimes we want the operation to simply fail if the wait is too long. This can be achieved with the [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait/) clauses.

SQL Server `WITH ONLINE = ON` is equivalent to MariaDB `LOCK = NONE`. However, note that [most ALTER TABLE statements](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/innodb-online-ddl-operations-with-the-instant-alter-algorithm/) support `ALGORITHM = INSTANT`, which is non-blocking and much faster (almost instantaneous, as the syntax suggests).

### IF EXISTS, IF NOT EXISTS, OR REPLACE

Most DDL statements, including [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/), support the following syntax:

- `DROP IF EXISTS`: A warning (not an error) is produced if the object does not exist.
- `OR REPLACE`: If the object exists, it is dropped and recreated; otherwise it is created. This operation is atomic, so at no point in time does the object not exist.
- `CREATE IF NOT EXISTS`: If the object already exists, a warning (not an error) is produced. The object will not be replaced.

These statements are functionally similar (but less verbose) than SQL Server snippets similar to the following:

```sql
IF NOT EXISTS (
        SELECT name
            FROM sysobjects
            WHERE name = 'my_table' AND xtype = 'U'
    )
    CREATE TABLE my_table (
        ...
    )
go
```

### Altering Columns

With SQL Server, the only syntax to alter a table column is `ALTER TABLE ... ALTER COLUMN`. MariaDB provides more [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) commands to obtain the same result:

- [CHANGE COLUMN](/kb/en/alter-table/#change-column) allows one to perform any change by specifying a new column definition, including the name.
- [MODIFY COLUMN](/kb/en/alter-table/#modify-column) allows any change, except renaming the column. This is a slightly simpler syntax that we can use when we don't want to change a column name.
- [ALTER COLUMN](/kb/en/alter-table/#alter-column) allows one to change or drop the `DEFAULT` value.
- [RENAME COLUMN](/kb/en/alter-table/#rename-column) allows one to only change the column name.

Using a more specific syntax is less error-prone. For example, by using `ALTER TABLE ... ALTER COLUMN` we will not accidentally change the data type.

The word `COLUMN` is usually optional, except in the case of `RENAME COLUMN`.

### SHOW Statements

MariaDB supports [SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/) statements to quickly list all objects of a certain type (tables, views, triggers...). Most `SHOW` statements support a `LIKE` clause to filter data. For example, to list the tables in the current database whose name begins with 'wp_':

```sql
SHOW TABLES LIKE 'wp\_%';
```

This is the equivalent of this query, which would work on both MariaDB and SQL Server:

```sql
SELECT TABLE_SCHEMA, TABLE_NAME
    FROM INFORMATION_SCHEMA.TABLES
    WHERE TABLE_NAME LIKE 'wp\_';
```

### SHOW CREATE Statements

In general, for each `CREATE` statement MariaDB also supports a `SHOW CREATE` statement. For example there is a [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) that returns the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement that can be used to recreate a table.

Though SQL Server has no way to show the DDL statement to recreate an object, `SHOW CREATE` statements are functionally similar to `sp_helptext()`.

### Database Comments

MariaDB does not support extended properties. Instead, it supports a `COMMENT` clause for most [CREATE](/sql-statements-structure/sql-statements/data-definition/create/) and [ALTER](/sql-statements-structure/sql-statements/data-definition/alter/) statements.

For example, to create and then change a table comment:

```sql
CREATE TABLE counter (
    c INT UNSIGNED AUTO_INCREMENT PRIMARY KEY
)
    COMMENT 'Monotonic counter'
;
ALTER TABLE counter COMMENT
    'Counter. It can contain many values, we only care about the max';
```

Comments can be seen with `SHOW CREATE` statements, or by querying `information_schema` tables. For example:

```sql
SELECT TABLE_COMMENT
    FROM information_schema.TABLES
    WHERE TABLE_SCHEMA = 'test' AND TABLE_NAME = 'counter';
+-----------------------------------------------------------------+
| TABLE_COMMENT                                                   |
+-----------------------------------------------------------------+
| Counter. It can contain many values, we only care about the max |
+-----------------------------------------------------------------+

```

## Administration

Administration and maintenance commands in MariaDB use different syntax to SQL Server.

- [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/) rebuilds table data and indexes. It can be considered as the MariaDB equivalent of SQL Server's `ALTER INDEX REBUILD`. See [Defragmenting InnoDB Tablespaces](/replication/optimization-and-tuning/optimizing-tables/defragmenting-innodb-tablespaces/) for more information. This statement is always locking. It supports [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait/) syntax,
- MariaDB has an [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) command, which is an equivalent of `UPDATE STATISTICS`.

## BULK INSERT

MariaDB has no `BULK INSERT` statement. Instead, it supports:

- [LOAD DATA INFILE](/kb/en/load-data-infile/) to load data from files in CSV or similar formats;
- [LOAD XML INFILE](/kb/en/load-xml/) to load data from XML files.

## See Also

- [SQL Server and MariaDB Types Comparison](/mariadb-administration/getting-installing-and-upgrading-mariadb/migrating-from-sql-server-to-mariadb/sql-server-and-mariadb-types-comparison/)
- [MariaDB Transactions and Isolation Levels for SQL Server Users](/mariadb-administration/getting-installing-and-upgrading-mariadb/migrating-from-sql-server-to-mariadb/mariadb-transactions-and-isolation-levels-for-sql-server-users/)
- [SQL_MODE=MSSQL](/kb/en/sql_modemssql/)