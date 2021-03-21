# ColumnStore Alter View

Alters the definition of a view. CREATE OR REPLACE VIEW may also be used to alter the definition
of a view.

## Syntax

```sql
CREATE
    [OR REPLACE]
    VIEW view_name [(column_list)]
    AS select_statement
```