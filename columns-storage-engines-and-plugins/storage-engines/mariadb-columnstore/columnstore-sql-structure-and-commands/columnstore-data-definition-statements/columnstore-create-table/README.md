# ColumnStore Create Table

A database consists of tables that store user data. You can create multiple columns with the create table statement. The data type follows the column name when adding columns.

## Syntax

```sql
CREATE TABLE [IF NOT EXISTS] tbl_name
    (create_definition,...)  
engine=columnstore  [ DEFAULT CHARSET=character-set] 
[COMMENT '[compression=0|1][;]
CREATE TABLE [IF NOT EXISTS] tbl_name
   { LIKE old_table_name | (LIKE old_table_name) }
create_definition:
    { col_name column_definition } 
column_definition:
    data_type
      [NOT NULL | NULL]
      [DEFAULT default_value]
      [COMMENT '[compression=0|1];
```

images here

## Notes:

- ColumnStore tables should not be created in the mysql, information_schema, calpontsys or test databases.
- ColumnStore stores all object names in lower case.
- CREATE TABLE LIKE is supported starting from version MariaDB CoumnStore 1.2 and higher
- CREATE TABLE AS SELECT is not supported, and will instead create the table in the default storage engine.
- Compression level (0 for no compression, 1 for compression) can be set at the system level. If a session default exists, this will override the system default. In turn, this can be overridden by the table level compression comment, and finally a compression comment at the column level.
- A table can be created in the front end only by using a ‘schema sync only’ comment. This could be useful when the table has been created on one user module, and needs to be synced to others.
- The column DEFAULT value can be a maximum of 64 characters.
- For maximum compatibility with external tools MariaDB ColumnStore will accept the following table attributes, however these are not implemented within MariaDB ColumnStore:
<ul start="1"><li>MIN_ROWS
</li><li>MAX_ROWS
</li><li>AUTO_INCREMENT
</li></ul>
- Datatype timestamp  is not supported for Columnstore below 1.4. Caused a "The syntax or the data type(s) is not supported by Columnstore" error.

All of these are ignored by ColumnStore.The following statement creates a table called orders with two columns: <em>orderkey</em> with datatype integer and <em>customer</em> with datatype varchar:

```sql
CREATE TABLE orders (
  orderkey INTEGER, 
  customer VARCHAR(45)
) ENGINE=ColumnStore
```