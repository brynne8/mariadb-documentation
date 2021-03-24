# InnoDB System Variables

This page documents system variables related to the [XtraDB/InnoDB storage engine](/columns-storage-engines-and-plugins/storage-engines/innodb/). For options that are not system variables, see [InnoDB Options](/kb/en/mysqld-options/#innodb-options).

See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting them.

Also see the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `have_innodb`

- <strong>Description:</strong> If the server supports [InnoDB tables](/columns-storage-engines-and-plugins/storage-engines/innodb/), will be set to `YES`, otherwise will be set to `NO`. Removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/), use the [Information Schema PLUGINS](/kb/en/information-schema-plugins-table/) table or [SHOW ENGINES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engines/) instead.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `ignore_builtin_innodb`

- <strong>Description:</strong> Setting this to `1` results in the built-in InnoDB storage engine being ignored. In some versions of MariaDB, XtraDB is the default and is always present, so this variable is ignored and setting it results in a warning. From [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/) to [MariaDB 10.0.8](/kb/en/mariadb-1008-release-notes/), when InnoDB was the default instead of XtraDB, this variable needed to be set. Usually used in conjunction with the [plugin-load=innodb=ha_innodb](/kb/en/mysqld-options/#-plugin-load) option to use the InnoDB plugin.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--ignore-builtin-innodb</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `innodb_adaptive_checkpoint`

- <strong>Description:</strong> Replaced with [innodb_adaptive_flushing_method](#innodb_adaptive_flushing_method). Controls adaptive checkpointing. InnoDB's fuzzy checkpointing can cause stalls, as many dirty blocks are flushed at once as the checkpoint age nears the maximum. Adaptive checkpointing aims for more consistent flushing, approximately `modified age / maximum checkpoint age`. Can result in larger transaction log files
<ul start="1"><li>`reflex` Similar to [innodb_max_dirty_pages_pct](#innodb_max_dirty_pages_pct) flushing but flushes blocks constantly and contiguously based on the oldest modified age. If the age exceeds 1/2 of the maximum age capacity, flushing will be weak contiguous. If the age exceeds 3/4, flushing will be strong. Strength can be adjusted by the variable [innodb_io_capacity](#innodb_io_capacity).
</li><li>`estimate` The default, and independent of [innodb_io_capacity](#innodb_io_capacity). If the oldest modified age exceeds 1/2 of the maximum age capacity, blocks will be flushed every second at a rate determined by the number of modified blocks, LSN progress speed and the average age of all modified blocks.
</li><li>`keep_average` Attempts to keep the I/O rate constant by using a shorter loop cycle of one tenth of a second. Designed for SSD cards.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-adaptive-checkpoint=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `estimate`
- <strong>Valid Values:</strong> `none` or `0`, `reflex` or `1`, `estimate` or `2`, `keep_average` or `3`
- <strong>Removed:</strong> XtraDB 5.5 - replaced with [innodb_adaptive_flushing_method](#innodb_adaptive_flushing_method)

---

#### `innodb_adaptive_flushing`

- <strong>Description:</strong> If set to `1`, the default, the server will dynamically adjust the flush rate of dirty pages in the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) buffer pool. This assists to reduce brief bursts of I/O activity.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-adaptive-flushing=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `innodb_adaptive_flushing_lwm`

- <strong>Description:</strong> Adaptive flushing is enabled when this this low water mark percentage of the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) capacity is reached.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-adaptive-flushing-lwm=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `double`
- <strong>Default Value:</strong> `10.000000`
- <strong>Range:</strong> `0` to `70`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_adaptive_flushing_method`

- <strong>Description:</strong> Determines the method of flushing dirty blocks from the InnoDB [buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/). If set to `native` or `0`, the original InnoDB method is used. The maximum checkpoint age is determined by the total length of all transaction log files. When the checkpoint age reaches the maximum checkpoint age, blocks are flushed. This can cause lag if there are many updates per second and many blocks with an almost identical age need to be flushed. If set to `estimate` or `1`, the default, the oldest modified age will be compared with the maximum age capacity. If it's more than 1/4 of this age, blocks are flushed every second. The number of blocks flushed is determined by the number of modified blocks, the LSN progress speed and the average age of all modified blocks. It's therefore independent of the [innodb_io_capacity](#innodb_io_capacity) for the 1-second loop, but not entirely so for the 10-second loop. If set to `keep_average` or `2`, designed specifically for SSD cards, a shorter loop cycle is used in an attempt to keep the I/O rate constant.  Removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6 and replaced with InnoDB flushing method from MySQL 5.6.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-adaptive-flushing-method=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `estimate`
- <strong>Valid Values:</strong> `native` or `0`, `estimate` or `1`, `keep_average` or `2`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/) - replaced with InnoDB flushing method from MySQL 5.6

---

#### `innodb_adaptive_hash_index`

- <strong>Description:</strong> If set to `1`, the default until [MariaDB 10.5](/kb/en/what-is-mariadb-105/), the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) hash index is enabled. Based on performance testing ([MDEV-17492](https://jira.mariadb.org/browse/MDEV-17492)), the InnoDB adaptive hash index helps performance in mostly read-only workloads, and could slow down performance in other environments, especially [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/), [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table/), [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/), or [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index/) operations.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-adaptive-hash-index=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF` (&gt;= [MariaDB 10.5](/kb/en/what-is-mariadb-105/)), `ON` (&lt;= [MariaDB 10.4](/kb/en/what-is-mariadb-104/))

---

#### `innodb_adaptive_hash_index_partitions`

- <strong>Description:</strong> Specifies the number of partitions for use in adaptive searching. If set to `1`, no extra partitions are created. XtraDB-only. From [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB), this is an alias for [innodb_adaptive_hash_index_parts](#innodb_adaptive_hash_index_parts) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-adaptive-hash-index-partitions=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` to `64`

---

#### `innodb_adaptive_hash_index_parts`

- <strong>Description:</strong> Specifies the number of partitions for use in adaptive searching. If set to `1`, no extra partitions are created.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-adaptive-hash-index-parts=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8`
- <strong>Range:</strong> `1` to `512`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `innodb_adaptive_max_sleep_delay`

- <strong>Description:</strong> Maximum time in microseconds to automatically adjust the [innodb_thread_sleep_delay](#innodb_thread_sleep_delay) value to, based on the workload. Useful in extremely busy systems with hundreds of thousands of simultaneous connections. `0` disables any limit. Deprecated and ignored from [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-adaptive-max-sleep-delay=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> 
<ul><li>`0` (&gt;= [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/))
</li><li>`150000` (&lt;= [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/))
</li></ul>
- <strong>Range:</strong> `0` to `1000000`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Deprecated:</strong> [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_additional_mem_pool_size`

- <strong>Description:</strong> Size in bytes of the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) memory pool used for storing information about internal data structures. Defaults to 8MB, if your application has many tables and a large structure, and this is exceeded, operating system memory will be allocated and warning messages written to the error log, in which case you should increase this value. Deprecated in [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and removed in [MariaDB 10.2](/kb/en/what-is-mariadb-102/) along with InnoDB's internal memory allocator.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-additional-mem-pool-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8388608`
- <strong>Range:</strong> `2097152` to `4294967295`
- <strong>Deprecated:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Removed:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `innodb_api_bk_commit_interval`

- <strong>Description:</strong> Time in seconds between auto-commits for idle connections using the InnoDB memcached interface (not implemented in MariaDB).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-api-bk-commit-interval=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `5`
- <strong>Range:</strong> `1` to `1073741824`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Removed:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `innodb_api_disable_rowlock`

- <strong>Description:</strong> For use with MySQL's memcached (not implemented in MariaDB)
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-api-disable-rowlock=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Removed:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `innodb_api_enable_binlog`

- <strong>Description:</strong> For use with MySQL's memcached (not implemented in MariaDB)
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-api-enable-binlog=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Removed:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `innodb_api_enable_mdl`

- <strong>Description:</strong> For use with MySQL's memcached (not implemented in MariaDB)
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-api-enable-mdl=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Removed:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `innodb_api_trx_level`

- <strong>Description:</strong> For use with MySQL's memcached (not implemented in MariaDB)
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-api-trx-level=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Removed:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `innodb_auto_lru_dump`

- <strong>Description:</strong> Renamed [innodb_buffer_pool_restore_at_startup](#innodb_buffer_pool_restore_at_startup) since XtraDB 5.5.10-20.1, which was in turn replaced by [innodb_buffer_pool_load_at_startup](#innodb_buffer_pool_load_at_startup) in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-auto-lru-dump=#</code>
- <strong>Removed:</strong> XtraDB 5.5.10-20.1

---

#### `innodb_autoextend_increment`

- <strong>Description:</strong> Size in MB to increment an auto-extending shared tablespace file when it becomes full. If [innodb_file_per_table](#innodb_file_per_table) was set to `1`, this setting does not apply to the resulting per-table tablespace files, which are automatically extended in their own way.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-autoextend-increment=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `64` (from [MariaDB 10.0](/kb/en/what-is-mariadb-100/)) `8` (before [MariaDB 10.0](/kb/en/what-is-mariadb-100/)),
- <strong>Range:</strong> `1` to `1000`

---

#### `innodb_autoinc_lock_mode`

- <strong>Description:</strong> The lock mode that is used when generating [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) values for InnoDB tables. 
<ul start="1"><li>Valid values are:
<ul start="1"><li>`0` is the traditional lock mode.
</li><li>`1` is the consecutive lock mode.
</li><li>`2` is the interleaved lock mode.
</li></ul>
</li><li>In order to use [Galera Cluster](/kb/en/galera/), the lock mode needs to be set to `2`.
</li><li>See [AUTO_INCREMENT Handling in InnoDB: AUTO_INCREMENT Lock Modes](/kb/en/auto_increment-handling-in-xtradbinnodb/#auto_increment-lock-modes) for more information. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-autoinc-lock-mode=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>
<ul start="1"><li>`1`
</li></ul>
- <strong>Range:</strong> `0` to `2`

---

#### `innodb_background_scrub_data_check_interval`

- <strong>Description:</strong> Check if spaces needs scrubbing every [innodb_background_scrub_data_check_interval](#innodb_background_scrub_data_check_interval) seconds. See [Data Scrubbing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-data-scrubbing/). Deprecated and ignored from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-background-scrub-data-check-interval=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `3600`
- <strong>Range:</strong> `1` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_background_scrub_data_compressed`

- <strong>Description:</strong> Enable scrubbing of compressed data by background threads (same as encryption_threads). See [Data Scrubbing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-data-scrubbing/). Deprecated and ignored from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-background-scrub-data-compressed={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_background_scrub_data_interval`

- <strong>Description:</strong> Scrub spaces that were last scrubbed longer than this number of seconds ago. See [Data Scrubbing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-data-scrubbing/). Deprecated and ignored from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-background-scrub-data-interval=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `604800`
- <strong>Range:</strong> `1` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_background_scrub_data_uncompressed`

- <strong>Description:</strong> Enable scrubbing of uncompressed data by background threads (same as encryption_threads). See [Data Scrubbing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-data-scrubbing/). Deprecated and ignored from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-background-scrub-data-uncompressed={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_blocking_buffer_pool_restore`

- <strong>Description:</strong> If set to `1` (`0` is default), XtraDB will wait until the least-recently used (LRU) dump is completely restored upon restart before reporting back to the server that it has successfully started up. Available with XtraDB only, not InnoDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-blocking-buffer-pool-restore={1|2}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Removed:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_buf_dump_status_frequency`

- <strong>Description:</strong> Determines how often (as a percent) the buffer pool dump status should be printed in the logs. For example, `10` means that the buffer pool dump status is printed when every 10% of the number of buffer pool pages are dumped. The default is `0` (only start and end status is printed).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buf-dump-status-frequency=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `100`
- <strong>Introduced:</strong> [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)

---

#### `innodb_buffer_pool_chunk_size`

- <strong>Description:</strong> Chunk size used for dynamically resizing the [buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/). Note that changing this setting can change the size of the buffer pool. When [large-pages](/kb/en/server-system-variables/#large_pages) is used this value is effectively rounded up to the next multiple of [large-page-size](/kb/en/server-system-variables/#large_page_size). See [Setting Innodb Buffer Pool Size Dynamically](/replication/optimization-and-tuning/system-variables/setting-innodb-buffer-pool-size-dynamically/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buffer-pool-chunk-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `134217728`
- <strong>Range:</strong> `1048576` to [innodb_buffer_pool_size](#innodb_buffer_pool_size)/[innodb_buffer_pool_instances](#innodb_buffer_pool_instances)
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `innodb_buffer_pool_dump_at_shutdown`

- <strong>Description:</strong> Whether to record pages cached in the [buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/) on server shutdown, which reduces the length of the warmup the next time the server starts. The related [innodb_buffer_pool_load_at_startup](#innodb_buffer_pool_load_at_startup) specifies whether the buffer pool is automatically warmed up at startup.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buffer-pool-dump-at-shutdown=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong>
<ul start="1"><li>`ON` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/))
</li><li>`OFF` (&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_buffer_pool_dump_now`

- <strong>Description:</strong> Immediately records pages stored in the [buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/). The related [innodb_buffer_pool_load_now](#innodb_buffer_pool_load_now) does the reverse, and will immediately warm up the buffer pool.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buffer-pool-dump-now=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_buffer_pool_dump_pct`

- <strong>Description:</strong> Dump only the hottest N% of each [buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/), defaults to 100 until [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/). Since [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), defaults to 25% along with the changes to  [innodb_buffer_pool_dump_at_shutdown](#innodb_buffer_pool_dump_at_shutdown) and [innodb_buffer_pool_load_at_startup](#innodb_buffer_pool_load_at_startup).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buffer-pool-dump-pct=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong>
<ul start="1"><li>`25` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/))
</li><li>`100` (&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
</li></ul>
- <strong>Range:</strong> `1` to `100`
- <strong>Introduced:</strong> [MariaDB 10.1.10](/kb/en/mariadb-10110-release-notes/)

---

#### `innodb_buffer_pool_evict`

- <strong>Description:</strong> Evict pages from the buffer pool.  If set to "uncompressed" then all uncompressed pages are evicted from the buffer pool.  Variable to be used only for testing. Only exists in DEBUG builds.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buffer-pool-evict=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `""`
- <strong>Valid Values:</strong> "" or "uncompressed"

---

#### `innodb_buffer_pool_filename`

- <strong>Description:</strong> The file that holds the [buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/) list of page numbers set by [innodb_buffer_pool_dump_at_shutdown](#innodb_buffer_pool_dump_at_shutdown) and [innodb_buffer_pool_dump_now](#innodb_buffer_pool_dump_now).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buffer-pool-filename=file</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `ib_buffer_pool`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_buffer_pool_instances`

- <strong>Description:</strong> If [innodb_buffer_pool_size](#innodb_buffer_pool_size) is set to more than 1GB, innodb_buffer_pool_instances divides the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) buffer pool into this many instances. The default was 1 in [MariaDB 5.5](/kb/en/what-is-mariadb-55/), but for large systems with buffer pools of many gigabytes, many instances can help reduce contention concurrency. The default is 8 in MariaDB 10 (except on Windows 32-bit, where it varies according to [innodb_buffer_pool_size](#innodb_buffer_pool_size), or from [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), where it is set to 1 if [innodb_buffer_pool_size](#innodb_buffer_pool_size) &lt; 1GB). Each instance manages its own data structures and takes an equal portion of the total buffer pool size, so for example if innodb_buffer_pool_size is 4GB and innodb_buffer_pool_instances is set to 4, each instance will be 1GB. Each instance should ideally be at least 1GB in size. Deprecated and ignored from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/), as the original reasons for for splitting the buffer pool have mostly gone away.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buffer-pool-instances=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> &gt;= [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/):  `8`, `1` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) if [innodb_buffer_pool_size](#innodb_buffer_pool_size) &lt; 1GB), or dependent on [innodb_buffer_pool_size](#innodb_buffer_pool_size) (Windows 32-bit)
- <strong>Deprecated:</strong> [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_buffer_pool_load_abort`

- <strong>Description:</strong> Aborts the process of restoring [buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/) contents started by [innodb_buffer_pool_load_at_startup](#innodb_buffer_pool_load_at_startup) or [innodb_buffer_pool_load_now](#innodb_buffer_pool_load_now).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buffer-pool-load-abort=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_buffer_pool_load_at_startup`

- <strong>Description:</strong> Specifies whether the [buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/) is automatically warmed up when the server starts by loading the pages held earlier. The related [innodb_buffer_pool_dump_at_shutdown](#innodb_buffer_pool_dump_at_shutdown) specifies whether pages are saved at shutdown.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buffer-pool-load-at-startup=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong>
<ul start="1"><li>`ON` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/))
</li><li>`OFF` (&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_buffer_pool_load_now`

- <strong>Description:</strong> Immediately warms up the [buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/) by loading the stored data pages. The related [innodb_buffer_pool_dump_now](#innodb_buffer_pool_dump_now) does the reverse, and immediately records pages stored in the buffer pool.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buffer-pool-load-now=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_buffer_pool_load_pages_abort`

- <strong>Description:</strong> Number of pages during a buffer pool load to process before signaling [innodb_buffer_pool_load_abort=1](#innodb_buffer_pool_load_abort). Debug builds only.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buffer-pool-load-pages-abort=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `9223372036854775807`
- <strong>Range:</strong> `1` to `9223372036854775807`
- <strong>Introduced:</strong> [MariaDB 10.3](/kb/en/what-is-mariadb-103/)

---

#### `innodb_buffer_pool_populate`

- <strong>Description:</strong> When set to `1` (`0` is default), XtraDB will preallocate pages in the buffer pool on starting up so that NUMA allocation decisions are made while the buffer cache is still clean. XtraDB only. This option was made ineffective in [MariaDB 10.0.23](/kb/en/mariadb-10023-release-notes/). Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-buffer-pool-populate={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Deprecated:</strong> [MariaDB 10.0.23](/kb/en/mariadb-10023-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_buffer_pool_restore_at_startup`

- <strong>Description:</strong> Time in seconds between automatic buffer pool dumps. If set to a non-zero value, XtraDB will also perform an automatic restore of the [buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/) at startup. If set to `0`, automatic dumps are not performed, nor automatic restores on startup. Replaced by [innodb_buffer_pool_load_at_startup](#innodb_buffer_pool_load_at_startup) in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-buffer-pool-restore-at-startup</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range - 32 bit:</strong> `0` to `4294967295`
- <strong>Range - 64 bit:</strong> `0` to `18446744073709547520`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/) - replaced by [innodb_buffer_pool_load_at_startup](#innodb_buffer_pool_load_at_startup)

---

#### `innodb_buffer_pool_shm_checksum`

- <strong>Description:</strong> Used with Percona's SHM buffer pool patch in XtraDB 5.5. Was shortly deprecated and removed in XtraDB 5.6. XtraDB only.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-buffer-pool-shm-checksum={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_buffer_pool_shm_key`

- <strong>Description:</strong> Used with Percona's SHM buffer pool patch in XtraDB 5.5. Later deprecated in XtraDB 5.5, and removed in XtraDB 5.6.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-buffer-pool-shm-key={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_buffer_pool_size`

- <strong>Description:</strong> InnoDB buffer pool size in bytes. The primary value to adjust on a database server with entirely/primarily [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables, can be set up to 80% of the total memory in these environments. See the [InnoDB Buffer Pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/) for more on setting this variable, and also [Setting Innodb Buffer Pool Size Dynamically](/replication/optimization-and-tuning/system-variables/setting-innodb-buffer-pool-size-dynamically/) if doing so dynamically.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-buffer-pool-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)), No (&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `134217728` (128MB)
- <strong>Range:</strong> `5242880` (5MB) to `9223372036854775807` (8192PB)

---

#### `innodb_change_buffer_dump`

- <strong>Description:</strong> If set, causes the contents of the InnoDB change buffer to be dumped to the server error log at startup. Only available in debug builds.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.2.28](/kb/en/mariadb-10228-release-notes/), [MariaDB 10.3.19](/kb/en/mariadb-10319-release-notes/), [MariaDB 10.4.9](/kb/en/mariadb-1049-release-notes/)

---

#### `innodb_change_buffer_max_size`

- <strong>Description:</strong> Maximum size of the [InnoDB Change Buffer](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-change-buffering/) as a percentage of the total buffer pool. The default is 25%, and this can be increased up to 50% for servers with high write activity, and lowered down to 0 for servers used exclusively for reporting.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-change-buffer-max-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `25`
- <strong>Range:</strong> `0` to `50`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_change_buffering`

- <strong>Description:</strong> Sets how [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) change buffering is performed. See [InnoDB Change Buffering](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-change-buffering/) for details on the settings.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-change-buffering=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration` (&gt;= [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)), `string` (&lt;= [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/))
- <strong>Default Value:</strong> `all`
- <strong>Valid Values:</strong> `inserts`, `none`, `deletes`, `purges`, `changes`, `all`

---

#### `innodb_change_buffering_debug`

- <strong>Description:</strong> If set to `1`, an [InnoDB Change Buffering](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-change-buffering/) debug flag is set. `1` forces all changes to the change buffer, while `2` causes a crash at merge. `0`, the default, indicates no flag is set. Only available in debug builds.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-change-buffering-debug=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `2`

---

#### `innodb_checkpoint_age_target`

- <strong>Description:</strong> The maximum value of the checkpoint age. If set to `0`, has no effect. Removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6 and replaced with InnoDB flushing method from MySQL 5.6.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-checkpoint-age-target=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` upwards
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/) - replaced with InnoDB flushing method from MySQL 5.6.

---

#### `innodb_checksum_algorithm`

- <strong>Description:</strong> Specifies how the InnoDB tablespace checksum is generated and verified.
<ul start="1"><li>`innodb`: Backwards compatible with earlier versions (&lt;= [MariaDB 5.5](/kb/en/what-is-mariadb-55/)). Deprecated in [MariaDB 10.3.29](/kb/en/mariadb-10329-release-notes/), [MariaDB 10.4.19](/kb/en/mariadb-10419-release-notes/), [MariaDB 10.5.10](/kb/en/mariadb-10510-release-notes/) and removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/). If really needed, data files can still be converted with [innochecksum](/clients-utilities/innochecksum/).
</li><li>`crc32`: A newer, faster algorithm, but incompatible with earlier versions. Tablespace blocks will be converted to the new format over time, meaning that a mix of checksums may be present.
</li><li>`full_crc32` and `strict_full_crc32`: From [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/). Permits encryption to be supported over a [SPATIAL INDEX](/sql-statements-structure/geographic-geometric-features/spatial-index/), which `crc32` does not support. Newly-created data files will carry a flag that indicates that all pages of the file will use a full CRC-32C checksum over the entire page contents (excluding the bytes where the checksum is stored, at the very end of the page). Such files will always use that checksum, no matter what parameter `innodb_checksum_algorithm` is assigned to. Even if `innodb_checksum_algorithm` is modified later, the same checksum will continue to be used. A special flag will be set in the FSP_SPACE_FLAGS in the first data page to indicate the new format of checksum and encryption/page_compressed. ROW_FORMAT=COMPRESSED tables will only use the old format.
These tables do not support new features, such as larger innodb_page_size or instant ADD/DROP COLUMN. Also cleans up the MariaDB tablespace flags - flags are reserved to store the page_compressed compression algorithm, and to store the compressed payload length, so that checksum can be computed over the compressed (and possibly encrypted) stream and can be validated without decrypting or decompressing the page. In the full_crc32 format, there no longer are separate before-encryption and after-encryption checksums for pages. The single checksum is computed on the page contents that is written to the file.See [MDEV-12026](https://jira.mariadb.org/browse/MDEV-12026) for details.
</li><li>`none`: Writes a constant rather than calculate a checksum. Deprecated in [MariaDB 10.3.29](/kb/en/mariadb-10329-release-notes/), [MariaDB 10.4.19](/kb/en/mariadb-10419-release-notes/), [MariaDB 10.5.10](/kb/en/mariadb-10510-release-notes/) and removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/) as was mostly used to disable the original, slow, page checksum for benchmarketing purposes.
</li><li>`strict_crc32`, `strict_innodb` and `strict_none`: The options are the same as the regular options, but InnoDB will halt if it comes across a mix of checksum values. These are faster, as both new and old checksum values are not required, but can only be used when setting up tablespaces for the first time.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-checksum-algorithm=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong>
<ul start="1"><li>`full_crc32` (&gt;= [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/))
</li><li>`crc32` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/))
</li><li>`innodb` (&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
</li></ul>
- <strong>Valid Values:</strong> 
<ul start="1"><li>&gt;= [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/): `crc32`, `full_crc32`, `strict_crc32`, `strict_full_crc32`
</li><li>[MariaDB 10.5](/kb/en/what-is-mariadb-105/), &gt;= [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/): `innodb`, `crc32`, `full_crc32`, `none`, `strict_innodb`, `strict_crc32`, `strict_none`, `strict_full_crc32`
</li><li>&lt;= [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/): `innodb`, `crc32`, `none`, `strict_innodb`, `strict_crc32`, `strict_none`
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_checksums`

- <strong>Description:</strong> By default, [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) performs checksum validation on all pages read from disk, which provides extra fault tolerance. You would usually want this set to `1` in production environments, although setting it to `0` can provide marginal performance improvements. Deprecated and functionality replaced by [innodb_checksum_algorithm](#innodb_checksum_algorithm) in [MariaDB 10.0](/kb/en/what-is-mariadb-100/), and should be removed to avoid conflicts. `ON` is equivalent to <code class="fixed" style="white-space:pre-wrap">--innodb_checksum_algorithm=innodb</code> and `OFF` to <code class="fixed" style="white-space:pre-wrap">--innodb_checksum_algorithm=none</code>.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-checksums</code>, <code class="fixed" style="white-space:pre-wrap">--skip-innodb-checksums</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Deprecated:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Removed:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `innodb_cleaner_lsn_age_factor`

- <strong>Description:</strong> XtraDB has enhanced page cleaner heuristics, and with these in place, the default InnoDB adaptive flushing may be too aggressive. As a result, a new LSN age factor formula has been introduced, controlled by this variable. The default setting, `high_checkpoint`, uses the new formula, while the alternative, `legacy`, uses the original algorithm. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-cleaner-lsn-age-factor=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> 
<ul start="1"><li>`deprecated` (&gt;= [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/))
</li><li>`high_checkpoint` (&lt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/))
</li></ul>
- <strong>Valid Values:</strong> 
<ul><li>`high_checkpoint`, `legacy` (&lt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/))
</li><li>`deprecated`, `high_checkpoint`, `legacy` (&gt;= [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/))
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_cmp_per_index_enabled`

- <strong>Description:</strong> If set to `ON` (`OFF` is default), per-index compression statistics are stored in the [INFORMATION_SCHEMA.INNODB_CMP_PER_INDEX](/kb/en/information-schema-innodb-tables-information-schema-innodb_cmp_per_index-an/) table. These are expensive to record, so this setting should only be changed with care, such as for performance tuning on development or slave servers.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-cmp-per-index-enabled=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_commit_concurrency`

- <strong>Description:</strong> Limit to the number of transaction threads that can can commit simultaneously. 0, the default, imposes no limit. While you can change from one positive limit to another at runtime, you cannot set this variable to 0, or change it from 0, while the server is running. Deprecated and ignored from [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-commit-concurrency=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `1000`
- <strong>Deprecated:</strong> [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_compression_algorithm`

- <strong>Description:</strong> Compression algorithm used for [InnoDB page compression](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-compression/). The supported values are:
<ul><li>`none`: Pages are not compressed.
</li><li>`zlib`: Pages are compressed using the bundled <a undefined>zlib</a> compression algorithm.
</li><li>`lz4`: Pages are compressed using the <a undefined>lz4</a> compression algorithm.
</li><li>`lzo`: Pages are compressed using the <a undefined>lzo</a> compression algorithm.
</li><li>`lzma`: Pages are compressed using the <a undefined>lzma</a> compression algorithm.
</li><li>`bzip2`: Pages are compressed using the <a undefined>bzip2</a> compression algorithm.
</li><li>`snappy`: Pages are compressed using the <a undefined>snappy</a> algorithm.
</li><li>On many distributions, MariaDB may not support all page compression algorithms by default.
</li><li>See [InnoDB Page Compression: Configuring the InnoDB Page Compression Algorithm](/kb/en/innodb-page-compression/#configuring-the-innodb-page-compression-algorithm) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-compression-algorithm=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `zlib` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/), [MariaDB 10.1.22](/kb/en/mariadb-10122-release-notes/)), `none` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/), [MariaDB 10.1.21](/kb/en/mariadb-10121-release-notes/))
- <strong>Valid Values:</strong>`none`, `zlib`, `lz4`, `lzo`, `lzma`, `bzip2` or `snappy` ([MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/))
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `innodb_compression_default`

- <strong>Description:</strong> Whether or not [InnoDB page compression](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-compression/) is enabled by default for new tables.
<ul start="1"><li>The default value is `OFF`, which means new tables are not compressed.
</li><li>See [InnoDB Page Compression: Enabling InnoDB Page Compression by Default](/kb/en/innodb-page-compression/#enabling-innodb-page-compression-by-default) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-compression-default={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)

---

#### `innodb_compression_failure_threshold_pct`

- <strong>Description:</strong> Specifies the percentage cutoff for expensive compression failures during updates to a table that uses [InnoDB page compression](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-compression/), after which free space is added to each new compressed page, dynamically adjusted up to the level set by [innodb_compression_pad_pct_max](#innodb_compression_pad_pct_max). Zero disables checking of compression efficiency and adjusting padding.
<ul start="1"><li>See [InnoDB Page Compression: Configuring the Failure Threshold and Padding](/kb/en/innodb-page-compression/#configuring-the-failure-threshold-and-padding) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-compression-failure-threshold-pct=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `5`
- <strong>Range:</strong> `0` to `100`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_compression_level`

- <strong>Description:</strong> Specifies the default level of compression for tables that use [InnoDB page compression](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-compression/).
<ul start="1"><li>Only a subset of InnoDB page compression algorithms support compression levels. If an InnoDB page compression algorithm does not support compression levels, then the compression level value is ignored.
</li><li>The compression level can be set to any value between `1` and `9`. The default compression level is `6`. The range goes from the fastest to the most compact, which means that `1` is the fastest and `9` is the most compact.
</li><li>See [InnoDB Page Compression: Configuring the Default Compression Level](/kb/en/innodb-page-compression/#configuring-the-default-compression-level) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-compression-level=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `6`
- <strong>Range:</strong> `1` to `9`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_compression_pad_pct_max`

- <strong>Description:</strong> The maximum percentage of reserved free space within each compressed page for tables that use [InnoDB page compression](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-compression/). Reserved free space is used when the page's data is reorganized and might be recompressed. Only used when [innodb_compression_failure_threshold_pct](#innodb_compression_failure_threshold_pct) is not zero, and the rate of compression failures exceeds its setting.
<ul start="1"><li>See [InnoDB Page Compression: Configuring the Failure Threshold and Padding](/kb/en/innodb-page-compression/#configuring-the-failure-threshold-and-padding) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-compression-pad-pct-max=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `50`
- <strong>Range:</strong> `0` to `75`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_concurrency_tickets`

- <strong>Description:</strong> Number of times a newly-entered thread can enter and leave [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) until it is again subject to the limitations of [innodb_thread_concurrency](#innodb_thread_concurrency) and may possibly be queued. Deprecated and ignored from [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-concurrency-tickets=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> 
<ul start="1"><li>`0` (&gt;= [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/))
</li><li>`5000` (&lt;= [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/))
</li></ul>
- <strong>Range:</strong> `1` to `18446744073709551615`
- <strong>Deprecated:</strong> [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_corrupt_table_action`

- <strong>Description:</strong> What action to perform when a corrupt table is found. XtraDB only.
<ul start="1"><li>When set to `assert`, the default, XtraDB will intentionally crash the server when it detects corrupted data in a single-table tablespace, with an assertion failure.
</li><li>When set to `warn`, it will pass corruption as corrupt table instead of crashing, and disable all further I/O (except for deletion) on the table file.
</li><li>If set to `salvage`, read access is permitted, but corrupted pages are ignored. [innodb_file_per_table](#innodb_file_per_table) must be enabled for this option. Previously named `innodb_pass_corrupt_table`. 
</li><li>Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-corrupt-table-action=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> 
<ul start="1"><li>`assert` (&lt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/))
</li><li>`deprecated` (&lt;= [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/))
</li></ul>
- <strong>Valid Values:</strong> 
<ul start="1"><li>`deprecated`, `assert`, `warn`, `salvage` (&gt;= [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/))
</li><li>`assert`, `warn`, `salvage` (&lt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/))
</li></ul>
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_data_file_path`

- <strong>Description:</strong> Individual [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) data files, paths and sizes. The value of [innodb_data_home_dir](#innodb_data_home_dir) is joined to each path specified by innodb_data_file_path to get the full directory path. If innodb_data_home_dir is an empty string, absolute paths can be specified here. A file size is specified with K for kilobytes, M for megabytes and G for gigabytes, and whether or not to autoextend the data file is also specified.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-data-file-path=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `ibdata1:12M:autoextend` (from [MariaDB 10.0](/kb/en/what-is-mariadb-100/)), `ibdata1:10M:autoextend` (before [MariaDB 10.0](/kb/en/what-is-mariadb-100/))

---

#### `innodb_data_home_dir`

- <strong>Description:</strong> Directory path for all [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) data files in the shared tablespace (assuming [innodb_file_per_table](#innodb_file_per_table) is not enabled). File-specific information can be added in [innodb_data_file_path](#innodb_data_file_path), as well as absolute paths if innodb_data_home_dir is set to an empty string.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-data-home-dir=path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `directory name`
- <strong>Default Value:</strong> `The MariaDB data directory`

---

#### `innodb_deadlock_detect`

- <strong>Description:</strong> By default, the InnoDB deadlock detector is enabled. If set to off, deadlock detection is disabled and MariaDB will rely on [innodb_lock_wait_timeout](#innodb_lock_wait_timeout) instead. This may be more efficient in systems with high concurrency as deadlock detection can cause a bottleneck when a number of threads have to wait for the same lock.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-deadlock-detect</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`
- <strong>Introduced:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)

---

#### `innodb_default_page_encryption_key`

- <strong>Description:</strong> Encryption key used for page encryption.
<ul start="1"><li>See [Data-at-Rest Encryption](/kb/en/data-at-rest-encryption/) and [InnoDB Encryption Keys](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/innodb-encryption-keys/) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-default-page-encryption-key=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` to `255`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `innodb_default_encryption_key_id`

- <strong>Description:</strong> ID of encryption key used by default to encrypt InnoDB tablespaces.
<ul start="1"><li>See [Data-at-Rest Encryption](/kb/en/data-at-rest-encryption/) and [InnoDB Encryption Keys](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/innodb-encryption-keys/) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-default-encryption-key-id=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `innodb_default_row_format`

- <strong>Description:</strong> Specifies the default [row format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-row-formats-overview/) to be used for InnoDB tables. The compressed row format cannot be set as the default.
<ul start="1"><li>See [InnoDB Row Formats Overview: Default Row Format](/kb/en/innodb-row-formats-overview/#default-row-format) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-default-row-format=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `dynamic` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)), `compact` (&gt;= [MariaDB 10.1.32](/kb/en/mariadb-10132-release-notes/))
- <strong>Valid Values:</strong> `redundant`, `compact` or `dynamic`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), [MariadB 10.1.32](/kb/en/mariadb-10132-release-notes/)

---

#### `innodb_defragment`

- <strong>Description:</strong> When set to `1` (the default is `0`), InnoDB defragmentation is enabled. When set to FALSE, all existing defragmentation will be paused and new defragmentation commands will fail. Paused defragmentation commands will resume when this variable is set to true again. See [Defragmenting InnoDB Tablespaces](/replication/optimization-and-tuning/optimizing-tables/defragmenting-innodb-tablespaces/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-defragment=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `innodb_defragment_fill_factor`

- <strong>Description:</strong>. Indicates how full defragmentation should fill a page. Together with [innodb_defragment_fill_factor_n_recs](#innodb_defragment_fill_factor_n_recs) ensures defragmentation won’t pack the page too full and cause page split on the next insert on every page. The variable indicating more defragmentation gain is the one effective. See [Defragmenting InnoDB Tablespaces](/replication/optimization-and-tuning/optimizing-tables/defragmenting-innodb-tablespaces/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-defragment-fill-factor=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `double`
- <strong>Default Value:</strong> `0.9`
- <strong>Range:</strong> `0.7` to `1`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `innodb_defragment_fill_factor_n_recs`

- <strong>Description:</strong> Number of records of space that defragmentation should leave on the page. This variable, together with [innodb_defragment_fill_factor](#innodb_defragment_fill_factor), is introduced so defragmentation won't pack the page too full and cause page split on the next insert on every page. The variable indicating more defragmentation gain is the one effective. See [Defragmenting InnoDB Tablespaces](/replication/optimization-and-tuning/optimizing-tables/defragmenting-innodb-tablespaces/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-defragment-fill-factor-n-recs=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `20`
- <strong>Range:</strong> `1` to `100`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `innodb_defragment_frequency`

- <strong>Description:</strong> Maximum times per second for defragmenting a single index. This controls the number of times the defragmentation thread can request X_LOCK on an index. The defragmentation thread will check whether 1/defragment_frequency (s) has passed since it last worked on this index, and put the index back in the queue if not enough time has passed. The actual frequency can only be lower than this given number. See [Defragmenting InnoDB Tablespaces](/replication/optimization-and-tuning/optimizing-tables/defragmenting-innodb-tablespaces/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-defragment-frequency=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `integer`
- <strong>Default Value:</strong> `40`
- <strong>Range:</strong> `1` to `1000`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `innodb_defragment_n_pages`

- <strong>Description:</strong> Number of pages considered at once when merging multiple pages to defragment. See [Defragmenting InnoDB Tablespaces](/replication/optimization-and-tuning/optimizing-tables/defragmenting-innodb-tablespaces/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-defragment-n-pages=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `7`
- <strong>Range:</strong> `2` to `32`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `innodb_defragment_stats_accuracy`

- <strong>Description:</strong> Number of defragment stats changes there are before the stats are written to persistent storage. Defaults to zero, meaning disable defragment stats tracking. See [Defragmenting InnoDB Tablespaces](/replication/optimization-and-tuning/optimizing-tables/defragmenting-innodb-tablespaces/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-defragment-stats-accuracy=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `innodb_dict_size_limit`

- <strong>Description:</strong> Size in bytes of a soft limit the memory used by tables in the data dictionary. Once this limit is reached, XtraDB will attempt to remove unused entries. If set to `0`, the default and standard InnoDB behavior, there is no limit to memory usage. Removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6 and replaced by MySQL 5.6's new [table_definition_cache](/kb/en/server-system-variables/#table_definition_cache) implementation.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-dict-size-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Default Value - 32 bit:</strong> `2147483648`
- <strong>Default Value - 64 bit:</strong> `9223372036854775807`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/) - replaced by MySQL 5.6's new [table_definition_cache](/kb/en/server-system-variables/#table_definition_cache) implementation.

---

#### `innodb_disable_sort_file_cache`

- <strong>Description:</strong> If set to `1` (`0` is default), the operating system file system cache for merge-sort temporary files is disabled.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-disable-sort-file-cache=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_disallow_writes`

- <strong>Description:</strong> Tell InnoDB to stop any writes to disk.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `innodb_doublewrite`

- <strong>Description:</strong> If set to `1`, the default, to improve fault tolerance [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) first stores data to a [doublewrite buffer](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-doublewrite-buffer/) before writing it to data file. Disabling will provide a marginal peformance improvement.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-doublewrite</code>, <code class="fixed" style="white-space:pre-wrap">--skip-innodb-doublewrite</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `innodb_doublewrite_file`

- <strong>Description:</strong> The absolute or relative path and filename to a dedicated tablespace for the [doublewrite buffer](/kb/en/xtradbinnodb-doublewrite-buffer/). In heavy workloads, the doublewrite buffer can impact heavily on the server, and moving it to a different drive will reduce contention on random reads. Since the doublewrite buffer is mostly sequential writes, a traditional HDD is a better choice than SSD. This Percona XtraDB variable has not been ported to XtraDB 5.6.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-doublewrite-file=filename</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `filename`
- <strong>Default Value:</strong> `NULL`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_empty_free_list_algorithm`

- <strong>Description:</strong> XtraDB 5.6.13-61 introduced an algorithm to assist with reducing mutex contention when the buffer pool free list is empty, controlled by this variable. If set to `backoff`, the default until [MariaDB 10.1.24](/kb/en/mariadb-10124-release-notes/), the new algorithm will be used. If set to `legacy`, the original InnoDB algorithm will be used. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades. See [#1651657](https://bugs.launchpad.net/percona-server/+bug/1651657) for the reasons this was changed back to `legacy` in XtraDB 5.6.36-82.0. When upgrading from 10.0 to 10.1 (&gt;= 10.1.24), for large buffer pools the default will remain `backoff`, while for small ones it will be changed to `legacy`.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-empty-free-list-algorithm=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> 
<ul start="1"><li>`deprecated` (&gt;= [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/))
</li><li>`legacy` (&gt;= [MariaDB 10.1.24](/kb/en/mariadb-10124-release-notes/))
</li><li>`backoff` (&lt;= [MariaDB 10.1.23](/kb/en/mariadb-10123-release-notes/))
</li></ul>
- <strong>Valid Values:</strong> 
<ul start="1"><li>`deprecated`, `backoff`, `legacy` (&gt;= [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/))
</li><li>`backoff`, `legacy` (&lt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/))
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_enable_unsafe_group_commit`

- <strong>Description:</strong> Unneeded after XtraDB 1.0.5. If set to `0`, the default, InnoDB will keep transactions between the transaction log and [binary log](/mariadb-administration/server-monitoring-logs/binary-log/)s in the same order. Safer, but slower. If set to `1`, transactions can be group-committed, but there is no guarantee of the order being kept, and a small risk of the two logs getting out of sync. In write-intensive environments, can lead to a significant improvement in performance.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-enable-unsafe-group-commit</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `1`
- <strong>Removed:</strong> Not needed after XtraDB 1.0.5

---

#### `innodb_encrypt_log`

- <strong>Description:</strong> Enables encryption of the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/). This also enables encryption of some temporary files created internally by InnoDB, such as those used for merge sorts and row logs.
<ul start="1"><li>See [Data-at-Rest Encryption](/kb/en/data-at-rest-encryption/) and [InnoDB / XtraDB Enabling Encryption: Enabling Encryption for Redo Log](/kb/en/innodb-enabling-encryption/#enabling-encryption-for-the-redo-log) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-encrypt-log</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `innodb_encrypt_tables`

- <strong>Description:</strong> Enables automatic encryption of all InnoDB tablespaces.
<ul start="1"><li>`OFF` - Disables table encryption for all new and existing tables that have the <a undefined>ENCRYPTED</a> table option set to `DEFAULT`.
</li><li>`ON` - Enables table encryption for all new and existing tables that have the <a undefined>ENCRYPTED</a> table option set to `DEFAULT`, but allows unencrypted tables to be created.
</li><li>`FORCE` - Enables table encryption for all new and existing tables that have the <a undefined>ENCRYPTED</a> table option set to `DEFAULT`, and doesn't allow unencrypted tables to be created (CREATE TABLE ... ENCRYPTED=NO will fail). 
</li><li>See [Data-at-Rest Encryption](/kb/en/data-at-rest-encryption/) and [InnoDB / XtraDB Enabling Encryption: Enabling Encryption for Automatically Encrypted Tablespaces](/kb/en/innodb-enabling-encryption/#enabling-encryption-for-automatically-encrypted-tablespaces) for more information. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-encrypt-tables=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Valid Values:</strong> `ON`, `OFF`, `FORCE` (from [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/))
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `innodb_encrypt_temporary_tables`

- <strong>Description:</strong> Enables automatic encryption of the InnoDB [temporary tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-temporary-tablespaces/).
<ul start="1"><li>See [Data-at-Rest Encryption](/kb/en/data-at-rest-encryption/) and [InnoDB / XtraDB Enabling Encryption: Enabling Encryption for Temporary Tablespaces](/kb/en/innodb-enabling-encryption/#enabling-encryption-for-temporary-tablespaces) for more information. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-encrypt-temporary-tables=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Valid Values:</strong> `ON`, `OFF`
- <strong>Introduced:</strong> [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/)

---

#### `innodb_encryption_rotate_key_age`

- <strong>Description:</strong> Re-encrypt in background any page having a key older than this number of key versions. When setting up encryption, this variable must be set to a non-zero value.  Otherwise, when you enable encryption through <a undefined>innodb_encrypt_tables</a> MariaDB won't be able to automatically encrypt any unencrypted tables.
<ul start="1"><li>See [Data-at-Rest Encryption](/kb/en/data-at-rest-encryption/) and [InnoDB / XtraDB Encryption Keys: Key Rotation](/kb/en/innodb-xtradb-encryption-keys/#key-rotation) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-encryption-rotate-key-age=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `innodb_encryption_rotation_iops`

- <strong>Description:</strong> Use this many iops for background key rotation operations performed by the background encryption threads.
<ul start="1"><li>See [Data-at-Rest Encryption](/kb/en/data-at-rest-encryption/) and [InnoDB / XtraDB Encryption Keys: Key Rotation](/kb/en/innodb-xtradb-encryption-keys/#key-rotation) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-encryption-rotation_iops=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `innodb_encryption_threads`

- <strong>Description:</strong> Number of background encryption threads threads performing background key rotation and [scrubbing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-data-scrubbing/). When setting up encryption, this variable must be set to a non-zero value. Otherwise, when you enable encryption through [innodb_encrypt_tables](#innodb_encrypt_tables) MariaDB won't be able to automatically encrypt any unencrypted tables. Recommended never be set higher than 255.
<ul start="1"><li>See [Data-at-Rest Encryption](/kb/en/data-at-rest-encryption/) and [InnoDB Background Encryption Threads](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/innodb-background-encryption-threads/) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-encryption-threads=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> 
<ul><li>`0` to `4294967295`  (&lt;= [MariaDB 10.1.45](/kb/en/mariadb-10145-release-notes/), [MariaDB 10.2.32](/kb/en/mariadb-10232-release-notes/), [MariaDB 10.3.23](/kb/en/mariadb-10323-release-notes/), [MariaDB 10.4.13](/kb/en/mariadb-10413-release-notes/), [MariaDB 10.5.3](/kb/en/mariadb-1053-release-notes/)) 
</li><li>`0` to `255`  (&gt;=  [MariaDB 10.1.46](/kb/en/mariadb-10146-release-notes/), [MariaDB 10.2.33](/kb/en/mariadb-10233-release-notes/), [MariaDB 10.3.24](/kb/en/mariadb-10324-release-notes/), [MariaDB 10.4.14](/kb/en/mariadb-10414-release-notes/), [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/))
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `innodb_extra_rsegments`

- <strong>Description:</strong> Removed in XtraDB 5.5 and replaced by [innodb_rollback_segments](#innodb_rollback_segments). Usually there is one rollback segment protected by  single mutex, a source of contention in high write environments. This option specifies a number of extra user rollback segments. Changing the default will make the data readable by XtraDB only, and is incompatible with InnoDB. After modifying, the server must be slow-shutdown. If there is existing data, it must be dumped before changing, and re-imported after the change has taken effect.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-extra-rsegments=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `126`
- <strong>Removed:</strong> XtraDB 5.5 - replaced by [innodb_rollback_segments](#innodb_rollback_segments)

---

#### `innodb_extra_undoslots`

- <strong>Description:</strong> Usually, InnoDB has 1024 undo slots in its rollback segment, so 1024 transactions can run in parallel. New transactions will fail if all slots are used. Setting this variable to `1` expands the available undo slots to 4072. Not recommended unless you get the `Warning: cannot find a free slot for an undo log` error in the error log, as it makes data files unusable for ibbackup, or MariaDB servers not run with this option. See also [undo log](/kb/en/undo-log/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-extra-undoslots=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Removed:</strong> XtraDB 5.5

---

#### `innodb_fake_changes`

- <strong>Description:</strong> From [MariaDB 5.5](/kb/en/what-is-mariadb-55/) until [MariaDB 10.1](/kb/en/what-is-mariadb-101/), XtraDB-only option that enables the fake changes feature. In [replication](/replication/), setting up or restarting a slave can cause a replication reads to perform more slowly, as MariaDB is single-threaded and needs to read the data before it can execute the queries. This can be speeded up by prefetching threads to warm the server, replaying the statements and then rolling back at commit. This however has an overhead from locking rows only then to undo changes at rollback. Fake changes attempts to reduce this overhead by reading the rows for INSERT, UPDATE and DELETE statements but not updating them. The rollback is then very fast with little or nothing to do. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades. Not present in [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and beyond.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-fake-changes={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_fast_checksum`

- <strong>Description:</strong> Implements a more CPU efficient XtraDB checksum algorithm, useful for write-heavy loads with high I/O. If set to `1` on a server with tables that have been created with it set to `0`, reads will be slower, so tables should be recreated (dumped and reloaded). XtraDB will fail to start if set to `0` and there are tables created while set to `1`. Replaced with [innodb_checksum_algorithm](#innodb_checksum_algorithm) in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-fast-checksum={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6 - replaced with [innodb_checksum_algorithm](#innodb_checksum_algorithm)

---

#### `innodb_fast_shutdown`

- <strong>Description:</strong> The shutdown mode. 
<ul start="1"><li>`0` -  InnoDB performs a slow shutdown, including full purge (before [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/), not always, due to [MDEV-13603](https://jira.mariadb.org/browse/MDEV-13603)) and change buffer merge. Can be very slow, even taking hours in extreme cases.
</li><li>`1` - the default, [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) performs a fast shutdown, not performing a full purge or an insert buffer merge. 
</li><li>`2`, the [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) is flushed and a cold shutdown takes place, similar to a crash. The resulting startup then performs crash recovery. Extremely fast, in cases of emergency, but risks corruption. Not suitable for upgrades between major versions!
</li><li>`3` (from [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/)) - active transactions will not be rolled back, but all changed pages will be written to data files. The active transactions will be rolled back by a background thread on a subsequent startup.  The fastest option that will not involve [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) apply on subsequent startup. See [MDEV-15832](https://jira.mariadb.org/browse/MDEV-15832).
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-fast-shutdown[=#]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range: </strong> `0` to `3` (&gt;= [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/)), `0` to `2` (&lt;= [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/))

---

#### `innodb_fatal_semaphore_wait_threshold`

- <strong>Description:</strong> In MariaDB, the fatal semaphore timeout is configurable. This variable sets the maximum number of seconds for semaphores to time out in InnoDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-fatal-semaphore-wait-threshold=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `600`
- <strong>Range:</strong> `1` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

#### `innodb_file_format`

- <strong>Description:</strong> File format for new [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables. Can either be `Antelope`, the default and the original format, or `Barracuda`, which supports [compression](/kb/en/compression/). Note that this value is also used when a table is re-created with an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) which requires a table copy. See [XtraDB/InnoDB File Format](/kb/en/xtradbinnodb-file-format/) for more on the file formats. Removed in 10.3.1 and restored as a deprecated and unused variable in 10.4.3 for compatibility purposes.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-file-format=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong>
<ul start="1"><li>`Barracuda` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/))
</li><li>`Antelope` (&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
</li></ul>
- <strong>Valid Values:</strong> `Antelope`, `Barracuda`
- <strong>Deprecated:</strong> [MariaDB 10.2](/kb/en/what-is-mariadb-102/)
- <strong>Removed:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)
- <strong>Re-introduced:</strong> [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) (for compatibility purposes)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_file_format_check`

- <strong>Description:</strong> If set to `1`, the default, [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) checks the shared tablespace file format tag. If this is higher than the current version supported by XtraDB/InnoDB (for example Barracuda when only Antelope is supported), XtraDB/InnoDB will will not start. If it the value is not higher, XtraDB/InnoDB starts correctly and the [innodb_file_format_max](#innodb_file_format_max) value is set to this value. If innodb_file_format_check is set to `0`, no checking is performed. See [XtraDB/InnoDB File Format](/kb/en/xtradbinnodb-file-format/) for more on the file formats.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-file-format-check=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Deprecated:</strong> [MariaDB 10.2](/kb/en/what-is-mariadb-102/)
- <strong>Removed:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)

---

#### `innodb_file_format_max`

- <strong>Description:</strong> The highest [XtraDB/InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) file format. This is set to the value of the file format tag in the shared tablespace on startup (see [innodb_file_format_check](#innodb_file_format_check)). If the server later creates a higher table format, innodb_file_format_max is set to that value. See [XtraDB/InnoDB File Format](/kb/en/xtradbinnodb-file-format/) for more on the file formats.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-file-format-max=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `Antelope`
- <strong>Valid Values:</strong> `Antelope`, `Barracuda`
- <strong>Deprecated:</strong> [MariaDB 10.2](/kb/en/what-is-mariadb-102/)
- <strong>Removed:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)

---

#### `innodb_file_per_table`

- <strong>Description:</strong> If set to `ON`, then new [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables are created with their own [InnoDB file-per-table tablespaces](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/). If set to `OFF`, then new tables are created in the [InnoDB system tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/) instead. [Page compression](/kb/en/compression/) is only available with file-per-table tablespaces. Note that this value is also used when a table is re-created with an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) which requires a table copy.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-file-per-table</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `innodb_fill_factor`

- <strong>Description:</strong> Percentage of B-tree page filled during bulk insert (sorted index build). Used as a hint rather than an absolute value. Setting to `70`, for example, reserves 30% of the space on each B-tree page for the index to grow in future.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-fill-factor=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value</strong>: `100`
- <strong>Range:</strong> `10` to `100`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `innodb_flush_log_at_timeout`

- <strong>Description:</strong> Interval in seconds to write and flush the [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/). Before MariaDB 10, this was fixed at one second, which is still the default, but this can now be changed. It's usually increased to reduce flushing and avoid impacting performance of binary log group commit.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `2700`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_flush_log_at_trx_commit`

- <strong>Description:</strong> Set to `1`, along with [sync_binlog=1](/kb/en/replication-and-binary-log-server-system-variables/#sync_binlog) for the greatest level of fault tolerance. The value of [innodb_use_global_flush_log_at_trx_commit](#innodb_use_global_flush_log_at_trx_commit) determines whether this variable can be reset with a SET statement or not.
<ul start="1"><li>`1` The default, the log buffer is written to the [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) file and a flush to disk performed after each transaction. This is required for full ACID compliance. 
</li><li>`0` Nothing is done on commit; rather the log buffer is written and flushed to the [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) once a second. This gives better performance, but a server crash can erase the last second of transactions. 
</li><li>`2` The log buffer is written to the [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) after each commit, but flushing takes place once a second. Performance is slightly better, but a OS or power outage can cause the last second's transactions to be lost. 
</li><li>`3` (from [MariaDB 10.0](/kb/en/what-is-mariadb-100/)) Emulates [MariaDB 5.5](/kb/en/what-is-mariadb-55/) [group commit](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/) (3 syncs per group commit). See [Binlog group commit and innodb_flush_log_at_trx_commit](/kb/en/binlog-group-commit-and-innodb_flush_log_at_trx_commit/).
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-flush-log-at-trx-commit[=#]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `1`
- <strong>Valid Values:</strong> `0`, `1`, `2` or `3` (from [MariaDB 10.0](/kb/en/what-is-mariadb-100/))

---

#### `innodb_flush_method`

- <strong>Description:</strong> [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) flushing method. Windows always uses async_unbuffered and this variable then has no effect. On Unix, before [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/), by default fsync() is used to flush data and logs. Adjusting this variable can give performance improvements, but behavior differs widely on different filesystems, and changing from the default has caused problems in some situations, so test and benchmark carefully before adjusting. In MariaDB, Windows recognises and correctly handles the Unix methods, but if none are specified it uses own default - unbuffered write (analog of O_DIRECT) + syncs (e.g FileFlushBuffers()) for all files.
<ul start="1"><li>`O_DSYNC` - O_DSYNC is used to open and flush logs, and fsync() to flush the data files.
</li><li>`O_DIRECT` - O_DIRECT or directio(), is used to open data files, and fsync() to flush data and logs. Default on Unix from [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/).
</li><li>`fsync`  - Default on Unix until [MariaDB 10.5](/kb/en/what-is-mariadb-105/). Can be specified directly, but if the variable is unset on Unix, fsync() will be used by default.
</li><li>`O_DIRECT_NO_FSYNC` - introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/). Uses O_DIRECT during flushing I/O, but skips fsync() afterwards. Not suitable for XFS filesystems.
</li><li>`ALL_O_DIRECT` - introduced in [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and available with XtraDB only. Uses O_DIRECT for opening both data and logs and fsync() to flush data but not logs. Use with large InnoDB files only, otherwise may cause a performance degradation. Set [innodb_log_block_size](#innodb_log_block_size) to 4096 on ext4 filesystems. This is the default log block size on ext4 and will avoid unaligned AIO/DIO warnings.
</li><li>`unbuffered` - Windows-only default
</li><li>`async_unbuffered` - Windows-only, alias for `unbuffered`
</li><li>`normal` - Windows-only, alias for `fsync`
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-flush-method=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enumeration` (&gt;= [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)), `string` (&lt;= [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/))
- <strong>Default Value:</strong> 
<ul start="1"><li>`O_DIRECT` (Unix, &gt;= [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/))
</li><li>`fsync` (Unix, &gt;= [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/), &lt;= [MariaDB 10.5](/kb/en/what-is-mariadb-105/))
</li><li>Not set (&lt;= [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/))
</li></ul>
- <strong>Valid Values:</strong> 
<ul start="1"><li>Unix: `fsync`, `O_DSYNC`, `O_DIRECT`, `O_DIRECT_NO_FSYNC` (&gt;=[MariaDB 10.0](/kb/en/what-is-mariadb-100/)), `ALL_O_DIRECT` (&gt;= [MariaDB 5.5](/kb/en/what-is-mariadb-55/) to &lt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/), XtraDB only)
</li><li>Windows: `unbuffered`, `async_unbuffered`, `normal`
</li></ul>

---

#### `innodb_flush_neighbor_pages`

- <strong>Description:</strong> Determines whether, when dirty pages are flushed to the data file, neighboring pages in the data file are flushed at the same time. If set to `none`, the feature is disabled. If set to `area`, the default, the standard InnoDB behavior is used. For each page to be flushed, dirty neighboring pages are flushed too. If there's little head seek delay, such as SSD or large enough write buffer, one of the other two options may be more efficient. If set to `cont`, for each page to be flushed, neighboring contiguous blocks are flushed at the same time. Being contiguous, a sequential I/O is used, unlike the random I/O used in `area`. Replaced by [innodb_flush_neighbors](#innodb_flush_neighbors) in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-flush-neighbor-pages=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `area`
- <strong>Valid Values:</strong> `none` or `0`, `area` or `1`, `cont` or `2`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6 - replaced by [innodb_flush_neighbors](#innodb_flush_neighbors)

---

#### `innodb_flush_neighbors`

- <strong>Description:</strong>  Determines whether flushing a page from the [buffer pool](/kb/en/xtradbinnodb-memory-buffer/) will flush other dirty pages in the same group of pages (extent). In high write environments, if flushing is not aggressive enough, it can fall behind  resulting in higher memory usage, or if flushing is too aggressive, cause excess I/O activity. SSD devices, with low seek times, would be less likely to require dirty neighbor flushing to be set.
<ul start="1"><li>`1`: The default, flushes contiguous dirty pages in the same extent from the buffer pool.
</li><li>`0`: No other dirty pages are flushed.
</li><li>`2`: Flushes dirty pages in the same extent from the buffer pool.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-flush-neighbors=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `1`
- <strong>Valid Values:</strong> `0`, `1`, `2`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `innodb_flush_sync`

- <strong>Description:</strong> If set to `ON`, the default,  the [innodb_io_capacity](#innodb_io_capacity) setting is ignored for I/O bursts occuring at checkpoints.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-flush-sync={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value</strong>: `ON`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `innodb_flushing_avg_loops`

- <strong>Description:</strong> Determines how quickly adaptive flushing will respond to changing workloads. The value is the number of iterations that a previously calculated flushing state snapshot is kept. Increasing the value smooths and slows the rate that the flushing operations change, while decreasing it causes flushing activity to spike quickly in response to workload changes.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-flushing-avg-loops=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `30`
- <strong>Range:</strong> `1` to `1000`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `innodb_force_load_corrupted`

- <strong>Description:</strong> Set to `0` by default, if set to `1`, [XtraDB/InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) will be permitted to load tables marked as corrupt. Only use this to recover data you can't recover any other way, or in troubleshooting. Always restore to `0` when the returning to regular use.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-force-load-corrupted</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `innodb_force_primary_key`

- <strong>Description:</strong> If set to `1` (`0` is default) CREATE TABLEs without a primary or unique key where all keyparts are NOT NULL will not be accepted, and will return an error.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-force-primary-key</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `innodb_force_recovery`

- <strong>Description:</strong> [XtraDB/InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) crash recovery mode. `0` is the default. The other modes are for recovery purposes only, and no data can be changed while another mode is active. Some queries relying on indexes are also blocked. See [XtraDB/InnoDB Recovery Modes](/kb/en/xtradbinnodb-recovery-modes/) for more on mode specifics.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-force-recovery=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `6`

---

#### `innodb_foreground_preflush`

- <strong>Description:</strong> Before XtraDB 5.6.13-61.0, if the checkpoint age is in the sync preflush zone while a thread is writing to the [XtraDB redo log](/kb/en/xtradbinnodb-redo-log/), it will try to advance the checkpoint by issuing a flush list flush batch if this is not already being done. XtraDB has enhanced page cleaner tuning, and  may already be performing furious flushing, resulting in the flush simply adding unneeded mutex pressure. Instead, the thread now waits for the flushes to finish, and then has two options, controlled by this variable. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
<ul start="1"><li>`exponential_backoff` - thread sleeps while it waits for the flush list flush to occur. The sleep time randomly progressively increases, periodically reset to avoid runaway sleeps. 
</li><li>`sync_preflush` - thread issues a flush list batch, and waits for it to complete. This is the same as is used when the page cleaner thread is not running.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-foreground-preflush=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong>
<ul start="1"><li>`deprecated` (&gt;= [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/))
</li><li>`exponential_backoff` (&lt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/))
</li></ul>
- <strong>Valid Values:</strong> 
<ul start="1"><li>`deprecated`, `exponential_backoff`, `sync_preflush` (&gt;= [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/))
</li><li>`exponential_backoff`, `sync_preflush` (&lt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/))
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_ft_aux_table`

- <strong>Description:</strong> Diagnostic variable intended only to be set at runtime. It specifies the qualified name (for example `test/ft_innodb`) of an InnoDB table that has a [FULLTEXT index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/), and after being set the INFORMATION_SCHEMA tables [INNODB_FT_INDEX_TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_ft_index_table-table/), [INNODB_FT_INDEX_CACHE](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_ft_index_cache-table/), INNODB_FT_CONFIG, [INNODB_FT_DELETED](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_ft_deleted-table/), and [INNODB_FT_BEING_DELETED](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_ft_being_deleted-table/) will contain search index information for the specified table.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-ft-aux-table=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_ft_cache_size`

- <strong>Description:</strong> Cache size available for a parsed document while creating an InnoDB [FULLTEXT index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-ft-cache-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8000000`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_ft_enable_diag_print`

- <strong>Description:</strong> If set to `1`, additional [full-text](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) search diagnostic output is enabled.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-ft-enable-diag-print=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_ft_enable_stopword`

- <strong>Description:</strong> If set to `1`, the default, a set of [stopwords](/kb/en/stopwords/) is associated with an InnoDB [FULLTEXT index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) when it is created. The stopword list comes from the table set by the session variable [innodb_ft_user_stopword_table](#innodb_ft_user_stopword_table), if set, otherwise the global variable [innodb_ft_server_stopword_table](#innodb_ft_server_stopword_table), if that is set, or the [built-in list](/kb/en/stopwords/) if neither variable is set.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-ft-enable-stopword=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_ft_max_token_size`

- <strong>Description:</strong> Maximum length of words stored in an InnoDB [FULLTEXT index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/). A larger limit will increase the size of the index, slowing down queries, but permit longer words to be searched for. In most normal situations, longer words are unlikely search terms.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-ft-max-token-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `84`
- <strong>Range:</strong> `10` to `252`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_ft_min_token_size`

- <strong>Description:</strong> Minimum length of words stored in an InnoDB [FULLTEXT index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/). A smaller limit will increase the size of the index, slowing down queries, but permit shorter words to be searched for. For data stored in a Chinese, Japanese or Korean [character set](/kb/en/data-types-character-sets-and-collations/), a value of 1 should be specified to preserve functionality.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-ft-min-token-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `3`
- <strong>Range:</strong> `0` to `16`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_ft_num_word_optimize`

- <strong>Description:</strong>  Number of words processed during each [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/) on an InnoDB [FULLTEXT index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/). To ensure all changes are incorporated, multiple OPTIMIZE TABLE statements could be run in case of a substantial change to the index.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-ft-num-word-optimize=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `2000`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_ft_result_cache_limit`

- <strong>Description:</strong> Limit in bytes of the InnoDB [FULLTEXT index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) query result cache per fulltext query. The latter stages of the full-text search are handled in memory, and limiting this prevents excess memory usage. If the limit is exceeded, the query returns an error.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-ft-result-cache-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `2000000000`
- <strong>Range:</strong> `1000000` to `4294967295` (&lt;= [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/), [MariaDB 10.1.36](/kb/en/mariadb-10136-release-notes/), [MariaDB 10.0.36](/kb/en/mariadb-10036-release-notes/))
- <strong>Range:</strong> `1000000` to `18446744073709551615` (64-bit, &gt;= [MariaDB 10.2.19](/kb/en/mariadb-10219-release-notes/), [MariaDB 10.1.37](/kb/en/mariadb-10137-release-notes/), [MariaDB 10.0.37](/kb/en/mariadb-10037-release-notes/))
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)

---

#### `innodb_ft_server_stopword_table`

- <strong>Description:</strong> Table name containing a list of stopwords to ignore when creating an InnoDB [FULLTEXT index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/), in the format db_name/table_name. The specified table must exist before this option is set, and must be an InnoDB table with a single column, a [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/) named VALUE. See also [innodb_ft_enable_stopword](#innodb_ft_enable_stopword).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-ft-server-stopword-table=db_name/table_name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> Empty
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_ft_sort_pll_degree`

- <strong>Description:</strong>  Number of parallel threads used when building an InnoDB [FULLTEXT index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/). See also [innodb_sort_buffer_size](#innodb_sort_buffer_size).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-ft-sort-pll-degree=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `2`
- <strong>Range:</strong> `1` to `32`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_ft_total_cache_size`

- <strong>Description:</strong>Total memory allocated for the cache for all InnoDB [FULLTEXT index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) tables. A force sync is triggered if this limit is exceeded.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-ft-total-cache-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `640000000`
- <strong>Range:</strong> `32000000` to `1600000000`
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)

---

#### `innodb_ft_user_stopword_table`

- <strong>Description:</strong> Table name containing a list of stopwords to ignore when creating an InnoDB [FULLTEXT index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/), in the format db_name/table_name. The specified table must exist before this option is set, and must be an InnoDB table with a single column, a [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/) named VALUE. See also [innodb_ft_enable_stopword](#innodb_ft_enable_stopword).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-ft-user-stopword-table=db_name/table_name</code>
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> Empty
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_ibuf_accel_rate`

- <strong>Description:</strong> Allows the insert buffer activity to be adjusted. The following formula is used: [real activity] = [default activity] * (innodb_io_capacity/100) * (innodb_ibuf_accel_rate/100). As `innodb_ibuf_accel_rate` is increased from its default value of `100`, the lowest setting, insert buffer activity is increased. See also [innodb_io_capacity](#innodb_io_capacity). This Percona XtraDB variable has not been ported to XtraDB 5.6.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-ibuf-accel-rate=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100`
- <strong>Range:</strong> `100` to `999999999`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_ibuf_active_contract`

- <strong>Description:</strong> Specifies whether the insert buffer can be processed before it's full. If set to `0`, the standard InnoDB method is used, and the buffer is not processed until it's full. If set to `1`, the default, the insert buffer can be processed before it is full. This Percona XtraDB variable has not been ported to XtraDB 5.6.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-ibuf-active-contract=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `1`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_ibuf_max_size`

- <strong>Description:</strong> Maximum size in bytes of the insert buffer. Defaults to half the size of the [buffer pool](/kb/en/xtradbinnodb-memory-buffer/) so you may want to reduce if you have a very large buffer pool. If set to `0`, the insert buffer is disabled, which will cause all secondary index updates to be performed synchronously, usually at a cost to performance. This Percona XtraDB variable has not been ported to XtraDB 5.6.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-ibuf-max-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> 1/2 the size of the InnoDB buffer pool
- <strong>Range:</strong> `0` to 1/2 the size of the InnoDB buffer pool
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_idle_flush_pct`

- <strong>Description:</strong> Up to what percentage of dirty pages should be flushed when innodb finds it has spare resources to do so. Has had no effect since merging InnoDB 5.7 from mysql-5.7.9 ([MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)). Deprecated in [MariaDB 10.2.37](/kb/en/mariadb-10237-release-notes/), [MariaDB 10.3.28](/kb/en/mariadb-10328-release-notes/), [MariaDB 10.4.18](/kb/en/mariadb-10418-release-notes/) and removed in [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-idle-flush-pct=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100`
- <strong>Range:</strong> `0` to `100`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.37](/kb/en/mariadb-10237-release-notes/), [MariaDB 10.3.28](/kb/en/mariadb-10328-release-notes/), [MariaDB 10.4.18](/kb/en/mariadb-10418-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/)

---

#### `innodb_immediate_scrub_data_uncompressed`

- <strong>Description:</strong> Enable scrubbing of data. See [Data Scrubbing](/kb/en/xtradbinnodb-data-scrubbing/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-immediate-scrub-data-uncompressed=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `innodb_import_table_from_xtrabackup`

- <strong>Description:</strong> If set to `1`, permits importing of .ibd files exported with the [XtraBackup](/kb/en/backup-restore-and-import-xtrabackup/) --export option. Previously named `innodb_expand_import`. Removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6 and replaced with MySQL 5.6's transportable tablespaces.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-import-table-from-xtrabackup=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `1`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_instant_alter_column_allowed`

- <strong>Description:</strong> 
<ul start="1"><li>If a table is altered using ALGORITHM=INSTANT, it can force the table to use a non-canonical
format: A hidden metadata record at the start of the clustered index is used to store each column's DEFAULT value. This makes it possible to add new columns that have default values without rebuilding the table. Starting with [MariaDB 10.4](/kb/en/what-is-mariadb-104/), a BLOB in the hidden metadata record is used to store column mappings. This makes
it possible to drop or reorder columns without rebuilding the table. This also makes it possible to add columns to any position or drop columns from any position in the table without rebuilding the table. If a column is dropped without rebuilding the table, old records will contain garbage in that column's former position, and new records
will be written with NULL values, empty strings, or dummy values. 
</li><li>This is generally not a problem. However, there may be cases where
you want to avoid putting a table into this format.
For example, to ensure that future UPDATE operations
after an ADD COLUMN will be performed in-place, to reduce write
amplification. (Instantly added columns are essentially always
variable-length.) Also avoid bugs similar to
[MDEV-19916](https://jira.mariadb.org/browse/MDEV-19916), or to be able to export tables to
older versions of the server.
</li><li>This variable has been introduced as a result, with the following values:
</li><li>`never` (0): Do not allow instant add/drop/reorder,
to maintain format compatibility with MariaDB 10.x and MySQL 5.x.
If the table (or partition) is not in the canonical format, then
any ALTER TABLE (even one that does not involve instant column
operations) will force a table rebuild.
</li><li>`add_last` (1, default in 10.3): Store a hidden metadata record that
allows columns to be appended to the table instantly ([MDEV-11369](https://jira.mariadb.org/browse/MDEV-11369)).
In 10.4 or later, if the table (or partition) is not in this format,
then any ALTER TABLE (even one that does not involve column changes)
will force a table rebuild.
</li><li>`add_drop_reorder` (2, default): From [MariaDB 10.4](/kb/en/what-is-mariadb-104/) only. Like 'add_last', but allow the
metadata record to store a column map, to support instant
add/drop/reorder of columns.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-instant-alter-column-allowed=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Valid Values:</strong> 
<ul start="1"><li>&lt;= [MariaDB 10.3](/kb/en/what-is-mariadb-103/): `never`, `add_last`
</li><li>&gt;= [MariaDB 10.4](/kb/en/what-is-mariadb-104/): `never`, `add_last`, `add_drop_reorder`
</li></ul>
- <strong>Default Value:</strong>
<ul start="1"><li>&lt;= [MariaDB 10.3](/kb/en/what-is-mariadb-103/): `add_last`
</li><li>&gt;= [MariaDB 10.4](/kb/en/what-is-mariadb-104/): `add_drop_reorder`
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.3.23](/kb/en/mariadb-10323-release-notes/), [MariaDB 10.4.13](/kb/en/mariadb-10413-release-notes/), [MariaDB 10.5.3](/kb/en/mariadb-1053-release-notes/)

---

#### `innodb_instrument_semaphores`

- <strong>Description:</strong> Enable semaphore request instrumentation. This could have some effect on performance but allows better information on long semaphore wait problems.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-instrument-semaphores={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/) (treated as if `OFF`)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_io_capacity`

- <strong>Description:</strong> Limit on I/O activity for XtraDB/InnoDB background tasks, including merging data from the insert buffer and flushing pages. Should be set to around the number of I/O operations per second that system can handle, based on the type of drive/s being used. You can also set it higher when the server starts to help with the extra workload at that time, and then reduce for normal use. Ideally, opt for a lower setting, as at higher value data is removed from the buffers too quickly, reducing the effectiveness of caching. See also [innodb_flush_sync](#innodb_flush_sync).
<ul start="1"><li>See [InnoDB Page Flushing: Configuring the InnoDB I/O Capacity](/kb/en/innodb-page-flushing/#configuring-the-innodb-io-capacity) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-io-capacity=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `200`
- <strong>Range:</strong> `100` to `18446744073709551615` (2<sup>64</sup>-1)

---

#### `innodb_io_capacity_max`

- <strong>Description:</strong> Upper limit to which InnodDB can extend [innodb_io_capacity](#innodb_io_capacity) in case of emergency. Only applicable if no value was specified for innodb_io_capacity when the server started up.
<ul start="1"><li>See [InnoDB Page Flushing: Configuring the InnoDB I/O Capacity](/kb/en/innodb-page-flushing/#configuring-the-innodb-io-capacity) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-io-capacity-max=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `2000`
- <strong>Range :</strong> `100` to `18446744073709551615` (2<sup>64</sup>-1)
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_kill_idle_transaction`

- <strong>Description:</strong> Time in seconds before killing an idle XtraDB transaction. If set to `0` (the default), the feature is disabled. Used to prevent accidental user locks. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `9223372036854775807`
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_large_prefix`

- <strong>Description:</strong> If set to `1`, tables that use specific [row formats](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-row-formats-overview/) are permitted to have index key prefixes up to 3072 bytes (for 16k pages, [smaller otherwise](/kb/en/innodb-limitations/#page-sizes)). If not set, the limit is 767 bytes.
<ul start="1"><li>This applies to the [DYNAMIC](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-dynamic-row-format/) and [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) row formats.
</li><li>Removed in 10.3.1 and restored as a deprecated and unused variable in 10.4.3 for compatibility purposes.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-large-prefix</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong>
<ul start="1"><li>`ON` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/))
</li><li>`OFF` (&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
</li></ul>
- <strong>Deprecated:</strong> [MariaDB 10.2](/kb/en/what-is-mariadb-102/)
- <strong>Removed:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)
- <strong>Re-introduced:</strong> [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) (for compatibility purposes)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_lazy_drop_table`

- <strong>Description:</strong> Deprecated and removed in XtraDB 5.6. [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/) processing can take a long time when [innodb_file_per_table](#innodb_file_per_table) is set to 1 and there's a large [buffer pool](/kb/en/xtradbinnodb-memory-buffer/). If `innodb_lazy_drop_table` is set to `1` (`0` is default), XtraDB attempts to optimize [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/) processing by deferring the dropping of related pages from the [buffer pool](/kb/en/xtradbinnodb-memory-buffer/) until there is time, only initially marking them.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-lazy-drop-table={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`
- <strong>Deprecated:</strong> XtraDB 5.5.30-30.2
- <strong>Removed:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_lock_schedule_algorithm`

- <strong>Description:</strong> Specifies the algorithm that InnoDB/XtraDB uses to decide which of the waiting transactions should be granted the lock once it has been released. The possible values are: `FCFS` (First-Come-First-Served) where locks are granted in the order they appear in the lock queue and `VATS` (Variance-Aware-Transaction-Scheduling) where locks are granted based on the Eldest-Transaction-First heuristic. Note that `VATS` should not be used with [Galera](/kb/en/galera/). From [MariaDB 10.1.30](/kb/en/mariadb-10130-release-notes/), InnoDB will refuse to start if `VATS` is used with Galera. From [MariaDB 10.2](/kb/en/what-is-mariadb-102/), `VATS` is default, but from [MariaDB 10.2.12](/kb/en/mariadb-10212-release-notes/), the value will be changed to `FCFS` and a warning produced when using Galera.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-lock-schedule-algorithm=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No (&gt;= [MariaDB 10.2.12](/kb/en/mariadb-10212-release-notes/), [MariaDB 10.1.30](/kb/en/mariadb-10130-release-notes/)), Yes (&lt;= [MariaDB 10.2.11](/kb/en/mariadb-10211-release-notes/), [MariaDB 10.1.29](/kb/en/mariadb-10129-release-notes/))
- <strong>Data Type:</strong> `enum`
- <strong>Valid Values:</strong> `FCFS`, `VATS`
- <strong>Default Value:</strong> `FCFS` ([MariaDB 10.3.9](/kb/en/mariadb-1039-release-notes/), [MariaDB 10.2.17](/kb/en/mariadb-10217-release-notes/)), `VATS` ([MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)), `FCFS` ([MariaDB 10.1](/kb/en/what-is-mariadb-101/))
- <strong>Introduced:</strong> [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/), [MariaDB 10.1.19](/kb/en/mariadb-10119-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/), [MariaDB 10.4.16](/kb/en/mariadb-10416-release-notes/), [MariaDB 10.3.26](/kb/en/mariadb-10326-release-notes/), [MariaDB 10.2.35](/kb/en/mariadb-10235-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_lock_wait_timeout`

- <strong>Description:</strong> Time in seconds that an InnoDB transaction waits for an InnoDB row lock (not table lock) before giving up with the error <code class="fixed" style="white-space:pre-wrap">ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction</code>. When this occurs, the statement (not transaction) is rolled back. The whole transaction can be rolled back if the [innodb_rollback_on_timeout](#innodb_rollback_on_timeout) option is used. Increase this for data warehousing applications or where other long-running operations are common, or decrease for OLTP and other highly interactive applications. This setting does not apply to deadlocks, which InnoDB detects immediately, rolling back a deadlocked transaction. `0` (from [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)) means no wait. See [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-lock-wait-timeout=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `50`
- <strong>Range:</strong>
<ul start="1"><li>`0` to `1073741824` (&gt;= [MariaDB 10.3](/kb/en/what-is-mariadb-103/))
</li><li>`1` to `1073741824` (&lt;= [MariaDB 10.2](/kb/en/what-is-mariadb-102/))
</li></ul>

---

#### `innodb_locking_fake_changes`

- <strong>Description:</strong> From [MariaDB 5.5](/kb/en/what-is-mariadb-55/) to [MariaDB 10.1](/kb/en/what-is-mariadb-101/), XtraDB-only option that if set to `OFF`, fake transactions (see [innodb_fake_changes](#innodb_fake_changes)) don't take row locks. This is an experimental feature to attempt to deal with drawbacks in fake changes blocking real locks. It is not safe for use in all environments. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-locking-fake-changes</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_locks_unsafe_for_binlog`

- <strong>Description:</strong> Set to `0` by default, in which case XtraDB/InnoDB uses [gap locking](/kb/en/xtradbinnodb-lock-modes/#gap-locks). If set to `1`, gap locking is disabled for searches and index scans. Deprecated in [MariaDB 10.0](/kb/en/what-is-mariadb-100/), and removed in [MariaDB 10.5](/kb/en/what-is-mariadb-105/), use [READ COMMITTED transaction isolation level](/kb/en/set-transaction/#read-committed) instead.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-locks-unsafe-for-binlog</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Deprecated:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Removed:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `innodb_log_arch_dir`

- <strong>Description:</strong> The directory for [XtraDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) archiving. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-log-arch-dir=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `./`
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_log_arch_expire_sec`

- <strong>Description:</strong> Time in seconds since the last change after which the archived [XtraDB redo log](/kb/en/xtradbinnodb-redo-log/) should be deleted. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-log-arch-expire-sec=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_log_archive`

- <strong>Description:</strong> Whether or not [XtraDB redo log](/kb/en/xtradbinnodb-redo-log/) archiving is enabled. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-log-archive=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_log_block_size`

- <strong>Description:</strong> Size in bytes of the [XtraDB redo log](/kb/en/xtradbinnodb-redo-log/) records. Generally `512`, the default, or `4096`, are the only two useful values. If the server is restarted and this value is changed, all old log files need to be removed. Should be set to `4096` for SSD cards or if [innodb_flush_method](#innodb_flush_method) is set to `ALL_O_DIRECT` on ext4 filesystems. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-log-block-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `512`
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_log_buffer_size`

- <strong>Description:</strong> Size in bytes of the buffer for writing [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) files to disk. Increasing this means larger transactions can run without needing to perform disk I/O before committing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-log-buffer-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>  `16777216` (16MB) &gt;= [MariaDB 10.1.9](/kb/en/mariadb-1019-release-notes/), `8388608` (8MB) &lt;= [MariaDB 10.1.8](/kb/en/mariadb-1018-release-notes/)
- <strong>Range:</strong> `262144` to `4294967295` (256KB to 4096MB)

---

#### `innodb_log_checksum_algorithm`

- <strong>Description:</strong> Experimental feature (as of [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)), this variable specifies how to generate and verify [XtraDB redo log](/kb/en/xtradbinnodb-redo-log/) checksums. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
<ul start="1"><li>`none` - No checksum. A constant value is instead written to logs, and no checksum validation is performed. 
</li><li>`innodb` - The default, and the original InnoDB algorithm. This is inefficient, but compatible with all MySQL, MariaDB and Percona versions that don't support other checksum algorithms.
</li><li>`crc32` - CRC32<span>©</span> is used for log block checksums, which also permits recent CPUs to use hardware acceleration (on SSE4.2 x86 machines and Power8 or later) for the checksums.
</li><li>`strict_*` - Whether or not to accept checksums from other algorithms. If strict mode is used, checksums blocks will be considered corrupt if they don't match the specified algorithm. Normally they are considered corrupt only if no other algorithm matches.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-log-checksum-algorithm=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> 
<ul start="1"><li>`deprecated` (&gt;= [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/))
</li><li>`innodb` (&lt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/))
</li></ul>
- <strong>Valid Values:</strong> 
<ul start="1"><li>`deprecated`, `innodb`, `none`, `crc32`, `strict_none`, `strict_innodb`, `strict_crc32` (&gt;= [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/))
</li><li>`innodb`, `none`, `crc32`, `strict_none`, `strict_innodb`, `strict_crc32` (&lt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/))
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_log_checksums`

- <strong>Description:</strong> If set to `1`, the CRC32C for Innodb or `innodb_log_checksum_algorithm` for XtraDB algorithm is used for [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) pages. If disabled, the checksum field contents are ignored. From [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/), the variable is deprecated, and checksums are always calculated, as previously, the InnoDB redo log used the slow innodb algorithm, but with hardware or SIMD assisted CRC-32C computation being available, there is no reason to allow checksums to be disabled on the redo log.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-log-checksums={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_log_compressed_pages`

- <strong>Description:</strong> Whether or not images of recompressed pages are stored in the [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/).  Deprecated and ignored from [MariaDB 10.5.3](/kb/en/mariadb-1053-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-log-compressed-pages=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> 
<ul start="1"><li>`ON` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/), &gt;= [MariaDB 10.1.26](/kb/en/mariadb-10126-release-notes/), &lt;= [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/))
</li><li>`OFF` ([MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/) - [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/), [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/) - [MariaDB 10.1.25](/kb/en/mariadb-10125-release-notes/))
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.5.3](/kb/en/mariadb-1053-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_log_file_size`

- <strong>Description:</strong> Size in bytes of each [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) file in the log group. The combined size can be no more than 512GB. Larger values mean less disk I/O due to less flushing checkpoint activity, but also slower recovery from a crash. In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), crash recovery has been improved and shouldn't run out of memory, so the default has been increased. It can safely be set higher to reduce checkpoint flushing, even larger than [innodb_buffer_pool_size](#innodb_buffer_pool_size).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-log-file-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100663296` (96MB) (&gt;= [MariaDB 10.5](/kb/en/what-is-mariadb-105/)), `50331648` (48MB) (&lt;= [MariaDB 10.4](/kb/en/what-is-mariadb-104/))
- <strong>Range:</strong> `1048576` to `512GB` (1MB to 512GB)

---

#### `innodb_log_files_in_group`

- <strong>Description:</strong> Number of physical files in the [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/). Deprecated and ignored from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-log-files-in-group=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1` (&gt;= [MariaDB 10.5](/kb/en/what-is-mariadb-105/)), `2` (&lt;= [MariaDB 10.4](/kb/en/what-is-mariadb-104/))
- <strong>Range:</strong> `1` to `100` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)), `2` to `100` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))
- <strong>Deprecated:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_log_group_home_dir`

- <strong>Description:</strong> Path to the [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) files. If none is specified, [innodb_log_files_in_group](#innodb_log_files_in_group) files named ib_logfile0 and so on, with a size of [innodb_log_file_size](#innodb_log_file_size) are created in the data directory.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-log-group-home-dir=path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `directory name`

---

#### `innodb_log_optimize_ddl`

- <strong>Description:</strong> Whether [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) activity should be reduced when natively creating indexes or rebuilding tables. Reduced logging requires additional page flushing and interferes with [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/). Enabling this may slow down backup and cause delay due to page flushing. Deprecated and ignored from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/). Deprecated (but not ignored) from [MariaDB 10.4.16](/kb/en/mariadb-10416-release-notes/), [MariaDB 10.3.26](/kb/en/mariadb-10326-release-notes/) and [MariaDB 10.2.35](/kb/en/mariadb-10235-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-log-optimize-ddl={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> 
<ul><li>`OFF` (&gt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/), [MariaDB 10.4.16](/kb/en/mariadb-10416-release-notes/), [MariaDB 10.3.26](/kb/en/mariadb-10326-release-notes/), [MariaDB 10.2.35](/kb/en/mariadb-10235-release-notes/))
</li><li>`ON` (&lt;= [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/), [MariaDB 10.4.15](/kb/en/mariadb-10415-release-notes/), [MariaDB 10.3.25](/kb/en/mariadb-10325-release-notes/), [MariaDB 10.2.34](/kb/en/mariadb-10234-release-notes/))
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.2.17](/kb/en/mariadb-10217-release-notes/), [MariaDB 10.3.9](/kb/en/mariadb-1039-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/), [MariaDB 10.4.16](/kb/en/mariadb-10416-release-notes/), [MariaDB 10.3.26](/kb/en/mariadb-10326-release-notes/), [MariaDB 10.2.35](/kb/en/mariadb-10235-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_log_write_ahead_size`

- <strong>Description:</strong> [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) write ahead unit size to avoid read-on-write. Should match the OS cache block IO size.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-log-write-ahead-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8192`
- <strong>Range:</strong> `512` to [innodb_page_size](#innodb_page_size)
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `innodb_lru_flush_size`

- <strong>Description:</strong> Number of pages to flush on LRU eviction.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-lru-flush-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `32`
- <strong>Range:</strong> `1` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/)

---

#### `innodb_lru_scan_depth`

- <strong>Description:</strong> Specifies how far down the buffer pool least-recently used (LRU) list the cleaning thread should look for dirty pages to flush. This process is performed once a second. In an I/O intensive-workload, can be increased if there is spare I/O capacity, or decreased if in a write-intensive workload with little spare I/O capacity.
<ul start="1"><li>See [InnoDB Page Flushing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-flushing/) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-lru-scan-depth=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> 
<ul start="1"><li>`1536` (&gt;= [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/))
</li><li>`1024` (&lt;= [MariaDB 10.5.6](/kb/en/mariadb-1056-release-notes/))
</li></ul>
- <strong>Range - 32bit:</strong> `100` to `2<sup>32</sup>-1`
- <strong>Range - 64bit:</strong> `100` to `2<sup>64</sup>-1`

---

#### `innodb_max_bitmap_file_size`

- <strong>Description:</strong> Limit in bytes of the changed page bitmap files. For faster incremental backup with [Xtrabackup](/kb/en/backup-restore-and-import-xtrabackup/), XtraDB tracks pages with changes written to them according to the [XtraDB redo log](/kb/en/xtradbinnodb-redo-log/) and writes the information to special changed page bitmap files. These files are rotated when the server restarts or when this limit is reached. XtraDB only. See also [innodb_track_changed_pages](#innodb_track_changed_pages) and [innodb_max_changed_pages](#innodb_max_changed_pages). 
<ul start="1"><li>Deprecated and ignored in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-max-bitmap-file-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4096` (4KB)
- <strong>Range:</strong> `4096` (4KB) to `18446744073709551615` (16EB)
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)

---

#### `innodb_max_changed_pages`

- <strong>Description:</strong> Limit to the number of changed page bitmap files (stored in the [Information Schema INNODB_CHANGED_PAGES table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_changed_pages-table/)). Zero is unlimited. See [innodb_max_bitmap_file_size](#innodb_max_bitmap_file_size) and [innodb_track_changed_pages](#innodb_track_changed_pages). Previously named `innodb_changed_pages_limit`. XtraDB only.
<ul start="1"><li>Deprecated and ignored in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-max-changed-pages=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1000000`
- <strong>Range:</strong> `0` to `18446744073709551615`
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)

---

#### `innodb_max_dirty_pages_pct`

- <strong>Description:</strong> Maximum percentage of unwritten (dirty) pages in the buffer pool.
<ul start="1"><li>See [InnoDB Page Flushing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-flushing/) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-max-dirty-pages-pct=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> 
<ul start="1"><li>`90.000000` (&gt;= [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/))
</li><li>`75.000000` (&lt;= [MariaDB 10.5.6](/kb/en/mariadb-1056-release-notes/))
</li></ul>
- <strong>Range:</strong> `0` to `99.999`

---

#### `innodb_max_dirty_pages_pct_lwm`

- <strong>Description:</strong> Low water mark percentage of dirty pages that will enable preflushing to lower the dirty page ratio. The value 0 (default) means 'refer to [innodb_max_dirty_pages_pct](#innodb_max_dirty_pages_pct)'.
<ul start="1"><li>See [InnoDB Page Flushing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-flushing/) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-max-dirty-pages-pct-lwm=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)), `0.001000` (&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
- <strong>Range:</strong>  `0` to `99.999`

---

#### `innodb_max_purge_lag`

- <strong>Description:</strong> When purge operations are lagging on a busy server, setting innodb_max_purge_lag can help. By default set to `0`, no lag, the figure is used to calculate a time lag for each INSERT, UPDATE, and DELETE when the system is lagging. XtraDB/InnoDB keeps a list of transactions with delete-marked index records due to UPDATE and DELETE statements. The length of this list is `purge_lag`, and the calculation, performed every ten seconds, is as follows: ((purge_lag/innodb_max_purge_lag)×10)–5 milliseconds.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-max-purge-lag=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `innodb_max_purge_lag_delay`

- <strong>Description:</strong> Maximum delay in milliseconds imposed by the [innodb_max_purge_lag](#innodb_max_purge_lag) setting. If set to `0`, the default, there is no maximum.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-max-purge-lag-delay=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_max_purge_lag_wait`

- <strong>Description:</strong> Wait until History list length is below the specified limit.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-max-purge-wait=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4294967295`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/), [MariaDB 10.4.16](/kb/en/mariadb-10416-release-notes/), [MariaDB 10.3.26](/kb/en/mariadb-10326-release-notes/), [MariaDB 10.2.35](/kb/en/mariadb-10235-release-notes/)

---

#### `innodb_max_undo_log_size`

- <strong>Description:</strong> If an undo tablespace is larger than this, it will be marked for truncation if [innodb_undo_log_truncate](#innodb_undo_log_truncate) is set.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-max-undo-log-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> 
<ul start="1"><li>`10485760` (&gt;= [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/))
</li><li>`1073741824` (&lt;= [MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/))
</li></ul>
- <strong>Range:</strong> `10485760` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `innodb_merge_sort_block_size`

- <strong>Description:</strong> Size in bytes of the block used for merge sorting in fast index creation. Replaced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6 by [innodb_sort_buffer_size](#innodb_sort_buffer_size).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-merge-sort-block-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1048576` (1M)
- <strong>Range:</strong> `1048576` (1M) to `1073741824` (1G)
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/) - replaced by [innodb_sort_buffer_size](#innodb_sort_buffer_size)

---

#### `innodb_mirrored_log_groups`

- <strong>Description:</strong> Unused. Restored as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Deprecated:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Removed:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) - [MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/)

---

#### `innodb_mtflush_threads`

- <strong>Description:</strong> Sets the number of threads to use in Multi-Threaded Flush operations. For more information, see [Fusion-io Multi-threaded Flush](/kb/en/fusion-io-multi-threaded-flush/).
<ul start="1"><li>InnoDB's multi-thread flush feature was deprecated in [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/) and removed from [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/). In later versions of MariaDB, use <a undefined>innodb_page_cleaners</a> system variable instead.
</li><li>See [InnoDB Page Flushing: Page Flushing with Multi-threaded Flush Threads](/kb/en/innodb-page-flushing/#page-flushing-with-multi-threaded-flush-threads) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-mtflush-threads=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8`
- <strong>Range:</strong> `1` to `64`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/)

---

#### `innodb_monitor_disable`

- <strong>Description:</strong> Disables the specified counters in the [INFORMATION_SCHEMA.INNODB_METRICS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_metrics-table/) table.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-monitor-disable=string</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_monitor_enable`

- <strong>Description:</strong> Enables the specified counters in the [INFORMATION_SCHEMA.INNODB_METRICS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_metrics-table/) table.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-monitor-enable=string</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_monitor_reset`

- <strong>Description:</strong> Resets the count value of the specified counters in the [INFORMATION_SCHEMA.INNODB_METRICS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_metrics-table/) table to zero.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-monitor-reset=string</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_monitor_reset_all`

- <strong>Description:</strong> Resets all values for the specified counters in the [INFORMATION_SCHEMA.INNODB_METRICS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_metrics-table/) table.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">---innodb-monitor-reset-all=string</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_numa_interleave`

- <strong>Description:</strong> Whether or not to use the NUMA interleave memory policy to allocate the [InnoDB buffer pool](/kb/en/xtradbinnodb-buffer-pool/). Before [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/), required that MariaDB be compiled on a NUMA-enabled Linux system.  innodb_numa_interleave is not present in Community Server 10.5.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-numa-interleave={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `innodb_old_blocks_pct`

- <strong>Description:</strong> Percentage of the [buffer pool](/kb/en/xtradbinnodb-memory-buffer/) to use for the old block sublist.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-old-blocks-pct=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `37`
- <strong>Range:</strong> `5` to `95`

---

#### `innodb_old_blocks_time`

- <strong>Description:</strong> Time in milliseconds an inserted block must stay in the old sublist after its first access before it can be moved to the new sublist. '0' means "no delay". Setting  a non-zero value can help prevent full table scans clogging the [buffer pool](/kb/en/xtradbinnodb-memory-buffer/). See also [innodb_old_blocks_pct](#innodb_old_blocks_pct).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-old-blocks-time=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1000` (from [MariaDB 10.0](/kb/en/what-is-mariadb-100/)), `0` (before [MariaDB 10.0](/kb/en/what-is-mariadb-100/))
- <strong>Range:</strong> `0` to `2<sup>32</sup>-1`

---

#### `innodb_online_alter_log_max_size`

- <strong>Description:</strong> The maximum size for temporary log files during online DDL (data and index structure changes). The temporary log file is used for each table being altered, or index being created, to store data changes to the table while the process is underway. The table is extended by [innodb_sort_buffer_size](#innodb_sort_buffer_size) up to the limit set by this variable. If this limit is exceeded, the online DDL operation fails and all uncommitted changes are rolled back. A lower value reduces the time a table could lock at the end of the operation to apply all the log's changes, but also increases the chance of the online DDL changes failing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-online-alter-log-max-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `134217728`
- <strong>Range:</strong> `65536` to `2<sup>64</sup>-1`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `innodb_open_files`

- <strong>Description:</strong> Maximum .ibd files MariaDB can have open at the same time. Only applies to systems with multiple XtraDB/InnoDB tablespaces, and is separate to the table cache and [open_files_limit](#open_files_limit). In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) the default, if [innodb_file_per_table](#innodb_file_per_table) is disabled, is 300 or the value of [table_open_cache](/kb/en/server-system-variables/#table_open_cache), whichever is higher. It will also auto-size up to the default value if it is set to a value less than `10`.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-open-files=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `autosized` (from [MariaDB 10.0](/kb/en/what-is-mariadb-100/)), `300` (before [MariaDB 10.0](/kb/en/what-is-mariadb-100/))
- <strong>Range:</strong> `10` to `4294967295`

---

#### `innodb_optimize_fulltext_only`

- <strong>Description:</strong> When set to `1` (`0` is default), [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/) will only process InnoDB [FULLTEXT index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) data. Only intended for use during fulltext index maintenance.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-optimize-fulltext-only=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_page_cleaners`

- <strong>Description:</strong> Number of page cleaner threads. The default is `4`, but the value will be set to the number of [innodb_buffer_pool_instances](#innodb_buffer_pool_instances) if this is lower. If set to `1`, only a single cleaner thread is used, as was the case until [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/). Cleaner threads flush dirty pages from the [buffer pool](/kb/en/xtradbinnodb-buffer-pool/), performing flush list and least-recently used (LRU) flushing. Deprecated and ignored from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/), as the original reasons for for splitting the buffer pool have mostly gone away.
<ul start="1"><li>See [InnoDB Page Flushing: Page Flushing with Multiple InnoDB Page Cleaner Threads](/kb/en/innodb-page-flushing/#page-flushing-with-multiple-innodb-page-cleaner-threads) for more information. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-page-cleaners=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes (&gt;= [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)), No (&lt;= [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/))
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4` (or set to [innodb_buffer_pool_instances](#innodb_buffer_pool_instances) if lower)
- <strong>Range:</strong> `1` to `64`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_page_size`

- <strong>Description:</strong> Specifies the page size in bytes for all InnoDB tablespaces. The default, `16k`, is suitable for most uses.
<ul start="1"><li>A smaller InnoDB page size might work more effectively in a situation with many small writes (OLTP), or with SSD storage, which usually has smaller block sizes.
</li><li>A larger InnoDB page size can provide a larger [maximum row size](/kb/en/innodb-row-formats-overview/#maximum-row-size).
</li><li>InnoDB's page size can be as large as `64k` for tables using the following [row formats](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-row-formats-overview/): [DYNAMIC](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-dynamic-row-format/), [COMPACT](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compact-row-format/), and [REDUNDANT](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-redundant-row-format/).
</li><li>InnoDB's page size must still be `16k` or less for tables using the [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) row format.
</li><li>This system variable's value cannot be changed after the `datadir` has been initialized. InnoDB's page size is set when a MariaDB instance starts, and it remains constant afterwards.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-page-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `16384`
- <strong>Valid Values:</strong> `4k` or `4096`, `8k` or `8192`, `16k` or `16384`, `32k` and `64k`.

---

#### `innodb_pass_corrupt_table`

- <strong>Removed:</strong> XtraDB 5.5  - renamed [innodb_corrupt_table_action](#innodb_corrupt_table_action).

---

#### `innodb_prefix_index_cluster_optimization`

- <strong>Description:</strong> Enable prefix optimization to sometimes avoid cluster index lookups.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-prefix-index-cluster-optimization=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

#### `innodb_print_all_deadlocks`

- <strong>Description:</strong> If set to `1` (`0` is default), all XtraDB/InnoDB transaction deadlock information is written to the [error log](/mariadb-administration/server-monitoring-logs/error-log/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-print-all-deadlocks=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `innodb_purge_batch_size`

- <strong>Description:</strong> Units of [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) records that will trigger a purge operation. Together with [innodb_purge_threads](#innodb_purge_threads) has a small effect on tuning.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-purge-batch-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `20`
- <strong>Range:</strong> `1` to `5000`

---

#### `innodb_purge_rseg_truncate_frequency`

- <strong>Description:</strong> Frequency with which undo records are purged. Set by default to every 128 times, reducing this increases the frequency at which rollback segments are freed. See also [innodb_undo_log_truncate](#innodb_undo_log_truncate).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">-- innodb-purge-rseg-truncate-frequency=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `128`
- <strong>Range:</strong> `1` to `128`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `innodb_purge_threads`

- <strong>Description:</strong> Number of background threads dedicated to XtraDB/InnoDB purge operations. Since [MariaDB 10.0](/kb/en/what-is-mariadb-100/), the range has been `1` to `32`. At least one background thread is always used from [MariaDB 10.0](/kb/en/what-is-mariadb-100/). The default has been increased from `1` to `4` in [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/). Setting to a value greater than 1 creates that many separate purge threads. This can improve efficiency in some cases, such as when performing DML operations on many tables. In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), the options are `0` and `1`. If set to `0`, the default, purging is done with the master thread. If set to `1`, purging is done on a separate thread, which could reduce contention. See also [innodb_purge_batch_size](#innodb_purge_batch_size).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-purge-threads=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>
<ul start="1"><li>`4` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/))
</li><li>`1` (&gt;=[MariaDB 10.0](/kb/en/what-is-mariadb-100/) to &lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
</li><li>`0` ([MariaDB 5.5](/kb/en/what-is-mariadb-55/))
</li></ul>
- <strong>Range:</strong> `1` to `32` (&gt;=[MariaDB 10.0](/kb/en/what-is-mariadb-100/)), `0` to `1` ([MariaDB 5.5](/kb/en/what-is-mariadb-55/))

---

#### `innodb_random_read_ahead`

- <strong>Description:</strong> Originally, random read-ahead was always set as an optimization technique, but was removed in [MariaDB 5.5](/kb/en/what-is-mariadb-55/). `innodb_random_read_ahead` permits it to be re-instated if set to `1` (`0`) is default.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-random-read-ahead=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `innodb_read_ahead`

- <strong>Description:</strong> If set to `linear`, the default, XtraDB/InnoDB will automatically fetch remaining pages if there are enough within the same extent that can be accessed sequentially. If set to `none`, read-ahead is disabled. `random` has been removed and is now ignored, while `both` sets to both `linear` and `random`. Also see [innodb_read_ahead_threshold](#innodb_read_ahead_threshold) for more control on read-aheads. Removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6 and replaced by MySQL 5.6's [innodb_random_read_ahead](#innodb_random_read_ahead).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-read-ahead=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `linear`
- <strong>Valid Values:</strong> `none`, `random`, `linear`, `both`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6 - replaced by MySQL 5.6's [innodb_random_read_ahead](#innodb_random_read_ahead)

---

#### `innodb_read_ahead_threshold`

- <strong>Description:</strong> Minimum number of pages XtraDB/InnoDB must read from an extent of 64 before initiating an asynchronous read for the following extent.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-read-ahead-threshold=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `56`
- <strong>Range:</strong> `0` to `64`

---

#### `innodb_read_io_threads`

- <strong>Description:</strong> Number of I/O threads for XtraDB/InnoDB reads. You may on rare occasions need to reduce this default on Linux systems running multiple MariaDB servers to avoid exceeding system limits.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-read-io-threads=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4`
- <strong>Range:</strong> `1` to `64`

---

#### `innodb_read_only`

- <strong>Description:</strong> If set to `1` (`0` is default), the server will be read-only. For use in distributed applications, data warehouses or read-only media.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-read-only=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `innodb_read_only_compressed`

- <strong>Description:</strong> If set (the default), [ROW_FORMAT=COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) tables will be read-only.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-read-only-compressed</code>, <code class="fixed" style="white-space:pre-wrap">--skip-innodb-read-only-compressed</code>
- <strong>Scope:</strong>
- <strong>Dynamic:</strong>
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_recovery_stats`

- <strong>Description:</strong> If set to `1` (`0` is default) and recovery is necessary on startup, the server will write detailed recovery statistics to the error log at the end of the recovery process. This Percona XtraDB variable has not been ported to XtraDB 5.6.
- <strong>Commandline:</strong> No
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_recovery_update_relay_log`

- <strong>Description:</strong> If set to `1` (`0` is default), the relay log info file will be overwritten on crash recovery if the information differs from the InnoDB record. Should not be used if multiple storage engine types are being replicated. Previously named `innodb_overwrite_relay_log_info`. Removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6 and replaced by MySQL 5.6's `relay-log-recovery`
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-recovery-update-relay-log={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/) - replaced by MySQL 5.6's `relay-log-recovery`

---

#### `innodb_replication_delay`

- <strong>Description:</strong> Time in milliseconds for the slave server to delay the replication thread if [innodb_thread_concurrency](#innodb_thread_concurrency) is reached. Deprecated and ignored from [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-replication-delay=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Deprecated:</strong> [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_rollback_on_timeout`

- <strong>Description:</strong> InnoDB usually rolls back the last statement of a transaction that's been timed out (see [innodb_lock_wait_timeout](#innodb_lock_wait_timeout)). If innodb_rollback_on_timeout is set to 1 (0 is default), InnoDB will roll back the entire transaction. Before [MariaDB 5.5](/kb/en/what-is-mariadb-55/), rolling back the entire transaction was the default behavior.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-rollback-on-timeout</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `innodb_rollback_segments`

- <strong>Description:</strong> Specifies the number of rollback segments that XtraDB/InnoDB will use within a transaction (see [undo log](/kb/en/undo-log/)). Deprecated and replaced by [innodb_undo_logs](#innodb_undo_logs) in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-rollback-segments=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `128`
- <strong>Range:</strong> `1` to `128`
- <strong>Deprecated:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Removed:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `innodb_safe_truncate`

- <strong>Description:</strong> Use a backup-safe [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table/) implementation and crash-safe rename operations inside InnoDB. This is not compatible with hot backup tools other than [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/mariabackup-overview/). Users who need to use such tools may set this to `OFF`.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-safe-truncate={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.2.19](/kb/en/mariadb-10219-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_scrub_log`

- <strong>Description:</strong> Enable [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) scrubbing. See [Data Scrubbing](/kb/en/xtradbinnodb-data-scrubbing/). Deprecated and ignored from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), as never  really worked ([MDEV-13019](https://jira.mariadb.org/browse/MDEV-13019) and [MDEV-18370](https://jira.mariadb.org/browse/MDEV-18370)). If old log contents should be kept secret, then enabling [innodb_encrypt_log](/kb/en/innodb-system-variables/#innodb_encrypt_log) or setting a smaller [innodb_log_file_size](/kb/en/innodb-system-variables/#innodb_log_file_size) could help.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-scrub-log</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_scrub_log_interval`

- <strong>Description:</strong> Used with [Data Scrubbing](/kb/en/xtradbinnodb-data-scrubbing/) in 10.1.3 only - replaced in 10.1.4 by [innodb_scrub_log_speed](#innodb_scrub_log_speed). [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) scrubbing interval in milliseconds.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-scrub-log-interval=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `56`
- <strong>Range:</strong> `0` to `50000`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `innodb_scrub_log_speed`

- <strong>Description:</strong> [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) scrubbing speed in bytes/sec. See [Data Scrubbing](/kb/en/xtradbinnodb-data-scrubbing/). Deprecated and ignored from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-scrub-log-speed=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `256`
- <strong>Range:</strong> `1` to `50000`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_sched_priority_cleaner`

- <strong>Description:</strong> Set a thread scheduling priority for cleaner and least-recently used (LRU) manager threads. The range from `0` to `39` corresponds in reverse order to Linux nice values of `-20` to `19`. So `0` is the lowest priority (Linux nice value `19`) and `39` is the highest priority (Linux nice value `-20`). XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-sched-priority-cleaner=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `19`
- <strong>Range:</strong> `0` to `39`
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_show_locks_held`

- <strong>Description:</strong> Specifies the number of locks held for each InnoDB transaction to be displayed in [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine/) output. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-show-locks-held=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `0` to `1000`
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_show_verbose_locks`

- <strong>Description:</strong> If set to `1`, and [innodb_status_output_locks](#innodb_status_output_locks) is also ON, the traditional InnoDB behavior is followed and locked records will be shown in [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status/) output. If set to `0`, the default, only high-level information about the lock is shown. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-show-verbose-locks=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `1`
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_simulate_comp_failures`

- <strong>Description:</strong> Simulate compression failures. Used for testing robustness against random compression failures. XtraDB only.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `99`
- <strong>Introduced:</strong> [MariaDB 10.0.14](/kb/en/mariadb-10014-release-notes/)

---

#### `innodb_sort_buffer_size`

- <strong>Description:</strong> Size of the sort buffers used for sorting data when an InnoDB index is created, as well as the amount by which the temporary log file is extended during online DDL operations to record concurrent writes. Before [MariaDB 10.0](/kb/en/what-is-mariadb-100/), this was not configurable and the current default setting of 1MB was fixed. The larger the setting, the fewer merge phases are required between buffers while sorting. When a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) creates a new index, three buffers of this size are allocated, as well as pointers for the rows in the buffer.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-sort-buffer-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1048576` (1M)
- <strong>Range:</strong> `65536` to `67108864`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_spin_wait_delay`

- <strong>Description:</strong> Maximum delay (not strictly corresponding to a time unit) between spin lock polls. Default changed from `6` to `4` in [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/), as this was verified to give the best throughput by OLTP update index and read-write benchmarks on Intel Broadwell (2/20/40) and ARM (1/46/46).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-spin-wait-delay=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4` (&gt;= [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)), `6` (&lt;= [MariaDB 10.3.4](/kb/en/mariadb-1034-release-notes/))
- <strong>Range:</strong> `0` to `4294967295`

---

#### `innodb_stats_auto_recalc`

- <strong>Description:</strong> If set to `1` (the default), persistent statistics are automatically recalculated when the table changes significantly (more than 10% of the rows). Affects tables created or altered with STATS_PERSISTENT=1 (see [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/)), or when [innodb_stats_persistent](#innodb_stats_persistent) is enabled. [innodb_stats_persistent_sample_pages](#innodb_stats_persistent_sample_pages) determines how much data to sample when recalculating. See [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-stats-auto-recalc=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `innodb_stats_auto_update`

- <strong>Description:</strong> If set to `0` (`1` is default), index statistics will not be automatically calculated except when an [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) is run, or the table is first opened. Replaced by [innodb_stats_auto_recalc](#innodb_stats_auto_recalc) in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/) - replaced by [innodb_stats_auto_recalc](#innodb_stats_auto_recalc).

---

#### `innodb_stats_include_delete_marked`

- <strong>Description:</strong> Include delete marked records when calculating persistent statistics.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)

---

#### `innodb_stats_method`

- <strong>Description:</strong> Determines how NULLs are treated for XtraDB/InnoDB index statistics purposes. If set to `nulls_equal`, the default, all NULL index values are treated as a single group. This is usually fine, but if you have large numbers of NULLs the average group size is slanted higher, and the optimizer may miss using the index for ref accesses when it would be useful. If set to `nulls_unequal`, the opposite approach is taken, with each NULL forming its own group of one. Conversely, the average group size is slanted lower, and the optimizer may use the index for ref accesses when not suitable. Setting to `nulls_ignored` ignores NULLs altogether from index group calculations. See also [Index Statistics](/replication/optimization-and-tuning/optimization-and-indexes/index-statistics/), [aria_stats_method](/kb/en/aria-server-system-variables/#aria_stats_method) and [myisam_stats_method](/kb/en/myisam-server-system-variables/#myisam_stats_method).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-stats-method=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `nulls_equal`
- <strong>Valid Values:</strong> `nulls_equal`, `nulls_unequal`, `nulls_ignored`

---

#### `innodb_stats_modified_counter`

- <strong>Description:</strong> The number of rows modified before we calculate new statistics. If set to `0`, the default, current limits are used.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-stats-modified-counter=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/)

---

#### `innodb_stats_on_metadata`

- <strong>Description:</strong> If set to `1`, the default, XtraDB/InnoDB updates statistics when accessing the INFORMATION_SCHEMA.TABLES or INFORMATION_SCHEMA.STATISTICS tables, and when running metadata statements such as [SHOW INDEX](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-index/) or [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status/). If set to `0`, statistics are not updated at those times, which can reduce the access time for large schemas, as well as make execution plans more stable.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-stats-on-metadata</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF` (from [MariaDB 10.0](/kb/en/what-is-mariadb-100/)), `ON` (before [MariaDB 10.0](/kb/en/what-is-mariadb-100/))

---

#### `innodb_stats_persistent`

- <strong>Description:</strong> [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) produces index statistics, and this setting determines whether they will be stored on disk, or be required to be recalculated more frequently, such as when the server restarts. This information is stored for each table, and can be set with the STATS_PERSISTENT clause when creating or altering tables (see [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/)). See [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-stats-persistent=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `innodb_stats_persistent_sample_pages`

- <strong>Description:</strong> Number of index pages sampled when estimating cardinality and statistics for indexed columns. Increasing this value will increases index statistics accuracy, but use more I/O resources when running [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/). See [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-stats-persistent-sample-pages=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `20`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_stats_sample_pages`

- <strong>Description:</strong> Gives control over the index distribution statistics by determining the number of index pages to sample. Higher values produce more disk I/O, but, especially for large tables, produce more accurate statistics and therefore make more effective use of the query optimizer. Lower values than the default are not recommended, as the statistics can be quite inaccurate.
<ul start="1"><li>If <a undefined>innodb_stats_traditional</a> is enabled, then the exact number of pages configured by this system variable will be sampled for statistics.
</li><li>If <a undefined>innodb_stats_traditional</a> is disabled, then the number of pages to sample for statistics is calculated using a logarithmic algorithm, so the exact number can change depending on the size of the table. This means that more samples may be used for larger tables.
</li><li>If [persistent statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/) are enabled, then the <a undefined>innodb_stats_persistent_sample_pages</a> system variable applies instead. [persistent statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/) are enabled with the <a undefined>innodb_stats_persistent</a> system variable.
</li><li>In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later, this system variable has been <strong>deprecated</strong>. In those versions, the <a undefined>innodb_stats_transient_sample_pages</a> system variable should be used instead.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-stats-sample-pages=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8`
- <strong>Range:</strong> `1` to `2<sup>64</sup>-1`
- <strong>Deprecated:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Removed:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `innodb_stats_traditional`

- <strong>Description:</strong> This system variable affects how the number of pages to sample for transient statistics is determined. In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), this system variable affects how the <a undefined>innodb_stats_sample_pages</a> system variable is used. In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later, this system variable affects how the <a undefined>innodb_stats_transient_sample_pages</a> is used.
<ul start="1"><li>If <a undefined>innodb_stats_traditional</a> is enabled, then the exact number of pages configured by the system variable will be sampled for statistics.
</li><li>If <a undefined>innodb_stats_traditional</a> is disabled, then the number of pages to sample for statistics is calculated using a logarithmic algorithm, so the exact number can change depending on the size of the table. This means that more samples may be used for larger tables.
</li><li>This system variable does not affect the calculation of [persistent statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/).
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-stats-traditional=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `innodb_stats_transient_sample_pages`

- <strong>Description:</strong> Gives control over the index distribution statistics by determining the number of index pages to sample. Higher values produce more disk I/O, but, especially for large tables, produce more accurate statistics and therefore make more effective use of the query optimizer. Lower values than the default are not recommended, as the statistics can be quite inaccurate.
<ul start="1"><li>If <a undefined>innodb_stats_traditional</a> is enabled, then the exact number of pages configured by this system variable will be sampled for statistics.
</li><li>If <a undefined>innodb_stats_traditional</a> is disabled, then the number of pages to sample for statistics is calculated using a logarithmic algorithm, so the exact number can change depending on the size of the table. This means that more samples may be used for larger tables.
</li><li>If [persistent statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/) are enabled, then the <a undefined>innodb_stats_persistent_sample_pages</a> system variable applies instead. [persistent statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/) are enabled with the <a undefined>innodb_stats_persistent</a> system variable.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-stats-transient-sample-pages=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8`
- <strong>Range:</strong> `1` to `2<sup>64</sup>-1`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_stats_update_need_lock`

- <strong>Description:</strong> Setting to `0` (`1` is default) may help reduce contention of the `&amp;dict_operation_lock`, but also disables the <em>Data_free</em> option in [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status/). This Percona XtraDB variable has not been ported to XtraDB 5.6.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6

---

#### `innodb_status_output`

- <strong>Description:</strong> Enable [InnoDB monitor](/kb/en/xtradbinnodb-monitors/) output to the [error log](/mariadb-administration/server-monitoring-logs/error-log/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-status-output={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/)

---

#### `innodb_status_output_locks`

- <strong>Description:</strong> Enable [InnoDB lock monitor](/columns-storage-engines-and-plugins/storage-engines/innodb/xtradb-innodb-monitors/) output to the [error log](/mariadb-administration/server-monitoring-logs/error-log/) and [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status/). Also requires [innodb_status_output=ON](#innodb_status_output) to enable output to the error log.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-status-output-locks={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/)

---

#### `innodb_strict_mode`

- <strong>Description:</strong> If set to `1` (`0` is the default before [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)), XtraDB/InnoDB will return errors instead of warnings in certain cases, similar to strict SQL mode.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-strict-mode=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong>
<ul start="1"><li>`ON` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/))
</li><li>`OFF` (&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
</li></ul>

---

#### `innodb_support_xa`

- <strong>Description:</strong> If set to `1`, the default, [XA transactions](/sql-statements-structure/sql-statements/transactions/xa-transactions/) are supported. XA support ensures data is written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) in the same order to the actual database, which is critical for [replication](/replication/) and disaster recovery, but comes at a small performance cost. If your database is set up to only permit one thread to change data (for example, on a replication slave with only the replication thread writing), it is safe to turn this option off. Removed in [MariaDB 10.3](/kb/en/what-is-mariadb-103/), XA transactions are always supported.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-support-xa</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Deprecated:</strong> [MariaDB 10.2](/kb/en/what-is-mariadb-102/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_sync_array_size`

- <strong>Description:</strong> By default `1`, can be increased to split internal thread co-ordinating, giving higher concurrency when there are many waiting threads.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-sync-array-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` to `1024`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `innodb_sync_spin_loops`

- <strong>Description:</strong> The number of times a thread waits for an XtraDB/InnoDB mutex to be freed before the thread is suspended.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-sync-spin-loops=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `30`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `innodb_table_locks`

- <strong>Description:</strong> If [autocommit](#autocommit) is set to to `0` (`1` is default), setting innodb_table_locks to `1`, the default, will cause XtraDB/InnoDB to lock a table internally upon a [LOCK TABLE](LOCK_TABLES).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-table-locks</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `innodb_thread_concurrency`

- <strong>Description:</strong> Once this number of threads is reached (excluding threads waiting for locks), XtraDB/InnoDB will place new threads in a wait state in a first-in, first-out queue for execution, in order to limit the number of threads running concurrently. A setting of `0`, the default, permits as many threads as necessary. A suggested setting is twice the number of CPU's plus the number of disks. Deprecated and ignored from [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-thread-concurrency=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `1000`
- <strong>Deprecated:</strong> [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_thread_concurrency_timer_based`

- <strong>Description:</strong> If set to `1`, thread concurrency will be handled in a lock-free timer-based manner rather than the default mutex-based method. Depends on atomic op builtins being available. This Percona XtraDB variable has not been ported to XtraDB 5.6.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-thread-concurrency-timer-based={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6

---

#### `innodb_thread_sleep_delay`

- <strong>Description:</strong> Time in microseconds that InnoDB threads sleep before joining the queue. Setting to `0` disables sleep. Deprecated and ignored from [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/)
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-thread-sleep-delay=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> 
<ul start="1"><li>`0` (&gt;= [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/).)
</li><li>`10000` (&lt;= [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/))
</li></ul>
- <strong>Range:</strong> `0` to `1000000`
- <strong>Deprecated:</strong> [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_temp_data_file_path`

- <strong>Description:</strong>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-temp-data-file-path=path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `ibtmp1:12M:autoextend`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `innodb_tmpdir`

- <strong>Description:</strong> Allows an alternate location to be set for temporary non-tablespace files. If not set (the default), files will be created in the usual [tmpdir](/kb/en/server-system-variables/#tmpdir) location.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-tmpdir=path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> Empty
- <strong>Introduced:</strong> [MariaDB 10.1.14](/kb/en/mariadb-10114-release-notes/), [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/)

---

#### `innodb_track_changed_pages`

- <strong>Description:</strong> For faster incremental backup with [Xtrabackup](/kb/en/backup-restore-and-import-xtrabackup/), XtraDB tracks pages with changes written to them according to the [XtraDB redo log](/kb/en/xtradbinnodb-redo-log/) and writes the information to special changed page bitmap files. This read-only variable is used for controlling this feature. See also [innodb_max_changed_pages](#innodb_max_changed_pages) and [innodb_max_bitmap_file_size](#innodb_max_bitmap_file_size). XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-track-changed-pages={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)

---

#### `innodb_track_redo_log_now`

- <strong>Description:</strong> Available on debug builds only. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-track-redo-log-now={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)

---

#### `innodb_undo_directory`

- <strong>Description:</strong> Path to the directory (relative or absolute) that InnoDB uses to create separate tablespaces for the [undo logs](/kb/en/undo-log/). `.` (the default value before 10.2.2) leaves the undo logs in the same directory as the other log files. From [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), the default value is NULL, and if no path is specified, undo tablespaces will be created in the directory defined by [datadir](/kb/en/server-system-variables/#datadir). Use together with [innodb_undo_logs](#innodb_undo_logs) and [innodb_undo_tablespaces](#innodb_undo_tablespaces). Undo logs are most usefully placed on a separate storage device.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-undo-directory=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> NULL (&gt;= 10.2.2), `.` (&lt;= 10.2.1)
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_undo_log_truncate`

- <strong>Description:</strong> When enabled, undo tablespaces that are larger than [innodb_max_undo_log_size](#innodb_max_undo_log_size) are marked for truncation. See also [innodb_purge_rseg_truncate_frequency](#innodb_purge_rseg_truncate_frequency).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-undo-log-truncate[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `innodb_undo_logs`

- <strong>Description:</strong> Specifies the number of rollback segments that XtraDB/InnoDB will use within a transaction (or the number of active [undo logs](/kb/en/undo-log/)). By default set to the maximum, `128`, it can be reduced to avoid allocating unneeded rollback segments. See the [Innodb_available_undo_logs](/kb/en/xtradbinnodb-server-status-variables/#innodb_available_undo_logs) status variable for the number of undo logs available. See also [innodb_undo_directory](#innodb_undo_directory) and [innodb_undo_tablespaces](#innodb_undo_tablespaces). Replaces [innodb_rollback_segments](#innodb_rollback_segments) in [MariaDB 10.0](/kb/en/what-is-mariadb-100/). The [Information Schema XTRADB_RSEG Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-xtradb-tables/information-schema-xtradb_rseg-table/) contains information about the XtraDB rollback segments. Deprecated and ignored in [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/), as it always makes sense to use the maximum number of rollback segments.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-undo-logs=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `128`
- <strong>Range:</strong> `0` to `128`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `innodb_undo_tablespaces`

- <strong>Description:</strong> Number of tablespaces files used for dividing up the [undo logs](/kb/en/undo-log/). By default, undo logs are all part of the system tablespace, which contains one undo tablespace more than the `innodb_undo_tablespaces` setting. When the undo logs can grow large, splitting them over multiple tablespaces will reduce the size of any single tablespace. Must be set before InnoDB is initialized, or else MariaDB will fail to start, with an error saying that `InnoDB did not find the expected number of undo tablespaces`. The files are created in the directory specified by [innodb_undo_directory](/kb/en/xtradbinnodb-server-system-variables/#innodb_undo_directory), and are named `undoN`, N being an integer. The default size of an undo tablespace is 10MB. [innodb_undo_logs](/kb/en/xtradbinnodb-server-system-variables/#innodb_undo_logs) must have a non-zero setting for `innodb_undo_tablespaces` to take effect.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-undo-tablespaces=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `95` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)), `0` to `126` (&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `innodb_use_atomic_writes`

- <strong>Description:</strong> Implement atomic writes on supported SSD devices. See [atomic write support](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-performance-advanced-configurations/atomic-write-support/) for other variables affected when this is set.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-use-atomic-writes={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)), `OFF` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))

---

#### `innodb_use_fallocate`

- <strong>Description:</strong> Preallocate files fast, using operating system functionality. On POSIX systems, posix_fallocate system call is used.
<ul start="1"><li>Automatically set to `1` when [innodb_use_atomic_writes](#innodb_use_atomic_writes) is set - see [FusionIO DirectFS atomic write support](/kb/en/fusionio-directfs-atomic-write-support/).
</li><li>See [InnoDB Page Compression: Saving Storage Space with Sparse Files](/kb/en/innodb-page-compression/#saving-storage-space-with-sparse-files) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-use-fallocate={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Deprecated:</strong> [MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/) (treated as if `ON`)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_use_global_flush_log_at_trx_commit`

- <strong>Description:</strong> Determines whether a user can set the variable [innodb_flush_log_at_trx_commit](#innodb_flush_log_at_trx_commit). If set to `1`, a user cannot reset the value with a SET command, while if set to `1`, a user can reset the value of `innodb_flush_log_at_trx_commit`. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-use-global-flush-log-at-trx_commit={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_use_mtflush`

- <strong>Description:</strong> Whether to enable Multi-Threaded Flush operations.
For more information, see Fusion. 
<ul start="1"><li>InnoDB's multi-thread flush feature was deprecated in [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/) and removed from [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/). In later versions of MariaDB, use <a undefined>innodb_page_cleaners</a> system variable instead.
</li><li>See [InnoDB Page Flushing: Page Flushing with Multi-threaded Flush Threads](/kb/en/innodb-page-flushing/#page-flushing-with-multi-threaded-flush-threads) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-use-mtflush=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/)

---

#### `innodb_use_native_aio`

- <strong>Description:</strong> For Linux systems only, specified whether to use Linux's asynchronous I/O subsystem. Set to `1` by default, it may be changed to `0` at startup if InnoDB detects a problem
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-use-native-aio=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `innodb_use_purge_thread`

- <strong>Description:</strong> Usually with InnoDB, data changed by a transaction is written to an undo space to permit read consistency, and freed when the transaction is complete. Many, or large, transactions, can cause the main tablespace to grow dramatically, reducing performance. This option, introduced in XtraDB 5.1 and removed for 5.5, allows multiple threads to perform the purging, resulting in slower, but much more stable performance.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-use-purge-thread=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `32`
- <strong>Removed:</strong> XtraDB 5.5

---

#### `innodb_use_stacktrace`

- <strong>Description:</strong> If set to `ON` (`OFF` is default), a signal handler for SIGUSR2 is installed when the InnoDB server starts. When a long semaphore wait is detected at sync/sync0array.c, a SIGUSR2 signal is sent to the waiting thread and thread that has acquired the RW-latch. For both threads a full stacktrace is produced as well as if possible. XtraDB only. Added as a deprecated and ignored option in [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) (which uses InnoDB as default instead of XtraDB) to allow for easier upgrades.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-use-stacktrace=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Deprecated:</strong> [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_use_sys_malloc`

- <strong>Description:</strong> If set the `1`, the default, XtraDB/InnoDB will use the operating system's memory allocator. If set to `0` it will use its own. Deprecated in [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and removed in [MariaDB 10.2](/kb/en/what-is-mariadb-102/) along with InnoDB's internal memory allocator.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-use-sys-malloc=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Deprecated:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)
- <strong>Removed</strong>: [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `innodb_use_sys_stats_table`

- <strong>Description:</strong> If set to `1` (`0` is default), XtraDB will use the SYS_STATS system table for extra table index statistics. When a table is opened for the first time, statistics will then be loaded from SYS_STATS instead of sampling the index pages. Statistics are designed to be maintained only by running an [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/). Replaced by MySQL 5.6's Persistent Optimizer Statistics.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">innodb-use-sys-stats-table={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)/XtraDB 5.6

---

#### `innodb_use_trim`

- <strong>Description:</strong> Use trim to free up space of compressed blocks.
<ul start="1"><li>See [InnoDB Page Compression: Saving Storage Space with Sparse Files](/kb/en/innodb-page-compression/#saving-storage-space-with-sparse-files) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-use-trim=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)), `OFF` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/), [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/) Fusion-io
- <strong>Deprecated:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `innodb_version`

- <strong>Description:</strong> InnoDB version number. From [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/), as the InnoDB implementation in MariaDB has diverged from MySQL, the MariaDB version is instead reported. For example, the InnoDB version reported in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) (which is based on MySQL 5.6) included encryption and variable-size page compression before MySQL 5.7 introduced them. [MariaDB 10.2](/kb/en/what-is-mariadb-102/) (based on MySQL 5.7) introduced persistent AUTO_INCREMENT ([MDEV-6076](https://jira.mariadb.org/browse/MDEV-6076)) in a GA release before MySQL 8.0. [MariaDB 10.3](/kb/en/what-is-mariadb-103/) (based on MySQL 5.7) introduced instant ADD COLUMN ([MDEV-11369](https://jira.mariadb.org/browse/MDEV-11369)) before MySQL.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---

#### `innodb_write_io_threads`

- <strong>Description:</strong> Number of I/O threads for XtraDB/InnoDB writes. You may on rare occasions need to reduce this default on Linux systems running multiple MariaDB servers to avoid exceeding system limits.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-write-io-threads=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4`
- <strong>Range:</strong> `1` to `64`

---