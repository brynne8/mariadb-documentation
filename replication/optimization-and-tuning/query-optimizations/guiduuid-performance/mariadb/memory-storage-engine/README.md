# MEMORY Storage Engine

Contents of the MEMORY storage engine (previously known as HEAP) are stored in memory rather than on disk.

It is best-used for read-only caches of data from other tables, or for temporary work areas.

Since the data is stored in memory, it is highly vulnerable to power outages or hardware failure, and is unsuitable for permanent data storage. In fact, after a server restart, `MEMORY` tables will be recreated (because the definition file is stored on disk), but they will be empty. It is possible to re-populate them with a query using the `--init-file` server startup option.

Variable-length types like [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/) can be used in MEMORY tables. [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) or [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) columns are not supported for MEMORY tables.

The maximum total size of MEMORY tables cannot exceed the <a undefined>max_heap_table_size</a> system server variable. When a table is created this value applies to that table, and when the server is restarted this value applies to existing tables. Changing this value has no effect on existing tables. However, executing a `ALTER TABLE ... ENGINE=MEMORY` statement applies the current value of `max_heap_table_size` to the table. Also, it is possible to change the session value of `max_heap_table_size` before creating a table, to make sure that tables created by other sessions are not affected.

The `MAX_ROWS` table option provides a hint about the number of rows you plan to store in them. This is not a hard limit that cannot be exceeded, and does not allow to exceed `max_heap_table_size`. The storage engine uses max_heap_table_size and MAX_ROWS to calculate the maximum memory that could be allocated for the table.

When rows are deleted, space is not automatically freed. The only way to free space without dropping the table is using `ALTER TABLE tbl_name ENGINE = MEMORY`. [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table/) frees the memory too.

## Index Type

The MEMORY storage engine permits indexes to be either B-tree or Hash. Hash is the default type for MEMORY. See [Storage Engine index types](/replication/optimization-and-tuning/optimization-and-indexes/storage-engine-index-types/) for more on their characteristics.

A MEMORY table can have up to 64 indexes, 16 columns for each index and a maximum key length of 3072 bytes.

## See Also

- [Performance of MEMORY tables](/kb/en/performance-of-memory-tables/)

## Example

The following example shows how to create a `MEMORY` table with a given maximum size, as described above.

```sql
SET max_heap_table_size = 1024*516;

CREATE TABLE t (a VARCHAR(10), b INT) ENGINE = MEMORY;

SET max_heap_table_size = @@max_heap_table_size;
```