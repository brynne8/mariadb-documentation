# CREATE PROCEDURE

## Syntax

```sql
CREATE
    [OR REPLACE]
    [DEFINER = { user | CURRENT_USER | role | CURRENT_ROLE }]
    PROCEDURE sp_name ([proc_parameter[,...]])
    [characteristic ...] routine_body

proc_parameter:
    [ IN | OUT | INOUT ] param_name type

type:
    Any valid MariaDB data type

characteristic:
    LANGUAGE SQL
  | [NOT] DETERMINISTIC
  | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
  | SQL SECURITY { DEFINER | INVOKER }
  | COMMENT 'string'

routine_body:
    Valid SQL procedure statement
```

## Description

Creates a [stored procedure](/programming-customizing-mariadb/stored-routines/stored-procedures). By default, a routine is
associated with the default database. To associate the routine
explicitly with a given database, specify the name as db_name.sp_name
when you create it.

When the routine is invoked, an implicit USE db_name is performed (and
undone when the routine terminates). The causes the routine to have
the given default database while it executes. USE statements within
stored routines are disallowed.

When a stored procedure has been created, you invoke it by
using the `CALL` statement (see [CALL](/sql-statements-structure/sql-statements/stored-routine-statements/call)).

To execute the `CREATE PROCEDURE` statement, it is
necessary to have the `CREATE ROUTINE` privilege. By default, MariaDB
automatically grants the `ALTER ROUTINE` and `EXECUTE` privileges to the
routine creator. See also [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges).

The `DEFINER` and SQL SECURITY clauses specify the security context to
be used when checking access privileges at routine execution time, as
described later. Requires the [SUPER](/kb/en/grant/#super) privilege, or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [SET USER](/kb/en/grant/#set-user) privilege.

If the routine name is the same as the name of a built-in SQL
function, you must use a space between the name and the following
parenthesis when defining the routine, or a syntax error occurs. This
is also true when you invoke the routine later. For this reason, we
suggest that it is better to avoid re-using the names of existing SQL
functions for your own stored routines.

The IGNORE_SPACE SQL mode applies to built-in functions, not to stored
routines. It is always allowable to have spaces after a routine name,
regardless of whether IGNORE_SPACE is enabled.

The parameter list enclosed within parentheses must always be present.
If there are no parameters, an empty parameter list of () should be
used. Parameter names are not case sensitive.

Each parameter can be declared to use any valid data type, except that
the COLLATE attribute cannot be used.

For valid identifiers to use as procedure names, see [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names).

### IN/OUT/INOUT

Each parameter is an `IN` parameter by default. To specify otherwise for
a parameter, use the keyword `OUT` or `INOUT` before the parameter name.

An `IN` parameter passes a value into a procedure. The procedure might
modify the value, but the modification is not visible to the caller
when the procedure returns. An `OUT` parameter passes a value from the
procedure back to the caller. Its initial value is NULL within the
procedure, and its value is visible to the caller when the procedure
returns. An `INOUT` parameter is initialized by the caller, can be
modified by the procedure, and any change made by the procedure is
visible to the caller when the procedure returns.

For each `OUT` or `INOUT` parameter, pass a user-defined variable in the
`CALL` statement that invokes the procedure so that you can obtain its
value when the procedure returns. If you are calling the procedure
from within another stored procedure or function, you can also pass a
routine parameter or local routine variable as an `IN` or `INOUT`
parameter.

### DETERMINISTIC/NOT DETERMINISTIC

`DETERMINISTIC` and `NOT DETERMINISTIC` apply only to [functions](/programming-customizing-mariadb/stored-routines/stored-functions). Specifying `DETERMINISTC` or `NON-DETERMINISTIC` in procedures has no effect. The default value is `NOT DETERMINISTIC`. Functions are `DETERMINISTIC` when they always return the same value for the same input. For example, a truncate or substring function. Any function involving data, therefore, is always `NOT DETERMINISTIC`.

### CONTAINS SQL/NO SQL/READS SQL DATA/MODIFIES SQL DATA

`CONTAINS SQL`, `NO SQL`, `READS SQL DATA`, and `MODIFIES SQL DATA` are informative clauses that tell the server what the function does. MariaDB does not check in any way whether the specified clause is correct. If none of these clauses are specified, `CONTAINS SQL` is used by default.

`MODIFIES SQL DATA` means that the function contains statements that may modify data stored in databases. This happens if the function contains statements like [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update), [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert), [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace) or DDL.

`READS SQL DATA` means that the function reads data stored in databases, but does not modify any data. This happens if [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) statements are used, but there no write operations are executed.

`CONTAINS SQL` means that the function contains at least one SQL statement, but it does not read or write any data stored in a database. Examples include [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set) or [DO](/sql-statements-structure/sql-statements/stored-routine-statements/do).

`NO SQL` means nothing, because MariaDB does not currently support any language other than SQL.

The routine_body consists of a valid SQL procedure statement. This can
be a simple statement such as [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) or [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert), or it can be a
compound statement written using [BEGIN and END](/programming-customizing-mariadb/programmatic-compound-statements/begin-end). Compound statements
can contain declarations, loops, and other control structure
statements. See [Programmatic and Compound Statements](/kb/en/programmatic-and-compound-statements/) for syntax details.

MariaDB allows routines to contain DDL statements, such as `CREATE` and
DROP. MariaDB also allows [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures) (but not [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions))
to contain SQL transaction statements such as `COMMIT`.

For additional information about statements that are not allowed in
stored routines, see [Stored Routine Limitations](/programming-customizing-mariadb/stored-routines/stored-routine-limitations).

### Invoking stored procedure from within programs

For information about invoking [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures) from within programs written in a language that has a MariaDB/MySQL interface, see [CALL](/sql-statements-structure/sql-statements/stored-routine-statements/call).

### OR REPLACE

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

If the optional `OR REPLACE` clause is used, it acts as a shortcut for:

```sql
DROP PROCEDURE IF EXISTS name;
CREATE PROCEDURE name ...;
```

with the exception that any existing [privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges) for the procedure are not dropped.

### sql_mode

MariaDB stores the [sql_mode](/kb/en/server-system-variables/#sql_mode) system variable setting that is in effect at the time a routine is created, and always executes the routine with this setting in force, regardless of the server [SQL mode](/kb/en/sql_mode/) in effect when the routine is invoked.

### Character Sets and Collations

Procedure parameters can be declared with any character set/collation. If the character set and collation are not specifically set, the database defaults at the time of creation will be used. If the database defaults change at a later stage, the stored procedure character set/collation will not be changed at the same time; the stored procedure needs to be dropped and recreated to ensure the same character set/collation as the database is used.

### Oracle Mode

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

From [MariaDB 10.3](/kb/en/what-is-mariadb-103/), a subset of Oracle's PL/SQL language has been supported in addition to the traditional SQL/PSM-based MariaDB syntax. See [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#stored-procedures-and-stored-functions) for details on changes when running Oracle mode.

## Examples

The following example shows a simple stored procedure that uses an `OUT`
parameter. It uses the DELIMITER command to set a new delimiter for the duration of the process <span>—</span> see [Delimiters in the mysql client](/kb/en/delimiters-in-the-mysql-client/).

```sql
DELIMITER //

CREATE PROCEDURE simpleproc (OUT param1 INT)
 BEGIN
  SELECT COUNT(*) INTO param1 FROM t;
 END;
//

DELIMITER ;

CALL simpleproc(@a);

SELECT @a;
+------+
| @a   |
+------+
|    1 |
+------+
```

Character set and collation:

```sql
DELIMITER //

CREATE PROCEDURE simpleproc2 (
  OUT param1 CHAR(10) CHARACTER SET 'utf8' COLLATE 'utf8_bin'
)
 BEGIN
  SELECT CONCAT('a'),f1 INTO param1 FROM t;
 END;
//

DELIMITER ;
```

CREATE OR REPLACE:

```sql
DELIMITER //

CREATE PROCEDURE simpleproc2 (
  OUT param1 CHAR(10) CHARACTER SET 'utf8' COLLATE 'utf8_bin'
)
 BEGIN
  SELECT CONCAT('a'),f1 INTO param1 FROM t;
 END;
//
ERROR 1304 (42000): PROCEDURE simpleproc2 already exists

DELIMITER ;

DELIMITER //

CREATE OR REPLACE PROCEDURE simpleproc2 (
  OUT param1 CHAR(10) CHARACTER SET 'utf8' COLLATE 'utf8_bin'
)
 BEGIN
  SELECT CONCAT('a'),f1 INTO param1 FROM t;
 END;
//
ERROR 1304 (42000): PROCEDURE simpleproc2 already exists

DELIMITER ;
Query OK, 0 rows affected (0.03 sec)
```

## See Also

- [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names)
- [Stored Procedure Overview](/programming-customizing-mariadb/stored-routines/stored-procedures/stored-procedure-overview)
- [ALTER PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/alter-procedure)
- [DROP PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/drop-procedure)
- [SHOW CREATE PROCEDURE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-procedure)
- [SHOW PROCEDURE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-procedure-status)
- [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges)
- [Information Schema ROUTINES Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table)