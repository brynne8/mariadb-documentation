# IF

## Syntax

```sql
IF search_condition THEN statement_list
    [ELSEIF search_condition THEN statement_list] ...
    [ELSE statement_list]
END IF;
```

## Description

`IF` implements a basic conditional construct. If the `search_condition`
evaluates to true, the corresponding SQL statement list is executed.
If no `search_condition` matches, the statement list in the `ELSE` clause
is executed. Each statement_list consists of one or more statements.

## See Also

- The [IF() function](/built-in-functions/control-flow-functions/if-function/), which differs from the `IF` statement described above.
- [Changes in Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#simple-syntax-compatibility)