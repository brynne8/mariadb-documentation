# ALTER FUNCTION

## Syntax

```sql
ALTER FUNCTION func_name [characteristic ...]

characteristic:
    { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
  | SQL SECURITY { DEFINER | INVOKER }
  | COMMENT 'string'
```

## Description

This statement can be used to change the characteristics of a stored
function. More than one change may be specified in an `ALTER FUNCTION`
statement. However, you cannot change the parameters or body of a
stored function using this statement; to make such changes, you must
drop and re-create the function using [DROP FUNCTION](/programming-customizing-mariadb/stored-routines/stored-functions/drop-function/) and [CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function/).

You must have the `ALTER ROUTINE` privilege for the function. (That
privilege is granted automatically to the function creator.) If binary
logging is enabled, the `ALTER FUNCTION` statement might also require
the `SUPER` privilege, as described in [Binary Logging of Stored Routines](/programming-customizing-mariadb/stored-routines/binary-logging-of-stored-routines/).

## Example

```sql
ALTER FUNCTION hello SQL SECURITY INVOKER;
```

## See Also

- [CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function/)
- [SHOW CREATE FUNCTION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-function/)
- [DROP FUNCTION](/programming-customizing-mariadb/stored-routines/stored-functions/drop-function/)
- [SHOW FUNCTION STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-function-status/)
- [Information Schema ROUTINES Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table/)