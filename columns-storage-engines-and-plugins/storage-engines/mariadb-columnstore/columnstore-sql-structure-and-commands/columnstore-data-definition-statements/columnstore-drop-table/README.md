# ColumnStore Drop Table

The [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table) statement deletes a table from ColumnStore.

## Syntax

```sql
DROP  TABLE [IF EXISTS] 
    tbl_name 
    [RESTRICT ]
```

The RESTRICT clause limits the table to being dropped in the front end only.  This could be useful when the table has been dropped on one user module, and needs to be synced to others.

images here

The following statement drops the <em>orders</em> table on the front end only:

```sql
DROP TABLE orders RESTRICT;
```

## See also

- [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table)