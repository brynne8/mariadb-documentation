# SHOW PROCEDURE STATUS

## Syntax

```sql
SHOW PROCEDURE STATUS
    [LIKE 'pattern' | WHERE expr]
```

## Description

This statement is a MariaDB extension. It returns characteristics of a stored
procedure, such as the database, name, type, creator, creation and modification
dates, and character set information. A similar statement, 
 <code class="highlight fixed" style="white-space:pre-wrap">[SHOW FUNCTION STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-function-status)</code>, displays
information about stored functions.

The <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clause, if present, indicates which procedure or
function names to match. The <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> and <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clauses can be given to select rows using more general conditions, as discussed in [Extended SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show).

The [ROUTINES table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table) in the INFORMATION_SCHEMA database contains more detailed information.

## Examples

```sql
SHOW PROCEDURE STATUS LIKE 'p1'\G
*************************** 1. row ***************************
                  Db: test
                Name: p1
                Type: PROCEDURE
             Definer: root@localhost
            Modified: 2010-08-23 13:23:03
             Created: 2010-08-23 13:23:03
       Security_type: DEFINER
             Comment: 
character_set_client: latin1
collation_connection: latin1_swedish_ci
  Database Collation: latin1_swedish_ci
```

## See Also

- [Stored Procedure Overview](/programming-customizing-mariadb/stored-routines/stored-procedures/stored-procedure-overview)
- [CREATE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure)
- [ALTER PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/alter-procedure)
- [DROP PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/drop-procedure)
- [SHOW CREATE PROCEDURE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-procedure)
- [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges)
- [Information Schema ROUTINES Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table)