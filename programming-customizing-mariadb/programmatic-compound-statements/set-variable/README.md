# SET Variable

## Syntax

```sql
SET var_name = expr [, var_name = expr] ...
```

## Description

The `SET` statement in [stored programs](/kb/en/stored-programs-and-views/) is an extended version of the general [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/) statement. Referenced variables may be ones declared inside a stored program, global system variables, or user-defined variables.

The `SET` statement in stored programs is implemented as part of the
pre-existing [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/) syntax. This allows an extended syntax of <code>SET a=x, 
b=y, ...</code> where different variable types (locally declared variables,
global and session server variables, user-defined variables) can be
mixed. This also allows combinations of local variables and some
options that make sense only for system variables; in that case, the
options are recognized but ignored.

`SET` can be used with both [local variables](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable/) and [user-defined variables](/sql-statements-structure/sql-language-structure/user-defined-variables/).

When setting several variables using the columns returned by a query, <a undefined>SELECT INTO</a> should be preferred.

To set many variables to the same value, the [LAST_VALUE( )](/built-in-functions/secondary-functions/information-functions/last_value/) function can be used.

Below is an example of how a user-defined variable may be set:

```sql
SET @x = 1;
```

## See Also

- [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/)
- [SET STATEMENT](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set-statement/)
- [DECLARE Variable](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable/)