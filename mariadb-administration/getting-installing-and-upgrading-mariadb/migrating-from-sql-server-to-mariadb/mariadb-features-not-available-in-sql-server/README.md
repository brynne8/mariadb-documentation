# MariaDB Features Not Available in SQL Server

Some MariaDB features are not available in SQL Server.

At first glance, it is not important to know about those features to migrate from SQL Server to MariaDB. However, this is not the case. Using MariaDB features that are not in SQL Server allows one to obtain more advantages from the migration, getting the most from MariaDB.

This page has a list of MariaDB features that are not supported in SQL Server. The list is not exhaustive.

## Plugin Architecture

- [Storage engines](/columns-storage-engines-and-plugins/storage-engines).
- [Authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins).
- [Encryption plugins](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins).
- [ColumnStore](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore) is a columnar storage engine designed to scale horizontally. It runs on a specific edition of MariaDB, so currently it cannot be used in combination with other engines.

## SQL

- The [sql_mode](/mariadb-administration/variables-and-modes/sql-mode) variable determines in which cases an SQL statement should fail with an error, and in which cases it should succeed with a warning even if it is not entirely correct. For example, when a statement tries to insert a string in a column which is not big enough to contain it, it could fail, or it could insert a truncated string and emit a warning. It is a tradeoff between reliability and flexibility.
<ul start="1"><li>[SQL_MODE=MSSQL](/kb/en/sql_modemssql/) allows one to use a small subset of SQL Server proprietary syntax.
</li></ul>
- The [`CREATE ... IF EXISTS`, `CREATE OR REPLACE`, `DROP ... IF NOT EXISTS`](/kb/en/syntax-differences-between-mariadb-and-sql-server/#if-exists-if-not-exists-or-replace) options are supported for most [DDL statements](/sql-statements-structure/sql-statements/data-definition).
- [SHOW](/kb/en/syntax-differences-between-mariadb-and-sql-server/#show-statements) statements.
- [SHOW CREATE](/kb/en/syntax-differences-between-mariadb-and-sql-server/#show-create-statements) statements.
- [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist) and [PERFORMANCE_SCHEMA THREAD table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table) provide much richer information, compared to SQL Server `sp_who()` and `sp_who2()` procedures.
- [CHECKSUM TABLE](/sql-statements-structure/sql-statements/table-statements/checksum-table) statement.
- [PL/SQL support](/kb/en/sql_modeoracle-from-mariadb-103/) (only for stored procedures and stored functions).
- Row constructors.
- `BEFORE` [triggers](/programming-customizing-mariadb/triggers-events/triggers).
- [HANDLER](/sql-statements-structure/nosql/handler/handler-commands) statements, to scroll table rows ordered by an index or in their physical order.
- [DO](/sql-statements-structure/sql-statements/stored-routine-statements/do) statement, to call functions without returning a result set.
- [BENCHMARK()](/built-in-functions/secondary-functions/information-functions/benchmark) function, to measure the speed of an SQL expression.

See also [Syntax Differences between MariaDB and SQL Server](/mariadb-administration/getting-installing-and-upgrading-mariadb/migrating-from-sql-server-to-mariadb/syntax-differences-between-mariadb-and-sql-server).

## Types

- [Character sets and collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets) don't depend on column type. They can be set globally, or at database, table or column level.
- Columns may use non-constant expressions as the [DEFAULT](/kb/en/create-table/#default-column-option) value. [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp) columns may have a `DEFAULT` value.
- [UNSIGNED](/kb/en/numeric-data-type-overview/#signed-unsigned-and-zerofill) numeric types.
- [Dynamic columns](/sql-statements-structure/nosql/dynamic-columns) (note that JSON is usually preferred to this feature).

See also [SQL Server and MariaDB Types Comparison](/mariadb-administration/getting-installing-and-upgrading-mariadb/migrating-from-sql-server-to-mariadb/sql-server-and-mariadb-types-comparison).

### JSON

For compatibility with some other database systems, MariaDB supports the [JSON](/columns-storage-engines-and-plugins/data-types/string-data-types/json-data-type) pseudo-type. However, it is just an alias for:

`LONGTEXT CHECK (JSON_VALID(column_name))`

[JSON_VALID()](/built-in-functions/special-functions/json-functions/json_valid) is the MariaDB equivalent of SQL Server's `ISJSON()`.

## Features

- [Flashback](/mariadb-administration/server-monitoring-logs/binary-log/flashback) functionality allows one to "undo" the changes that happened after a certain point in time.
- [Partitioned tables](/mariadb-administration/partitioning-tables) support the following features:
<ul start="1"><li>Tables can be partitioned based on [multiple columns](/mariadb-administration/partitioning-tables/partitioning-types/range-columns-and-list-columns-partitioning-types).
</li><li>Several [partitioning types](/kb/en/partitioning-overview/#partitioning-types) are available.
</li><li>Subpartitions.
</li></ul>
- [Progress reporting](/kb/en/progress-reporting/) for some typically expensive statements.