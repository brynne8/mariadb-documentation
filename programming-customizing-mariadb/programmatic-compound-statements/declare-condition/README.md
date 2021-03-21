# DECLARE CONDITION

## Syntax

```sql
DECLARE condition_name CONDITION FOR condition_value

condition_value:
    SQLSTATE [VALUE] sqlstate_value
  | mysql_error_code
```

## Description

The `DECLARE ... CONDITION` statement defines a named error condition.
It specifies a condition that needs specific handling and associates a
name with that condition. Later, the name can be used in a [DECLARE ... HANDLER](/programming-customizing-mariadb/programmatic-compound-statements/declare-handler), [SIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/signal) or [RESIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/resignal) statement (as long as the statement is located in the same [BEGIN ... END](/programming-customizing-mariadb/programmatic-compound-statements/begin-end) block).

Conditions must be declared after [local variables](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable), but before [CURSORs](/kb/en/programmatic-and-compound-statements-cursors/) and [HANDLERs](/programming-customizing-mariadb/programmatic-compound-statements/declare-handler).

A condition_value for `DECLARE ... CONDITION` can be an [SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate) value (a
5-character string literal) or a MySQL error code (a number). You should not
use SQLSTATE value '00000' or MySQL error code 0, because those indicate sucess
rather than an error condition. If you try, or if you specify an invalid SQLSTATE value, an error like this is produced:

```sql
ERROR 1407 (42000): Bad SQLSTATE: '00000'
```

For a list of SQLSTATE values and MariaDB error
codes, see [MariaDB Error Codes](/sql-statements-structure/sql-language-structure/mariadb-error-codes).