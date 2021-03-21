# Dynamic Columns Functions

[Dynamic columns](/sql-statements-structure/nosql/dynamic-columns/) is a feature that allows one to store different sets of columns for each row in a table. It works by storing a set of columns in a blob and having a small set of functions to manipulate it.

- [COLUMN_ADD](/built-in-functions/special-functions/dynamic-columns-functions/column_add/) — Adds or updates dynamic columns
- [COLUMN_CHECK](/built-in-functions/special-functions/dynamic-columns-functions/column_check/) — Checks if a dynamic column blob is valid
- [COLUMN_CREATE](/built-in-functions/special-functions/dynamic-columns-functions/column_create/) — Returns a dynamic columns blob
- [COLUMN_DELETE](/built-in-functions/special-functions/dynamic-columns-functions/column_delete/) — Deletes a dynamic column
- [COLUMN_EXISTS](/built-in-functions/special-functions/dynamic-columns-functions/column_exists/) — Checks is a column exists
- [COLUMN_GET](/built-in-functions/special-functions/dynamic-columns-functions/column_get/) — Gets a dynamic column value by name
- [COLUMN_JSON](/built-in-functions/special-functions/dynamic-columns-functions/column_json/) — Returns a JSON representation of dynamic column blob data
- [COLUMN_LIST](/built-in-functions/special-functions/dynamic-columns-functions/column_list/) — Returns comma-separated list