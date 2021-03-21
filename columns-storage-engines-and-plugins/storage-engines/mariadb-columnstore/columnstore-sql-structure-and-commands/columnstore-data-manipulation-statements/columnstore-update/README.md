# ColumnStore Update

The UPDATE statement changes data stored in rows.

## Syntax

Single-table syntax:

```sql
UPDATE  table_reference 
  SET col1={expr1|DEFAULT} [,col2={expr2|DEFAULT}] ...
  [WHERE where_condition]
  [ORDER BY ...]
  [LIMIT row_count]
```

Multiple-table syntax:

```sql
UPDATE table_references
    SET col1={expr1|DEFAULT} [, col2={expr2|DEFAULT}] ...
    [WHERE where_condition]
```

Note: 
It can only 1 table (but multiple columns)  be updated from the  table list in table_references.