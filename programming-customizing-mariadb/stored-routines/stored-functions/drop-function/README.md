# DROP FUNCTION

## Syntax

```sql
DROP FUNCTION [IF EXISTS] f_name
```

## Description

The DROP FUNCTION statement is used to drop a [stored function](/programming-customizing-mariadb/stored-routines/stored-functions) or a user-defined function (UDF). That is, the specified routine is removed from the server, along with all privileges specific to the function. You must have the `ALTER ROUTINE` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) for the routine in order to drop it. If the [automatic_sp_privileges](/kb/en/server-system-variables/#automatic_sp_privileges) server system variable is set, both the `ALTER ROUTINE` and `EXECUTE` privileges are granted automatically to the routine creator - see [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges).

#### IF EXISTS

The <code class="highlight fixed" style="white-space:pre-wrap">IF EXISTS</code> clause is a MySQL/MariaDB extension.  It
prevents an error from occurring if the function does not exist. A
`NOTE` is produced that can be viewed with [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings).

For dropping a [user-defined functions](/programming-customizing-mariadb/user-defined-functions) (UDF), see [DROP FUNCTION UDF](/programming-customizing-mariadb/user-defined-functions/drop-function-udf).

## Examples

```sql
DROP FUNCTION hello;
Query OK, 0 rows affected (0.042 sec)

DROP FUNCTION hello;
ERROR 1305 (42000): FUNCTION test.hello does not exist

DROP FUNCTION IF EXISTS hello;
Query OK, 0 rows affected, 1 warning (0.000 sec)

SHOW WARNINGS;
+-------+------+------------------------------------+
| Level | Code | Message                            |
+-------+------+------------------------------------+
| Note  | 1305 | FUNCTION test.hello does not exist |
+-------+------+------------------------------------+
```

## See Also

- [DROP PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/drop-procedure)
- [Stored Function Overview](/programming-customizing-mariadb/stored-routines/stored-functions/stored-function-overview)
- [CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function)
- [CREATE FUNCTION UDF](/programming-customizing-mariadb/user-defined-functions/create-function-udf)
- [ALTER FUNCTION](/sql-statements-structure/sql-statements/data-definition/alter/alter-function)
- [SHOW CREATE FUNCTION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-function)
- [SHOW FUNCTION STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-function-status)
- [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges)
- [INFORMATION_SCHEMA ROUTINES Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table)