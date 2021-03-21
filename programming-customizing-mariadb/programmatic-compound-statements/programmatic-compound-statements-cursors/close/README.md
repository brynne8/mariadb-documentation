# CLOSE

## Syntax

```sql
CLOSE cursor_name
```

## Description

This statement closes a previously [opened](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/open) cursor. The cursor must have been previously opened or else an error occurs.

If not closed explicitly, a cursor is closed at the end of the
compound statement in which it was declared.

See [Cursor Overview](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/cursor-overview) for an example.

## See Also

- [Cursor Overview](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/cursor-overview)
- [DECLARE CURSOR](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/declare-cursor)
- [OPEN cursor_name](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/open)
- [FETCH cursor_name](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/fetch)
- [Cursors in Oracle mode](/kb/en/sql_modeoracle-from-mariadb-103/#cursors)