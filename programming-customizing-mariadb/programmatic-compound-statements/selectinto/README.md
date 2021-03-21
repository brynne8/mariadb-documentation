# SELECT INTO

## Syntax

```sql
SELECT col_name [, col_name] ...
    INTO var_name [, var_name] ...
    table_expr
```

## Description

SELECT ... INTO enables selected columns to be stored directly
into variables. No resultset is produced. The query should return a single row. If the query
returns no rows, a warning with error code 1329 occurs (No data), and
the variable values remain unchanged. If the query returns multiple
rows, error 1172 occurs (Result consisted of more than one row). If it
is possible that the statement may retrieve multiple rows, you can use
`LIMIT 1` to limit the result set to a single row.

The INTO clause can also be specified at the end of the statement.

In the context of such statements that occur as part of events
executed by the Event Scheduler, diagnostics messages (not only
errors, but also warnings) are written to the error log, and, on
Windows, to the application event log.

This statement can be used with both [local variables](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable) and [user-defined variables](/sql-statements-structure/sql-language-structure/user-defined-variables).

For the complete syntax, see [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select).

Another way to set a variable's value is the [SET](/programming-customizing-mariadb/programmatic-compound-statements/set-variable) statement.

`SELECT ... INTO` results are not stored in the [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache) even if `SQL_CACHE` is specified.

## Examples

```sql
SELECT id, data INTO @x,@y 
FROM test.t1 LIMIT 1;
```

## See Also

- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) - full SELECT syntax.
- [SELECT INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile) - formatting and writing the result to an external file.
- [SELECT INTO DUMPFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-dumpfile) - binary-safe writing of the unformatted results to an external file.