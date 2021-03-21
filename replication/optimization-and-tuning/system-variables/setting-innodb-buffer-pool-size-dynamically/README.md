# Setting Innodb Buffer Pool Size Dynamically

##### MariaDB starting with [10.2.2](/kb/en/mariadb-1022-release-notes/)

From [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), the [InnoDB Buffer Pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/) size can be set dynamically.

Resizing the buffer pool is performed in chunks determined by the size of the [innodb_buffer_pool_chunk_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_chunk_size) variable.

The resize operation waits until all active transactions and operations are completed, and new transactions and operations that need to access the buffer pool must wait until the resize is complete (although  when decreasing the size, access is permitted when defragmenting and withdrawing pages).

Nested transactions may fail if started after the buffer pool resize has begun.

The new buffer pool size must be a multiple of [innodb_buffer_pool_chunk_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_chunk_size) * [innodb_buffer_pool_instances](/kb/en/innodb-system-variables/#innodb_buffer_pool_instances) (note that `innodb_buffer_pool_instances` is ignored from [MariaDB 10.5](/kb/en/what-is-mariadb-105/), and removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/), as the buffer pool is no longer split into multiple instances). If you attempt to set a different figure, the value is automatically adjusted to a multiple of at least the attempted size. Note that adjusting the [innodb_buffer_pool_chunk_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_chunk_size) setting can result in a change in the buffer pool size.

The number of chunks as calculated by [innodb_buffer_pool_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_size) / [innodb_buffer_pool_chunk_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_chunk_size) should not exceed 1000 in order to avoid performance issues.

A background thread performs the resizing operation. The [Innodb_buffer_pool_resize_status](/kb/en/innodb-status-variables/#innodb_buffer_pool_resize_status) status variable shows the progress of the resizing operation, for example:

```sql
SHOW STATUS LIKE 'Innodb_buffer_pool_resize_status';
+----------------------------------+----------------------------------+
| Variable_name                    | Value                            |
+----------------------------------+----------------------------------+
| Innodb_buffer_pool_resize_status | Resizing also other hash tables. |
+----------------------------------+----------------------------------+
```

or

```sql
SHOW STATUS LIKE 'Innodb_buffer_pool_resize_status';
+----------------------------------+----------------------------------------------------+
| Variable_name                    | Value                                              |
+----------------------------------+----------------------------------------------------+
| Innodb_buffer_pool_resize_status | Completed resizing buffer pool at 161103 16:26:54. |
+----------------------------------+----------------------------------------------------+
```

Progress is also logged in the [error log](/mariadb-administration/server-monitoring-logs/error-log/).