# ColumnStore Non-Distributed Post-Processed Functions

ColumnStore supports all MariaDB functions that can be used in a post-processing manner where data is returned by ColumnStore first and then MariaDB executes the function on the data returned. The functions are currently supported only in the projection (SELECT) and ORDER BY portions of the SQL statement.

## See also

- [ColumnStore Distributed Functions](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-sql-structure-and-commands/columnstore-distributed-functions)