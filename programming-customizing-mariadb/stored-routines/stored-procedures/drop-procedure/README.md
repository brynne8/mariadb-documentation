# DROP PROCEDURE

## Syntax

```sql
DROP PROCEDURE [IF EXISTS] sp_name
```

## Description

This statement is used to drop a [stored procedure](/programming-customizing-mariadb/stored-routines/stored-procedures/). That is, the
specified routine is removed from the server along with all privileges specific to the [procedure](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/). You must have the `ALTER ROUTINE` privilege for the routine. If the <a undefined>automatic_sp_privileges</a> server system variable is set, that privilege and `EXECUTE` are granted automatically to the routine creator - see [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges/).

The <code class="highlight fixed" style="white-space:pre-wrap">IF EXISTS</code> clause is a MySQL/MariaDB extension.  It
prevents an error from occurring if the procedure or function does not exist. A
`NOTE` is produced that can be viewed with [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/).

While this statement takes effect immediately, threads which are executing a procedure can continue execution.

## Examples

```sql
DROP PROCEDURE simpleproc;
```

IF EXISTS:

```sql
DROP PROCEDURE simpleproc;
ERROR 1305 (42000): PROCEDURE test.simpleproc does not exist

DROP PROCEDURE IF EXISTS simpleproc;
Query OK, 0 rows affected, 1 warning (0.00 sec)

SHOW WARNINGS;
+-------+------+------------------------------------------+
| Level | Code | Message                                  |
+-------+------+------------------------------------------+
| Note  | 1305 | PROCEDURE test.simpleproc does not exist |
+-------+------+------------------------------------------+
```

## See Also

- [DROP FUNCTION](/programming-customizing-mariadb/stored-routines/stored-functions/drop-function/)
- [Stored Procedure Overview](/programming-customizing-mariadb/stored-routines/stored-procedures/stored-procedure-overview/)
- [CREATE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure/)
- [ALTER PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/drop-procedure/)
- [SHOW CREATE PROCEDURE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-procedure/)
- [SHOW PROCEDURE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-procedure-status/)
- [Information Schema ROUTINES Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table/)