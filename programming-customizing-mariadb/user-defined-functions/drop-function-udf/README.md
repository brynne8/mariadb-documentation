# DROP FUNCTION UDF

## Syntax

```sql
DROP FUNCTION [IF EXISTS] function_name
```

## Description

This statement drops the [user-defined function](/programming-customizing-mariadb/user-defined-functions) (UDF) named <code class="highlight fixed" style="white-space:pre-wrap">function_name</code>.

To drop a function, you must have the <code class="highlight fixed" style="white-space:pre-wrap">[DELETE privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant)</code> for the mysql database. This is because <code class="highlight fixed" style="white-space:pre-wrap">DROP FUNCTION</code> removes the row from the [mysql.func](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlfunc-table) system table that records the function's name, type and shared library name.

For dropping a stored function, see [DROP FUNCTION](/programming-customizing-mariadb/stored-routines/stored-functions/drop-function).

### Upgrading a UDF

To upgrade the UDF's shared library, first run a [DROP FUNCTION](/programming-customizing-mariadb/stored-routines/stored-functions/drop-function) statement, then upgrade the shared library and finally run the CREATE FUNCTION statement. If you upgrade without following this process, you may crash the server.

## Examples

```sql
DROP FUNCTION jsoncontains_path;
```

IF EXISTS:

```sql
DROP FUNCTION jsoncontains_path;
ERROR 1305 (42000): FUNCTION test.jsoncontains_path does not exist

DROP FUNCTION IF EXISTS jsoncontains_path;
Query OK, 0 rows affected, 1 warning (0.00 sec)

SHOW WARNINGS;
+-------+------+------------------------------------------------+
| Level | Code | Message                                        |
+-------+------+------------------------------------------------+
| Note  | 1305 | FUNCTION test.jsoncontains_path does not exist |
+-------+------+------------------------------------------------+
```