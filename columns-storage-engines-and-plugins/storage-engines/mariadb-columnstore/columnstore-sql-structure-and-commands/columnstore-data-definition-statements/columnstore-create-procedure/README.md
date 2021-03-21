# ColumnStore Create Procedure

Creates a stored routine in ColumnStore.

## Syntax

```sql
CREATE
    [DEFINER = { user | CURRENT_USER }]
    PROCEDURE sp_name ([proc_parameter[,...]])
    [characteristic ...] routine_body

proc_parameter:
    [ IN | OUT | INOUT ] param_name type

type:
    Any valid MariaDB ColumnStore data type

routine_body:
    Valid SQL procedure statement
```

ColumnStore currently accepts definition of stored procedures with only input arguments and a single SELECT query while in Operating Mode = 1 (VTABLE mode). However, while in the Operating Mode = 0 (TABLE mode), ColumnStore will allow additional complex definition of stored procedures (i.e., OUT parameter, declare, cursors,etc.)

See Operating Mode for information on Operating Modes

images here

The following statements create and call the sp_complex_variable stored procedure:

```sql
CREATE PROCEDURE sp_complex_variable(in arg_key int, in arg_date date)
  begin
    Select *
    from lineitem, orders
    where o_custkey < arg_key
    and l_partkey < 10000
    and l_shipdate>arg_date
    and l_orderkey = o_orderkey
    order by l_orderkey, l_linenumber;
  end

call sp_complex_variable(1000, '1998-10-10');
```