# ALTER PROCEDURE

## Syntax

```sql
ALTER PROCEDURE proc_name [characteristic ...]

characteristic:
    { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
  | SQL SECURITY { DEFINER | INVOKER }
  | COMMENT 'string'
```

## Description

This statement can be used to change the characteristics of a [stored
procedure](/programming-customizing-mariadb/stored-routines/stored-procedures). More than one change may be specified in an `ALTER PROCEDURE`
statement. However, you cannot change the parameters or body of a
stored procedure using this statement. To make such changes, you must
drop and re-create the procedure using either [CREATE OR REPLACE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure) (since [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)) or [DROP PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/drop-procedure) and [CREATE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure) ([MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/) and before).

You must have the `ALTER ROUTINE` privilege for the procedure. By default, that privilege is granted automatically to the procedure creator. See [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges).

## Example

```sql
ALTER PROCEDURE simpleproc SQL SECURITY INVOKER;
```

## See Also

- [Stored Procedure Overview](/programming-customizing-mariadb/stored-routines/stored-procedures/stored-procedure-overview)
- [CREATE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure)
- [SHOW CREATE PROCEDURE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-procedure)
- [DROP PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/drop-procedure)
- [SHOW CREATE PROCEDURE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-procedure)
- [SHOW PROCEDURE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-procedure-status)
- [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges)
- [Information Schema ROUTINES Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table)