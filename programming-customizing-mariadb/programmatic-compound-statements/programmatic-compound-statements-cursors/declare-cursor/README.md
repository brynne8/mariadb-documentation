# DECLARE CURSOR

## Syntax

&lt;= [MariaDB 10.2](/kb/en/what-is-mariadb-102/)

```sql
DECLARE cursor_name CURSOR FOR select_statement
```

From [MariaDB 10.3](/kb/en/what-is-mariadb-103/)

```sql
DECLARE cursor_name CURSOR [(cursor_formal_parameter[,...])] FOR select_statement

cursor_formal_parameter:
    name type [collate clause]
```

## Description

This statement declares a [cursor](/kb/en/programmatic-and-compound-statements-cursors/). Multiple cursors may be declared in a [stored program](/kb/en/stored-programs-and-views/), but each cursor in a given block must have a unique name.

`select_statement` is not executed until the [OPEN](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/open/) statement is executed. It is important to remember this if the query produces an error, or calls functions which have side effects.

A `SELECT` associated to a cursor can use variables, but the query itself cannot be a variable, and cannot be dynamically composed. The `SELECT` statement cannot have an `INTO` clause.

Cursors must be declared before [HANDLERs](/programming-customizing-mariadb/programmatic-compound-statements/declare-handler/), but after local variables and [CONDITIONs](/programming-customizing-mariadb/programmatic-compound-statements/declare-condition/).

##### MariaDB starting with [10.3.0](/kb/en/mariadb-1030-release-notes/)

From [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/), cursors can have parameters. This is a non-standard SQL extension. Cursor parameters can appear in any part of the DECLARE CURSOR select_statement where a stored procedure variable is allowed (select list, WHERE, HAVING, LIMIT etc).

See [Cursor Overview](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/cursor-overview/) for an example.

## See Also

- [Cursor Overview](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/cursor-overview/)
- [OPEN cursor_name](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/open/)
- [FETCH cursor_name](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/fetch/)
- [CLOSE cursor_name](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/close/)
- [Cursors in Oracle mode](/kb/en/sql_modeoracle-from-mariadb-103/#cursors)