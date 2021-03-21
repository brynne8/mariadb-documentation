# FETCH

## Syntax

```sql
FETCH cursor_name INTO var_name [, var_name] ...
```

## Description

This statement fetches the next row (if a row exists) using the
specified [open](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/open) [cursor](/kb/en/programmatic-and-compound-statements-cursors/), and advances the cursor pointer.

`var_name` can be a [local variable](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable), but <em>not</em> a [user-defined variable](/sql-statements-structure/sql-language-structure/user-defined-variables).

If no more rows are available, a No Data condition occurs with
`SQLSTATE` value `02000`. To detect this condition, you can set up a
handler for it (or for a `NOT FOUND` condition).

See [Cursor Overview](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/cursor-overview) for an example.

## See Also

- [Cursor Overview](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/cursor-overview)
- [DECLARE CURSOR](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/declare-cursor)
- [OPEN cursor_name](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/open)
- [CLOSE cursor_name](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/close)
- [Cursors in Oracle mode](/kb/en/sql_modeoracle-from-mariadb-103/#cursors)