# CALL

## Syntax

```sql
CALL sp_name([parameter[,...]])
CALL sp_name[()]
```

## Description

The <code class="highlight fixed" style="white-space:pre-wrap">CALL</code> statement invokes a [stored procedure](/programming-customizing-mariadb/stored-routines/stored-procedures/) that was
defined previously with [CREATE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure/).

Stored procedure names can be specified as `database_name.procedure_name`. Procedure names and database names can be quoted with backticks (). This is necessary if they are reserved words, or contain special characters. See [identifier qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers/) for details.

`CALL p()` and `CALL p` are equivalent.

If parentheses are used, any number of spaces, tab characters and newline characters are allowed between the procedure's name and the open parenthesis.

<code class="highlight fixed" style="white-space:pre-wrap">CALL</code> can pass back values to its caller using parameters
that are declared as <code class="highlight fixed" style="white-space:pre-wrap">OUT</code> or <code class="highlight fixed" style="white-space:pre-wrap">INOUT</code>
parameters. If no value is assigned to an `OUT` parameter, `NULL` is assigned (and its former value is lost). To pass such values from another stored program you can use [user-defined variables](/sql-statements-structure/sql-language-structure/user-defined-variables/), [local variables](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable/) or routine's parameters; in other contexts, you can only use user-defined variables.

`CALL` can also be executed as a prepared statement. Placeholders can be used for `IN` parameters in all versions of MariaDB; for `OUT` and `INOUT` parameters, placeholders can be used since [MariaDB 5.5](/kb/en/what-is-mariadb-55/).

When the procedure returns, a client program can also obtain the
number of rows affected for the final statement executed within the routine: At
the SQL level, call the [ROW_COUNT()](/built-in-functions/secondary-functions/information-functions/row_count/) function; from the C
API, call the <code class="highlight fixed" style="white-space:pre-wrap">mysql_affected_rows()</code> function.

If the `CLIENT_MULTI_RESULTS` API flag is set, `CALL` can return any number of resultsets and the called stored procedure can execute prepared statements. If it is not set, at most one resultset can be returned and prepared statements cannot be used within procedures.