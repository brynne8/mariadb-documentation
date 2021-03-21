# InnoDB Change Buffering

INSERT, UPDATE and DELETE statements can be particularly heavy operations to perform, as all indexes need to be updated after each change. For this reason these changes are often buffered.

Pages are modified in the [buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/), and not immediately on disk. When rows are deleted, a flag is set, thus rows are not immediately deleted on disk. Later the changes will be written to disk (''flushed'') by InnoDB background threads. Pages that have been modified in memory and not yet flushed are called dirty pages. The buffering of data changes is called Change Buffer.

Before [MariaDB 5.5](/kb/en/what-is-mariadb-55/), only inserted rows could be buffered, so this buffer was called Insert Buffer. The old name still appears in several places, for example in the output of [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status/).

The change buffer only contains changes to the indexes. Changes to UNIQUE indexes are primary keys cannot be buffered, because InnoDB has to read the disk to check for duplicate values.

The Change Buffer is an optimization because:

- A page can be modified several times in memory and be flushed to disk only once.
- Dirty pages are flushed together, so the number of IO operations is lower.

If the server crashes, usually the Change Buffer is not empty. However, changes are not lost because they are written to the transaction logs, so they can be applied at server restart.

The main server system variable here is [innodb_change_buffering](/kb/en/xtradbinnodb-server-system-variables/#innodb_change_buffering), which determines which form of change buffering, if any, to use.

The following settings are available:

- inserts
<ul><li>Only buffer insert operations
</li></ul>
- deletes
<ul><li>Only buffer delete operations
</li></ul>
- changes
<ul><li>Buffer both insert and delete operations
</li></ul>
- purges
<ul><li>Buffer the actual physical deletes that occur in the background
</li></ul>
- all
<ul><li>Buffer inserts, deletes and purges. This is the default setting from [MariaDB 5.5](/kb/en/what-is-mariadb-55/).
</li></ul>
- none
<ul><li>Don't buffer any operations.
</li></ul>

Modifying the value of this variable only affects the buffering of new operations. The merging of already buffered changes is not affected.

The [innodb_change_buffer_max_size](/kb/en/xtradbinnodb-server-system-variables/#innodb_change_buffer_max_size) server system variable, introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/), determines the maximum size of the change buffer, expressed as a percentage of the buffer pool.

## Change Buffer Related Status Variables

- [Innodb_buffer_pool_pages_dirty](/kb/en/xtradbinnodb-server-status-variables/#innodb_buffer_pool_pages_dirty): Number of dirty pages in the Change Buffer.
- [innodb_buffer_pool_bytes_dirty](/kb/en/xtradbinnodb-server-status-variables/#innodb_buffer_pool_bytes_dirty): Total size of the dirty pages, in bytes.
- [innodb_buffer_pool_wait_free](/kb/en/xtradbinnodb-server-status-variables/#innodb_buffer_pool_wait_free): How many times InnoDB was forced to flush dirty pages to write new data, because the buffer pool had no more free pages.

## See Also

- [InnoDB Buffer Pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/)