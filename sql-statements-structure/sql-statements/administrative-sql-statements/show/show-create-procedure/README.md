# SHOW CREATE PROCEDURE

## Syntax

```sql
SHOW CREATE PROCEDURE proc_name
```

## Description

This statement is a MariaDB extension. It returns the exact string that
can be used to re-create the named [stored procedure](/programming-customizing-mariadb/stored-routines/stored-procedures/), as well as the [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) that was used when the trigger has been created and the character set used by the connection.. A similar
statement, <code class="highlight fixed" style="white-space:pre-wrap">[SHOW CREATE FUNCTION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-function/)</code>,
displays information about [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions/).

Both statements require that you are the owner of the routine or have the [SELECT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) privilege on the <a undefined>mysql.proc</a> table.  When neither is true, the statements display `NULL` for the `Create Procedure` or `Create Function` field.

<strong>Warning</strong> Users with `SELECT` privileges on <a undefined>mysql.proc</a> or `USAGE` privileges on `*.*` can view the text of routines, even when they do not have privileges for the function or procedure itself.

The output of these statements is unreliably affected by the <a undefined>sql_quote_show_create</a> server system variable - see [http://bugs.mysql.com/bug.php?id=12719](http://bugs.mysql.com/bug.php?id=12719)

## Examples

Here's a comparison of the `SHOW CREATE PROCEDURE` and [SHOW CREATE FUNCTION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-function/) statements.

```sql
SHOW CREATE PROCEDURE test.simpleproc\G
*************************** 1. row ***************************
           Procedure: simpleproc
            sql_mode: 
    Create Procedure: CREATE PROCEDURE `simpleproc`(OUT param1 INT)
                      BEGIN
                      SELECT COUNT(*) INTO param1 FROM t;
                      END
character_set_client: latin1
collation_connection: latin1_swedish_ci
  Database Collation: latin1_swedish_ci

SHOW CREATE FUNCTION test.hello\G
*************************** 1. row ***************************
            Function: hello
            sql_mode:
     Create Function: CREATE FUNCTION `hello`(s CHAR(20))
                      RETURNS CHAR(50)
                      RETURN CONCAT('Hello, ',s,'!')
character_set_client: latin1
collation_connection: latin1_swedish_ci
  Database Collation: latin1_swedish_ci
```

When the user issuing the statement does not have privileges on the routine, attempting to [CALL](/sql-statements-structure/sql-statements/stored-routine-statements/call/) the procedure raises Error 1370.

```sql
CALL test.prc1();
Error 1370 (42000): execute command denieed to user 'test_user'@'localhost' for routine 'test'.'prc1'
```

If the user neither has privilege to the routine nor the [SELECT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) privilege on <a undefined>mysql.proc</a> table, it raises Error 1305, informing them that the procedure does not exist.

```sql
SHOW CREATE TABLES test.prc1\G
Error 1305 (42000): PROCEDURE prc1 does not exist
```

## See Also

- [Stored Procedure Overview](/programming-customizing-mariadb/stored-routines/stored-procedures/stored-procedure-overview/)
- [CREATE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure/)
- [ALTER PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/alter-procedure/)
- [DROP PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/drop-procedure/)
- [SHOW PROCEDURE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-procedure-status/)
- [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges/)
- [Information Schema ROUTINES Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table/)