# OPEN

## Syntax

&lt;= [MariaDB 10.2](/kb/en/what-is-mariadb-102/)

```sql
OPEN cursor_name
```

From [MariaDB 10.3](/kb/en/what-is-mariadb-103/)

```sql
OPEN cursor_name [expression[,...]];
```

## Description

This statement opens a [cursor](/kb/en/programmatic-and-compound-statements-cursors/) which was previously declared with [DECLARE CURSOR](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/declare-cursor).

The query associated to the DECLARE CURSOR is executed when OPEN is executed. It is important to remember this if the query produces an error, or calls functions which have side effects.

This is necessary in order to [FETCH](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/fetch) rows from a cursor.

See [Cursor Overview](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/cursor-overview) for an example.

## See Also

- [Cursor Overview](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/cursor-overview)
- [DECLARE CURSOR](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/declare-cursor)
- [FETCH cursor_name](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/fetch)
- [CLOSE cursor_name](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/close)
- [Cursors in Oracle mode](/kb/en/sql_modeoracle-from-mariadb-103/#cursors)