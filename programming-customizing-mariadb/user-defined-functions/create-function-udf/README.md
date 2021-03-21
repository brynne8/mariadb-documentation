# CREATE FUNCTION UDF

## Syntax

```sql
CREATE [OR REPLACE] [AGGREGATE] FUNCTION [IF NOT EXISTS] function_name
    RETURNS {STRING|INTEGER|REAL|DECIMAL}
    SONAME shared_library_name
```

## Description

A user-defined function (UDF) is a way to extend MariaDB with a new function
that works like a native (built-in) MariaDB function such as [ABS()](/built-in-functions/numeric-functions/abs/) or
[CONCAT()](/built-in-functions/string-functions/concat/).

`function_name` is the name that should be used in SQL statements to invoke
the function.

To create a function, you must have the [INSERT privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for the
mysql database. This is necessary because `CREATE FUNCTION` adds a row to the
[mysql.func system table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlfunc-table/) that records the function's name,
type, and shared library name. If you do not have this table, you should run
the [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade/) command to create it.

UDFs need to be written in C, C++ or another language that uses C calling
conventions, MariaDB needs to have been dynamically compiled, and your
operating system must support dynamic loading.

For an example, see `sql/udf_example.cc` in the source tree. For a collection of existing UDFs see [http://www.mysqludf.org/](http://www.mysqludf.org/).

Statements making use of user-defined functions are not
[safe for replication](/kb/en/unsafe-statements-for-replication/).

For creating a stored function as opposed to a user-defined function, see
[CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function/).

For valid identifiers to use as function names, see [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names/).

#### RETURNS

The `RETURNS` clause indicates the type of the function's
return value, and can be one of [STRING](string), [INTEGER](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/integer/), [REAL](real) or [DECIMAL](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/decimal/). `DECIMAL` functions currently return string values and should be written like [STRING](/columns-storage-engines-and-plugins/data-types/string-data-types/) functions.

#### shared_library_name

`shared_library_name` is the basename of the shared object file that contains
the code that implements the function. The file must be located in the plugin
directory. This directory is given by the value of the
[plugin_dir](/kb/en/server-system-variables/#plugin_dir) system variable. Note that
before MariaDB/MySQL 5.1, the shared object could be located in any directory
that was searched by your system's dynamic linker.

#### AGGREGATE

Aggregate functions are summary functions such as [SUM()](/built-in-functions/aggregate-functions/sum/) and
[AVG()](/built-in-functions/aggregate-functions/avg/).

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

Aggregate UDF functions can be used as [window functions](/built-in-functions/special-functions/window-functions/).

#### OR REPLACE

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

The `OR REPLACE` clause was added in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

If the optional `OR REPLACE` clause is used, it acts as a shortcut for:

```sql
DROP FUNCTION IF EXISTS function_name;
CREATE FUNCTION name ...;
```

#### IF NOT EXISTS

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

The `IF NOT EXISTS` clause was added in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

When the IF NOT EXISTS clause is used, MariaDB will return a warning instead of an error if the specified function already exists. Cannot be used together with OR REPLACE.

### Upgrading a UDF

To upgrade the UDF's shared library, first run a
[DROP FUNCTION](/programming-customizing-mariadb/stored-routines/stored-functions/drop-function/) statement, then upgrade the shared library and
finally run the CREATE FUNCTION statement. If you upgrade without following
this process, you may crash the server.

### Examples

```sql
CREATE FUNCTION jsoncontains_path RETURNS integer SONAME 'ha_connect.so';
Query OK, 0 rows affected (0.00 sec)
```

OR REPLACE and IF NOT EXISTS:

```sql
CREATE FUNCTION jsoncontains_path RETURNS integer SONAME 'ha_connect.so';
ERROR 1125 (HY000): Function 'jsoncontains_path' already exists

CREATE OR REPLACE FUNCTION jsoncontains_path RETURNS integer SONAME 'ha_connect.so';
Query OK, 0 rows affected (0.00 sec)

CREATE FUNCTION IF NOT EXISTS jsoncontains_path RETURNS integer SONAME 'ha_connect.so';
Query OK, 0 rows affected, 1 warning (0.00 sec)

SHOW WARNINGS;
+-------+------+---------------------------------------------+
| Level | Code | Message                                     |
+-------+------+---------------------------------------------+
| Note  | 1125 | Function 'jsoncontains_path' already exists |
+-------+------+---------------------------------------------+
```

## See Also

- [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names/)
- [DROP FUNCTION](/programming-customizing-mariadb/stored-routines/stored-functions/drop-function/)
- [CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function/)