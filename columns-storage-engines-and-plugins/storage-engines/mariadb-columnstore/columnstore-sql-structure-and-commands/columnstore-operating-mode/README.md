# ColumnStore Operating Mode

ColumnStore has the ability to support full MariaDB query syntax through an operating mode. This operating mode may be set as a default for the instance or set at the session level. To set the operating mode at the session level, the following command is used. Once the session has ended, any subsequent session will return to the default for the instance.

```sql
set infinidb_vtable_mode = n
```

where n is:

- 0) a generic, highly compatible row-by-row processing mode. Some WHERE clause components can be processed by ColumnStore, but joins are processed entirely by mysqld using a nested-loop join mechanism.
- 1) (the default) query syntax is evaluated by ColumnStore for compatibility with distributed execution and incompatible queries are rejected. Queries executed in this mode take advantage of distributed execution and typically result in higher performance.
- 2) auto-switch mode: ColumnStore will attempt to process the query internally, if it cannot, it will automatically switch the query to run in row-by-row mode.