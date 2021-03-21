# NAME_CONST

## Syntax

```sql
NAME_CONST(name,value)
```

## Description

Returns the given value. When used to produce a result set column,
 <code class="highlight fixed" style="white-space:pre-wrap">NAME_CONST()</code> causes the column to have the given name. The
arguments should be constants.

This function is used internally when replicating stored procedures. It makes little sense to use it explicitly in SQL statements, and it was not supposed to be used like that.

```sql
SELECT NAME_CONST('myname', 14);
+--------+
| myname |
+--------+
|     14 |
+--------+
```