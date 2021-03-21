# InnoDB Server Status Variables

See [Server Status Variables](/replication/optimization-and-tuning/system-variables/server-status-variables) for a complete list of status variables that can be viewed with [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status).

Much of the [InnoDB/XtraDB](/columns-storage-engines-and-plugins/storage-engines/innodb) information here can also be seen with a [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) statement.

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables).

#### `Innodb_adaptive_hash_cells`

- <strong>Description:</strong> As shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- <strong>Removed:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Innodb_adaptive_hash_hash_searches`

- <strong>Description:</strong> Hash searches as shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/), [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present. Use the `adaptive_hash_searches` counter in the <a undefined>information_schema.INNODB_METRICS</a> table instead.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Innodb_adaptive_hash_heap_buffers`

- <strong>Description:</strong> As shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- <strong>Removed:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Innodb_adaptive_hash_non_hash_searches`

- <strong>Description:</strong> Non-hash searches as shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/), [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present. Use the `adaptive_hash_searches_btree` counter in the <a undefined>information_schema.INNODB_METRICS</a> table instead.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Innodb_available_undo_logs`

- <strong>Description:</strong> Total number available InnoDB [undo logs](/kb/en/undo-log/). Differs from the  [innodb_undo_logs](/kb/en/innodb-system-variables/#innodb_undo_logs) system variable, which specifies the number of <em>active</em> undo logs.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_background_log_sync`

- <strong>Description:</strong> As shown in the BACKGROUND THREAD section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/), [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_buffer_pool_bytes_data`

- <strong>Description:</strong> Number of bytes contained in the [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/), both dirty (modified) and clean (unmodified). See also [Innodb_buffer_pool_pages_data](#innodb_buffer_pool_pages_data), which can contain pages of different sizes in the case of compression.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_bytes_dirty`

- <strong>Description:</strong> Number of dirty (modified) bytes contained in the [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/). See also [Innodb_buffer_pool_pages_dirty](#innodb_buffer_pool_pages_dirty), which can contain pages of different sizes in the case of compression.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_dump_status`

- <strong>Description:</strong> A text description of the progress or final status of the last Innodb buffer pool dump.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `string`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Innodb_buffer_pool_load_incomplete`

- <strong>Description:</strong> Whether or not the loaded buffer pool is incomplete, for example after a shutdown or abort during innodb buffer pool load from file caused an incomplete save.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `boolean`
- <strong>Introduced:</strong> [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)

---

#### `Innodb_buffer_pool_load_status`

- <strong>Description:</strong> A text description of the progress or final status of the last Innodb buffer pool load.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `string`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Innodb_buffer_pool_pages_data`

- <strong>Description:</strong> Number of [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/) pages which contain data, both dirty (modified) and clean (unmodified). See also [Innodb_buffer_pool_bytes_data](#innodb_buffer_pool_bytes_data).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_pages_dirty`

- <strong>Description:</strong> Number of [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/) pages which contain dirty (modified) data. See also [Innodb_buffer_pool_bytes_dirty](innodb_buffer_pool_bytes_dirty).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_pages_flushed`

- <strong>Description:</strong> Number of [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/) pages which have been flushed.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_pages_LRU_flushed`

- <strong>Description:</strong> Flush list as shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_buffer_pool_pages_LRU_freed`

- <strong>Description:</strong> Monitor the number of pages that were freed by a buffer pool LRU eviction scan, without flushing.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `Innodb_buffer_pool_pages_free`

- <strong>Description:</strong> Number of free [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/) pages.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_pages_made_not_young`

- <strong>Description:</strong> Pages not young as shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_buffer_pool_pages_made_young`

- <strong>Description:</strong> Pages made young as shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_buffer_pool_pages_misc`

- <strong>Description:</strong> Number of [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/) pages set aside for internal use.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_pages_old`

- <strong>Description:</strong> Old database page, as shown in the BUFFER POOL AND MEMORY section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present for XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_buffer_pool_pages_total`

- <strong>Description:</strong> Total number of [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/) pages.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_read_ahead`

- <strong>Description:</strong> Number of pages read into the [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/) by the read-ahead background thread.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_read_ahead_evicted`

- <strong>Description:</strong> Number of pages read into the [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/) by the read-ahead background thread that were evicted without having been accessed by queries.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_read_ahead_rnd`

- <strong>Description:</strong> Number of random read-aheads.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_read_requests`

- <strong>Description:</strong> Number of requests to read from the [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_reads`

- <strong>Description:</strong> Number of reads that could not be satisfied by the [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/) and had to be read from disk.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_resize_status`

- <strong>Description:</strong> Progress of the dynamic [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/) resizing operation. See [Setting Innodb Buffer Pool Size Dynamically](/replication/optimization-and-tuning/system-variables/setting-innodb-buffer-pool-size-dynamically).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `Innodb_buffer_pool_wait_free`

- <strong>Description:</strong> Number of times InnoDB waited for a free page before reading or creating a page. Normally, writes to the [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/) happen in the background. When no clean pages are available, dirty pages are flushed first in order to free some up. This counts the numbers of wait for this operation to finish. If this value is not small, look at increasing [innodb_buffer_pool_size](/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_size).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffer_pool_write_requests`

- <strong>Description:</strong> Number of requests to write to the [InnoDB buffer pool](/kb/en/xtradbinnodb-memory-buffer/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_buffered_aio_submitted`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_checkpoint_age`

- <strong>Description:</strong> The checkpoint age, as shown in the LOG section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output. (This is equivalent to subtracting "Last checkpoint at" from "Log sequence number".)
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_checkpoint_max_age`

- <strong>Description:</strong> Max checkpoint age, as shown in the LOG section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_checkpoint_target_age`

- <strong>Description:</strong> Checkpoint age target, as shown in the LOG section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output. XtraDB only. Removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and replaced with MySQL 5.6's flushing implementation.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `Innodb_current_row_locks`

- <strong>Description:</strong> Number of current row locks on InnoDB tables as shown in the TRANSACTIONS section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output. Renamed from [InnoDB_row_lock_numbers](#innodb_row_lock_numbers) in XtraDB 5.5.8-20.1.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_data_fsyncs`

- <strong>Description:</strong> Number of InnoDB fsync (sync-to-disk) calls. fsync call frequency can be influenced by the [innodb_flush_method](/kb/en/xtradbinnodb-server-system-variables/#innodb_flush_method) configuration option.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_data_pending_fsyncs`

- <strong>Description:</strong> Number of pending InnoDB fsync (sync-to-disk) calls. fsync call frequency can be influenced by the [innodb_flush_method](/kb/en/xtradbinnodb-server-system-variables/#innodb_flush_method) configuration option.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_data_pending_reads`

- <strong>Description:</strong> Number of pending InnoDB reads.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_data_pending_writes`

- <strong>Description:</strong> Number of pending InnoDB writes.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_data_read`

- <strong>Description:</strong> Number of InnoDB bytes read since server startup (not to be confused with [Innodb_data_reads](#innodb_data_reads)).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_data_reads`

- <strong>Description:</strong> Number of InnoDB read operations (not to be confused with [Innodb_data_read](#innodb_data_read)).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_data_writes`

- <strong>Description:</strong> Number of InnoDB write operations.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_data_written`

- <strong>Description:</strong> Number of InnoDB bytes written since server startup.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_dblwr_pages_written`

- <strong>Description:</strong> Number of pages written to the [InnoDB doublewrite buffer](/kb/en/xtradbinnodb-doublewrite-buffer/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_dblwr_writes`

- <strong>Description:</strong> Number of writes to the [InnoDB doublewrite buffer](/kb/en/xtradbinnodb-doublewrite-buffer/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_deadlocks`

- <strong>Description:</strong> Total number of InnoDB deadlocks. Deadlocks occur when at least two transactions are waiting for the other to finish, creating a circular dependency. InnoDB usually detects these quickly, returning an error.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_defragment_compression_failures`

- <strong>Description:</strong> Number of defragment re-compression failures. See [Defragmenting InnoDB Tablespaces](/replication/optimization-and-tuning/optimizing-tables/defragmenting-innodb-tablespaces).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Innodb_defragment_count`

- <strong>Description:</strong> Number of defragment operations. See [Defragmenting InnoDB Tablespaces](/replication/optimization-and-tuning/optimizing-tables/defragmenting-innodb-tablespaces).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Innodb_defragment_failures`

- <strong>Description:</strong> Number of defragment failures. See [Defragmenting InnoDB Tablespaces](/replication/optimization-and-tuning/optimizing-tables/defragmenting-innodb-tablespaces).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Innodb_dict_tables`

- <strong>Description:</strong> Number of entries in the XtraDB data dictionary cache. This Percona XtraDB variable was removed in MariaDB 10/XtraDB 5.6 as it was replaced with MySQL 5.6's [table_definition_cache](/kb/en/server-system-variables/#table_definition_cache) implementation.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> XtraDB 5.0.77-b13
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `Innodb_encryption_n_merge_blocks_decrypted`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.28](/kb/en/mariadb-10128-release-notes/), [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/)

---

#### `Innodb_encryption_n_merge_blocks_encrypted`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.28](/kb/en/mariadb-10128-release-notes/), [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/)

---

#### `Innodb_encryption_n_rowlog_blocks_decrypted`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.28](/kb/en/mariadb-10128-release-notes/), [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/)

---

#### `Innodb_encryption_n_rowlog_blocks_encrypted`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.28](/kb/en/mariadb-10128-release-notes/), [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/)

---

#### `Innodb_encryption_n_temp_blocks_decrypted`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/)

---

#### `Innodb_encryption_n_temp_blocks_encrypted`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/)

---

#### `Innodb_encryption_num_key_requests`

- <strong>Description:</strong> Was not present in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `Innodb_encryption_rotation_estimated_iops`

- <strong>Description:</strong> See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `Innodb_encryption_rotation_pages_flushed`

- <strong>Description:</strong> See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `Innodb_encryption_rotation_pages_modified`

- <strong>Description:</strong> See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `Innodb_encryption_rotation_pages_read_from_cache`

- <strong>Description:</strong> See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `Innodb_encryption_rotation_pages_read_from_disk`

- <strong>Description:</strong> See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `Innodb_have_atomic_builtins`

- <strong>Description:</strong> Whether the server has been built with atomic instructions, provided by the CPU ensuring that critical low-level operations can't be interrupted. XtraDB only.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `boolean`

---

#### `Innodb_have_bzip2`

- <strong>Description:</strong> Whether the server has the bzip2 compression method available. See [InnoDB/XtraDB Page Compression](/kb/en/innodbxtradb-page-compression/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `boolean`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `Innodb_have_lz4`

- <strong>Description:</strong> Whether the server has the lz4 compression method available. See [InnoDB/XtraDB Page Compression](/kb/en/innodbxtradb-page-compression/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `boolean`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `Innodb_have_lzma`

- <strong>Description:</strong> Whether the server has the lzma compression method available. See [InnoDB/XtraDB Page Compression](/kb/en/innodbxtradb-page-compression/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `boolean`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `Innodb_have_lzo`

- <strong>Description:</strong> Whether the server has the lzo compression method available. See [InnoDB/XtraDB Page Compression](/kb/en/innodbxtradb-page-compression/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `boolean`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `Innodb_have_punch_hole`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `Innodb_have_snappy`

- <strong>Description:</strong> Whether the server has the snappy compression method available. See [InnoDB/XtraDB Page Compression](/kb/en/innodbxtradb-page-compression/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `boolean`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `Innodb_history_list_length`

- <strong>Description:</strong> History list length as shown in the TRANSACTIONS section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output. XtraDB only until introduced in [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_ibuf_discarded_delete_marks`

- <strong>Description:</strong> As shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_ibuf_discarded_deletes`

- <strong>Description:</strong> As shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_ibuf_discarded_inserts`

- <strong>Description:</strong> As shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_ibuf_free_list`

- <strong>Description:</strong> As shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_ibuf_merged_delete_marks`

- <strong>Description:</strong> As shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_ibuf_merged_deletes`

- <strong>Description:</strong> As shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_ibuf_merged_inserts`

- <strong>Description:</strong> As shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_ibuf_merges`

- <strong>Description:</strong> As shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_ibuf_segment_size`

- <strong>Description:</strong> As shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_ibuf_size`

- <strong>Description:</strong> As shown in the INSERT BUFFER AND ADAPTIVE HASH INDEX section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_instant_alter_column`

- <strong>Description:</strong> See [Instant ADD COLUMN for InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/instant-add-column-for-innodb).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/)

---

#### `Innodb_log_waits`

- <strong>Description:</strong> Number of times InnoDB was forced to wait for log writes to be flushed due to the log buffer being too small.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_log_write_requests`

- <strong>Description:</strong> Number of requests to write to the InnoDB redo log.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_log_writes`

- <strong>Description:</strong> Number of writes to the InnoDB redo log.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_lsn_current`

- <strong>Description:</strong> Log sequence number as shown in the LOG section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_lsn_flushed`

- <strong>Description:</strong> Flushed up to log sequence number as shown in the LOG section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_lsn_last_checkpoint`

- <strong>Description:</strong> Log sequence number last checkpoint as shown in the LOG section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_master_thread_1_second_loops`

- <strong>Description:</strong> As shown in the BACKGROUND THREAD section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output. 
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `Innodb_master_thread_10_second_loops`

- <strong>Description:</strong> As shown in the BACKGROUND THREAD section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, this system variable is not present
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `Innodb_master_thread_active_loops`

- <strong>Description:</strong> As shown in the BACKGROUND THREAD section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/):<strong>
</strong>

---

#### `Innodb_master_thread_background_loops`

- <strong>Description:</strong> As shown in the BACKGROUND THREAD section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, this system variable is not present
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `Innodb_master_thread_idle_loops`

- <strong>Description:</strong> As shown in the BACKGROUND THREAD section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/):<strong>
</strong>

---

#### `Innodb_master_thread_main_flush_loops`

- <strong>Description:</strong> As shown in the BACKGROUND THREAD section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, this system variable is not present
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `Innodb_master_thread_sleeps`

- <strong>Description:</strong> As shown in the BACKGROUND THREAD section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine) output. XtraDB only.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/), [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present. Use the `innodb_master_thread_sleeps` counter in the <a undefined>information_schema.INNODB_METRICS</a> table instead.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `Innodb_max_trx_id`

- <strong>Description:</strong> As shown in the TRANSACTIONS section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_mem_adaptive_hash`

- <strong>Description:</strong> As shown in the BUFFER POOL AND MEMORY section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_mem_dictionary`

- <strong>Description:</strong> As shown in the BUFFER POOL AND MEMORY section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this system variable is not present.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable was reintroduced.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) (XtraDB-only), [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Innodb_mem_total`

- <strong>Description:</strong> As shown in the BUFFER POOL AND MEMORY section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_mutex_os_waits`

- <strong>Description:</strong> Mutex OS waits as shown in the SEMAPHORES section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_mutex_spin_rounds`

- <strong>Description:</strong> Mutex spin rounds as shown in the SEMAPHORES section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_mutex_spin_waits`

- <strong>Description:</strong> Mutex spin waits as shown in the SEMAPHORES section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_num_index_pages_written`

- <strong>Description:</strong>
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `Innodb_num_non_index_pages_written`

- <strong>Description:</strong>
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `Innodb_num_open_files`

- <strong>Description:</strong> Number of open files held by InnoDB. InnoDB only.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Innodb_num_page_compressed_trim_op`

- <strong>Description:</strong> Number of trim operations performed.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `Innodb_num_page_compressed_trim_op_saved`

- <strong>Description:</strong> Number of trim operations not done because of an earlier trim.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `Innodb_num_pages_decrypted`

- <strong>Description:</strong> Number of pages page decrypted. See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/). Was originally named `Innodb_num_pages_page_decrypted` in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/), renamed to its current name in [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `Innodb_num_pages_encrypted`

- <strong>Description:</strong> Number of pages page encrypted. See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/). Was originally named `Innodb_num_pages_page_encrypted` in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/), renamed to its current name in [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `Innodb_num_pages_page_compressed`

- <strong>Description:</strong> Number of pages that are page compressed.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `Innodb_num_pages_page_compression_error`

- <strong>Description:</strong> Number of compression errors.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1](/kb/en/what-is-mariadb-101/)

---

#### `Innodb_num_pages_page_decompressed`

- <strong>Description:</strong> Number of pages compressed with page compression that are decompressed.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `Innodb_num_pages_page_encryption_error`

- <strong>Description:</strong> Number of page encryption errors. See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Innodb_oldest_view_low_limit_trx_id`

- <strong>Description:</strong> As shown in the TRANSACTIONS section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_onlineddl_pct_progress`

- <strong>Description:</strong> Shows the progress of in-place alter table. It might be not so accurate because in-place alter is highly dependent on disk and buffer pool status. See [Monitoring progress and temporal memory usage of Online DDL in InnoDB](https://blog.mariadb.org/monitoring-progress-and-temporal-memory-usage-of-online-ddl-in-innodb).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Innodb_onlineddl_rowlog_pct_used`

- <strong>Description:</strong> Shows row log buffer usage in 5-digit integer (10000 means 100.00%). See [Monitoring progress and temporal memory usage of Online DDL in InnoDB](https://blog.mariadb.org/monitoring-progress-and-temporal-memory-usage-of-online-ddl-in-innodb).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Innodb_onlineddl_rowlog_rows`

- <strong>Description:</strong> Number of rows stored in the row log buffer. See [Monitoring progress and temporal memory usage of Online DDL in InnoDB](https://blog.mariadb.org/monitoring-progress-and-temporal-memory-usage-of-online-ddl-in-innodb).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Innodb_os_log_fsyncs`

- <strong>Description:</strong> Number of InnoDB log fsync (sync-to-disk) requests.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_os_log_pending_fsyncs`

- <strong>Description:</strong> Number of pending InnoDB log fsync (sync-to-disk) requests.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_os_log_pending_writes`

- <strong>Description:</strong> Number of pending InnoDB log writes.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_os_log_written`

- <strong>Description:</strong> Number of bytes written to the InnoDB log.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_page_compression_saved`

- <strong>Description:</strong> Number of bytes saved by page compression.
- <strong>Scope:</strong>
- <strong>Data Type:</strong>
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/), [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/) Fusion-io

---

#### `Innodb_page_compression_trim_sect512`

- <strong>Description:</strong> Number of TRIM operations performed for the page-compression/NVM Compression workload for the 512 byte block-size.
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/), [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/) Fusion-io
- <strong>Removed:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `Innodb_page_compression_trim_sect1024`

- <strong>Description:</strong> Number of TRIM operations performed for the page-compression/NVM Compression workload for the 1K block-size.
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/), [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/) Fusion-io
- <strong>Removed:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `Innodb_page_compression_trim_sect2048`

- <strong>Description:</strong> Number of TRIM operations performed for the page-compression/NVM Compression workload for the 2K block-size.
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/), [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/) Fusion-io
- <strong>Removed:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `Innodb_page_compression_trim_sect4096`

- <strong>Description:</strong> Number of TRIM operations performed for the page-compression/NVM Compression workload for the 4K block-size.
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/), [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/) Fusion-io
- <strong>Removed:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `Innodb_page_compression_trim_sect8192`

- <strong>Description:</strong> Number of TRIM operations performed for the page-compression/NVM Compression workload for the 8K block-size.
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/), [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/) Fusion-io
- <strong>Removed:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `Innodb_page_compression_trim_sect16384`

- <strong>Description:</strong> Number of TRIM operations performed for the page-compression/NVM Compression workload for the 16K block-size.
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/), [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/) Fusion-io
- <strong>Removed:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `Innodb_page_compression_trim_sect32768`

- <strong>Description:</strong> Number of TRIM operations performed for the page-compression/NVM Compression workload for the 32K block-size.
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/), [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/) Fusion-io
- <strong>Removed:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `Innodb_page_size`

- <strong>Description:</strong> Page size used by InnoDB. Defaults to 16KB, can be compiled with a different value.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_pages_created`

- <strong>Description:</strong> Number of InnoDB pages created.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_pages_read`

- <strong>Description:</strong> Number of InnoDB pages read.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_pages0_read`

- <strong>Description:</strong> Counter for keeping track of reads of the first page of InnoDB data files, because the original implementation of data-at-rest-encryption for InnoDB introduced new code paths for reading the pages. Removed in [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/) as the extra reads of the first page were removed, and the encryption subsystem will be initialized whenever we first read the first page of each data file, in fil_node_open_file().
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/), [MariaDB 10.1.21](/kb/en/mariadb-10121-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/)

---

#### `Innodb_pages_written`

- <strong>Description:</strong> Number of InnoDB pages written.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_purge_trx_id`

- <strong>Description:</strong> Purge transaction id as shown in the TRANSACTIONS section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_purge_undo_no`

- <strong>Description:</strong> As shown in the TRANSACTIONS section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_read_views_memory`

- <strong>Description:</strong> As shown in the BUFFER POOL AND MEMORY section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output. Shows the total of memory in bytes allocated for the InnoDB read view.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5.32](/kb/en/mariadb-5532-release-notes/)

---

#### `Innodb_row_lock_current_waits`

- <strong>Description:</strong> Number of pending row locks on InnoDB tables.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_row_lock_numbers`

- <strong>Description:</strong> Number of current row locks on InnoDB tables as shown in the TRANSACTIONS section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output. Renamed to [InnoDB_current_row_locks](#innodb_current_row_locks) in XtraDB 5.5.10-20.1.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) / XtraDB 5.5.8-20
- <strong>Removed:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/) / XtraDB 5.5.10-20.1

---

#### `Innodb_row_lock_time`

- <strong>Description:</strong> Total time in milliseconds spent getting InnoDB row locks.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_row_lock_time_avg`

- <strong>Description:</strong> Average time in milliseconds spent getting InnoDB row locks.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_row_lock_time_max`

- <strong>Description:</strong> Maximum time in milliseconds spent getting an InnoDB row lock.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_row_lock_time_waits`

- <strong>Description:</strong> Number of times InnoDB had to wait before getting a row lock.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_rows_deleted`

- <strong>Description:</strong> Number of rows deleted from InnoDB tables.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_rows_inserted`

- <strong>Description:</strong> Number of rows inserted into InnoDB tables.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_rows_read`

- <strong>Description:</strong> Number of rows read from InnoDB tables.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_rows_updated`

- <strong>Description:</strong> Number of rows updated in InnoDB tables.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Innodb_s_lock_os_waits`

- <strong>Description:</strong> As shown in the SEMAPHORES section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_s_lock_spin_rounds`

- <strong>Description:</strong> As shown in the SEMAPHORES section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_s_lock_spin_waits`

- <strong>Description:</strong> As shown in the SEMAPHORES section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_scrub_background_page_reorganizations`

- <strong>Description:</strong> See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Innodb_scrub_background_page_split_failures_missing_index`

- <strong>Description:</strong> See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Innodb_scrub_background_page_split_failures_out_of_filespace`

- <strong>Description:</strong> See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Innodb_scrub_background_page_split_failures_underflow`

- <strong>Description:</strong> See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Innodb_scrub_background_page_split_failures_unknown`

- <strong>Description:</strong> See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/), [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Innodb_scrub_background_page_splits`

- <strong>Description:</strong> See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1](/kb/en/what-is-mariadb-101/)
- <strong>Removed:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Innodb_scrub_log`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Innodb_secondary_index_triggered_cluster_reads`

- <strong>Description:</strong> Used to track the effectiveness of the Prefix Index Queries Optimization ([MDEV-6929](https://jira.mariadb.org/browse/MDEV-6929))
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

#### `Innodb_secondary_index_triggered_cluster_reads_avoided`

- <strong>Description:</strong> Used to track the effectiveness of the Prefix Index Queries Optimization ([MDEV-6929](https://jira.mariadb.org/browse/MDEV-6929))
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

#### `Innodb_system_rows_deleted`

- <strong>Description:</strong>
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/)

---

#### `Innodb_system_rows_inserted`

- <strong>Description:</strong>
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/)

---

#### `Innodb_system_rows_read`

- <strong>Description:</strong>
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/)

---

#### `Innodb_system_rows_updated`

- <strong>Description:</strong>
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/)

---

#### `Innodb_truncated_status_writes`

- <strong>Description:</strong> Number of times output from [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) has been truncated.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_undo_truncations`

- <strong>Description:</strong> Number of undo tablespace truncation operations.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/)

---

#### `Innodb_x_lock_os_waits`

- <strong>Description:</strong> As shown in the SEMAPHORES section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_x_lock_spin_rounds`

- <strong>Description:</strong> As shown in the SEMAPHORES section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Innodb_x_lock_spin_waits`

- <strong>Description:</strong> As shown in the SEMAPHORES section of the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) output.
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), this system variable is present in XtraDB.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, this system variable is not present.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---