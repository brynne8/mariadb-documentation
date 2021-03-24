# SHOW PROCEDURE CODE

## Syntax

```sql
SHOW PROCEDURE CODE proc_name
```

## Description

This statement is a MariaDB extension that is available only for servers that
have been built with debugging support. It displays a representation of the
internal implementation of the named [stored procedure](/programming-customizing-mariadb/stored-routines/stored-procedures/). A similar statement,
 <code class="highlight fixed" style="white-space:pre-wrap">[SHOW FUNCTION CODE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-function-code/)</code>, displays
information about [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions/).

Both statements require that you be the owner of the routine or have
 <code class="highlight fixed" style="white-space:pre-wrap">[SELECT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/)</code> access to the [mysql.proc](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlproc-table/) table.

If the named routine is available, each statement produces a result
set. Each row in the result set corresponds to one "instruction" in
the routine. The first column is Pos, which is an ordinal number
beginning with 0. The second column is Instruction, which contains an
SQL statement (usually changed from the original source), or a
directive which has meaning only to the stored-routine handler.

## Examples

```sql
DELIMITER //

CREATE PROCEDURE p1 ()
  BEGIN
    DECLARE fanta INT DEFAULT 55;
    DROP TABLE t2;
    LOOP
      INSERT INTO t3 VALUES (fanta);
      END LOOP;
  END//
Query OK, 0 rows affected (0.00 sec)

SHOW PROCEDURE CODE p1//
+-----+----------------------------------------+
| Pos | Instruction                            |
+-----+----------------------------------------+
|   0 | set fanta@0 55                         |
|   1 | stmt 9 "DROP TABLE t2"                 |
|   2 | stmt 5 "INSERT INTO t3 VALUES (fanta)" |
|   3 | jump 2                                 |
+-----+----------------------------------------+
```

## See Also

- [Stored Procedure Overview](/programming-customizing-mariadb/stored-routines/stored-procedures/stored-procedure-overview/)
- [CREATE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure/)
- [ALTER PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/alter-procedure/)
- [DROP PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/drop-procedure/)
- [SHOW CREATE PROCEDURE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-procedure/)
- [SHOW PROCEDURE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-procedure-status/)
- [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges/)
- [Information Schema ROUTINES Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table/)