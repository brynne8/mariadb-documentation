# MyRocks System Variables

This page documents system variables related to the [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/) storage engine. See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting them.

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `rocksdb_access_hint_on_compaction_start`

- <strong>Description:</strong> DBOptions::access_hint_on_compaction_start for RocksDB
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-access-hint-on-compaction-start=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `3`

---

#### `rocksdb_advise_random_on_open`

- <strong>Description:</strong> DBOptions::advise_random_on_open for RocksDB
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-advise-random-on-open={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_allow_concurrent_memtable_write`

- <strong>Description:</strong> DBOptions::allow_concurrent_memtable_write for RocksDB
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-allow-concurrent-memtable-write={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_allow_mmap_reads`

- <strong>Description:</strong> DBOptions::allow_mmap_reads for RocksDB
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-allow-mmap-reads={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_allow_mmap_writes`

- <strong>Description:</strong> DBOptions::allow_mmap_writes for RocksDB
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-allow-mmap-writes={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_allow_to_start_after_corruption`

- <strong>Description:</strong> Allow server still to start successfully even if RocksDB corruption is detected.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-allow-to-start-after-corruption={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/), [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/)

---

#### `rocksdb_background_sync`

- <strong>Description:</strong> Turns on background syncs for RocksDB
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-background-sync={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Removed:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_base_background_compactions`

- <strong>Description:</strong> DBOptions::base_background_compactions for RocksDB
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-base-background-compactions=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `-1` to `64`
- <strong>Removed:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_blind_delete_primary_key`

- <strong>Description:</strong> Deleting rows by primary key lookup, without reading rows (Blind Deletes). Blind delete is disabled if the table has secondary key.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-blind-delete-primary-key={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_block_cache_size`

- <strong>Description:</strong> Block_cache size for RocksDB (block size 1024)
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-block-cache-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `536870912`
- <strong>Range:</strong> `1024` to `9223372036854775807`

To see the statistics of block cache usage, check `SHOW ENGINE ROCKSDB STATUS` output
(search for lines starting with `rocksdb.block.cache`).

One can check the size of data of the block cache  in `DB_BLOCK_CACHE_USAGE`
column of the `INFORMATION_SCHEMA.ROCKSDB_DBSTATS` table.

---

#### `rocksdb_block_restart_interval`

- <strong>Description:</strong> BlockBasedTableOptions::block_restart_interval for RocksDB
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-block-restart-interval=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `16`
- <strong>Range:</strong> `1` to `2147483647`

---

#### `rocksdb_block_size`

- <strong>Description:</strong> BlockBasedTableOptions::block_size for RocksDB
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-block-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4096`
- <strong>Range:</strong> `1` to `18446744073709551615`

---

#### `rocksdb_block_size_deviation`

- <strong>Description:</strong> BlockBasedTableOptions::block_size_deviation for RocksDB
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-block-size-deviation=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `0` to `2147483647`

---

#### `rocksdb_bulk_load`

- <strong>Description:</strong> Use bulk-load mode for inserts. This disables unique_checks and enables rocksdb_commit_in_the_middle.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-bulk-load={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_bulk_load_allow_sk`

- <strong>Description:</strong> Allow bulk loading of sk keys during bulk-load. Can be changed only when bulk load is disabled.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-bulk-load_allow_sk={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/)

---

#### `rocksdb_bulk_load_allow_unsorted`

- <strong>Description:</strong> Allow unsorted input during bulk-load. Can be changed only when bulk load is disabled.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-bulk-load_allow_unsorted={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_bulk_load_size`

- <strong>Description:</strong> Maximum number of records in a batch for bulk-load mode.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-bulk-load-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1000`
- <strong>Range:</strong> `1` to `1073741824`

---

#### `rocksdb_bytes_per_sync`

- <strong>Description:</strong> DBOptions::bytes_per_sync for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-bytes-per-sync=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_cache_dump`

- <strong>Description:</strong> Include RocksDB block cache content in core dump.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-cache-dump={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/)

---

#### `rocksdb_cache_high_pri_pool_ratio`

- <strong>Description:</strong> Specify the size of block cache high-pri pool.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-cache-high-pri-pool-ratio=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `double`
- <strong>Default Value:</strong> `0.000000`
- <strong>Range:</strong> `0` to `1`
- <strong>Introduced:</strong> [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/)

---

#### `rocksdb_cache_index_and_filter_blocks`

- <strong>Description:</strong> BlockBasedTableOptions::cache_index_and_filter_blocks for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-cache-index-and-filter-blocks={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_cache_index_and_filter_with_high_priority`

- <strong>Description:</strong> cache_index_and_filter_blocks_with_high_priority for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-cache-index-and-filter-with-high-priority={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/)

---

#### `rocksdb_checksums_pct`

- <strong>Description:</strong> Percentage of rows to be checksummed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-checksums-pct=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100`
- <strong>Range:</strong> `0` to `100`

---

#### `rocksdb_collect_sst_properties`

- <strong>Description:</strong> Enables collecting SST file properties on each flush.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-collect-sst-properties={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_commit_in_the_middle`

- <strong>Description:</strong> Commit rows implicitly every rocksdb_bulk_load_size, on bulk load/insert, update and delete.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-commit-in-the-middle={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_commit_time_batch_for_recovery`

- <strong>Description:</strong> TransactionOptions::commit_time_batch_for_recovery for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-commit-time-batch-for-recovery={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/)

---

#### `rocksdb_compact_cf`

- <strong>Description:</strong> Compact column family.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-compact-cf=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty)

---

#### `rocksdb_compaction_readahead_size`

- <strong>Description:</strong> DBOptions::compaction_readahead_size for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-compaction-readahead-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_compaction_sequential_deletes`

- <strong>Description:</strong> RocksDB will trigger compaction for the file if it has more than this number sequential deletes per window.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-compaction-sequential-deletes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `2000000`

---

#### `rocksdb_compaction_sequential_deletes_count_sd`

- <strong>Description:</strong> Counting SingleDelete as rocksdb_compaction_sequential_deletes.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-compaction-sequential-deletes-count-sd={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_compaction_sequential_deletes_file_size`

- <strong>Description:</strong> Minimum file size required for compaction_sequential_deletes.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-compaction-sequential-deletes-file-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `-1` to `9223372036854775807`

---

#### `rocksdb_compaction_sequential_deletes_window`

- <strong>Description:</strong> Size of the window for counting rocksdb_compaction_sequential_deletes.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-compaction-sequential-deletes-window=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `2000000`

---

#### `rocksdb_concurrent_prepare`

- <strong>Description:</strong> DBOptions::concurrent_prepare for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-coconcurrent-prepare={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`
- <strong>Removed:</strong> [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/), [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/)

---

#### `rocksdb_create_checkpoint`

- <strong>Description:</strong> Checkpoint directory.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-create-checkpoint=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty)

---

#### `rocksdb_create_if_missing`

- <strong>Description:</strong> DBOptions::create_if_missing for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-create-if-missing={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_create_missing_column_families`

- <strong>Description:</strong> DBOptions::create_missing_column_families for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-create-missing-column-families={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_datadir`

- <strong>Description:</strong> RocksDB data directory.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-datadir[=value]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `./#rocksdb`

---

#### `rocksdb_db_write_buffer_size`

- <strong>Description:</strong> DBOptions::db_write_buffer_size for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-db-write-buffer-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_deadlock_detect`

- <strong>Description:</strong> Enables deadlock detection.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-deadlock-detect={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_deadlock_detect_depth`

- <strong>Description:</strong> Number of transactions deadlock detection will traverse through before assuming deadlock.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-deadlock-detect-depth=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `50`
- <strong>Range:</strong> `2` to `18446744073709551615`

---

#### `rocksdb_debug_manual_compaction_delay`

- <strong>Description:</strong> For debugging purposes only. Sleeping specified seconds for simulating long running compactions.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-debug_manual_compaction_delay=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/)

---

#### `rocksdb_debug_optimizer_no_zero_cardinality`

- <strong>Description:</strong> If cardinality is zero, override it with some value.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-debug-optimizer-no-zero-cardinality={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_debug_ttl_ignore_pk`

- <strong>Description:</strong> For debugging purposes only. If true, compaction filtering will not occur on PK TTL data. This variable is a no-op in non-debug builds.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-debug-ttl-ignore-pk={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_debug_ttl_read_filter_ts`

- <strong>Description:</strong> For debugging purposes only.  Overrides the TTL read filtering time to time + debug_ttl_read_filter_ts. A value of 0 denotes that the variable is not set. This variable is a no-op in non-debug builds.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-debug-ttl-read-filter-ts=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `-3600` to `3600`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_debug_ttl_rec_ts`

- <strong>Description:</strong> For debugging purposes only.  Overrides the TTL of records to now() + debug_ttl_rec_ts. The value can be +/- to simulate a record inserted in the past vs a record inserted in the 'future'. A value of 0 denotes that the variable is not set. This variable is a no-op in non-debug builds.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-debug-ttl-read-filter-ts=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `-3600` to `3600`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_debug_ttl_snapshot_ts`

- <strong>Description:</strong> For debugging purposes only. Sets the snapshot during compaction to now() + debug_set_ttl_snapshot_ts. The value can be positive or negative to simulate a snapshot in the past vs a snapshot created in the 'future'. A value of 0 denotes that the variable is not set. This variable is a no-op in non-debug builds.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-debug-ttl-snapshot-ts=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `-3600` to `3600`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_default_cf_options`

- <strong>Description:</strong> Default cf options for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-default-cf-options=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty)

---

#### `rocksdb_delayed_write_rate`

- <strong>Description:</strong> DBOptions::delayed_write_rate.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-delayed-write-rate=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0` (Previously `16777216`)
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_delete_cf`

- <strong>Description:</strong> Delete column family.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-delete-cf=val</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty string)
- <strong>Introduced:</strong> [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/)

---

#### `rocksdb_delete_obsolete_files_period_micros`

- <strong>Description:</strong> DBOptions::delete_obsolete_files_period_micros for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-delete-obsolete-files-period-micros=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `21600000000`
- <strong>Range:</strong> `0` to `9223372036854775807`

---

#### `rocksdb_enable_2pc`

- <strong>Description:</strong> Enable two phase commit for MyRocks. When set, MyRocks will keep its data consistent with the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) (in other words, the server will be a crash-safe master). The consistency is achieved by doing two-phase XA commit with the binary log.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-enable-2pc={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_enable_bulk_load_api`

- <strong>Description:</strong> Enables using SstFileWriter for bulk loading.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-enable-bulk-load-api={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_enable_insert_with_update_caching`

- <strong>Description:</strong> Whether to enable optimization where we cache the read from a failed insertion attempt in [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-enable-insert-with-update-caching={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/)

---

#### `rocksdb_enable_thread_tracking`

- <strong>Description:</strong> DBOptions::enable_thread_tracking for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-enable-thread-tracking={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_enable_ttl`

- <strong>Description:</strong> Enable expired TTL records to be dropped during compaction.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-enable-ttl={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_enable_ttl_read_filtering`

- <strong>Description:</strong> For tables with TTL, expired records are skipped/filtered out during processing and in query results. Disabling this will allow these records to be seen, but as a result rows may disappear in the middle of transactions as they are dropped during compaction. Use with caution.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-enable-ttl-read-filtering={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_enable_write_thread_adaptive_yield`

- <strong>Description:</strong> DBOptions::enable_write_thread_adaptive_yield for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-enable-write-thread-adaptive-yield={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_error_if_exists`

- <strong>Description:</strong> DBOptions::error_if_exists for RocksDBB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-error-if-exists={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_error_on_suboptimal_collation`

- <strong>Description:</strong> Raise an error instead of warning if a sub-optimal collation is used.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-error-on-suboptimal-collation={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/)

---

#### `rocksdb_flush_log_at_trx_commit`

- <strong>Description:</strong> Sync on transaction commit. Similar to [innodb_flush_log_at_trx_commit](/kb/en/innodb-system-variables/#innodb_flush_log_at_trx_commit). 1: sync on commit, 0,2: not sync on commit. One can check the flushing by examining the [rocksdb_wal_synced](/kb/en/myrocks-status-variables/#rocksdb_wal_synced) and [rocksdb_wal_bytes](/kb/en/myrocks-status-variables/#rocksdb_wal_bytes) status variables.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-flush-log-at-trx-commit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `2`

---

#### `rocksdb_flush_memtable_on_analyze`

- <strong>Description:</strong> Forces memtable flush on ANALZYE table to get accurate cardinality.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-flush-memtable-on-analyze={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Removed:</strong> [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/), [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/)

---

#### `rocksdb_force_compute_memtable_stats`

- <strong>Description:</strong> Force to always compute memtable stats.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-force-compute-memtable-stats={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_force_compute_memtable_stats_cachetime`

- <strong>Description:</strong> Time in usecs to cache memtable estimates.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-force-compute-memtable-stats-cachetime=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `60000000`
- <strong>Range:</strong> `0` to `2147483647`

---

#### `rocksdb_force_flush_memtable_and_lzero_now`

- <strong>Description:</strong> Acts similar to force_flush_memtable_now, but also compacts all L0 files.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-force-flush-memtable-and-lzero-now={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_force_flush_memtable_now`

- <strong>Description:</strong> Forces memstore flush which may block all write requests so be careful.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-force-flush-memtable-now={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_force_index_records_in_range`

- <strong>Description:</strong> Used to override the result of records_in_range() when [FORCE INDEX](/replication/optimization-and-tuning/query-optimizations/force-index/) is used.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-force-index-records-in-range=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `2147483647`

---

#### `rocksdb_git_hash`

- <strong>Description:</strong> Git revision of the RocksDB library used by MyRocks.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-git-hash=value=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> As per git revision.

---

#### `rocksdb_hash_index_allow_collision`

- <strong>Description:</strong> BlockBasedTableOptions::hash_index_allow_collision for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-hash-index-allow-collision={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_ignore_unknown_options`

- <strong>Description:</strong> Enable ignoring unknown options passed to RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-ignore-unknown-options={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/), [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/)

---

#### `rocksdb_index_type`

- <strong>Description:</strong> BlockBasedTableOptions::index_type for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-index-type=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `kBinarySearch`
- <strong>Valid Values:</strong> `kBinarySearch`, `kHashSearch`

---

#### `rocksdb_info_log_level`

- <strong>Description:</strong> Filter level for info logs to be written mysqld error log. Valid values include 'debug_level', 'info_level', 'warn_level', 'error_level' and 'fatal_level'.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-info-log-level=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `error_level`
- <strong>Valid Values:</strong> `error_level`, `debug_level`, `info_level`, `warn_level`, `fatal_level`

---

#### `rocksdb_io_write_timeout`

- <strong>Description:</strong> Timeout for experimental I/O watchdog.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-io-write-timeout=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Valid Values:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_is_fd_close_on_exec`

- <strong>Description:</strong> DBOptions::is_fd_close_on_exec for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-is-fd-close-on-exec={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_keep_log_file_num`

- <strong>Description:</strong> DBOptions::keep_log_file_num for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-keep-log-file-num=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1000`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_large_prefix`

- <strong>Description:</strong> Support large index prefix length of 3072 bytes. If off, the maximum index prefix length is 767.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-large_prefix={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_lock_scanned_rows`

- <strong>Description:</strong> Take and hold locks on rows that are scanned but not updated.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-lock-scanned-rows={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_lock_wait_timeout`

- <strong>Description:</strong> Number of seconds to wait for lock.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-lock-wait-timeout=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` to `1073741824`

---

#### `rocksdb_log_file_time_to_roll`

- <strong>Description:</strong> DBOptions::log_file_time_to_roll for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-log-file-time-to_roll=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_manifest_preallocation_size`

- <strong>Description:</strong> DBOptions::manifest_preallocation_size for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-manifest-preallocation-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4194304`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_manual_compaction_threads`

- <strong>Description:</strong> How many rocksdb threads to run for manual compactions.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-manual-compation-threads=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `128`
- <strong>Introduced:</strong> [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/)

---

#### `rocksdb_manual_wal_flush`

- <strong>Description:</strong> DBOptions::manual_wal_flush for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-manual-wal-flush={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_master_skip_tx_api`

- <strong>Description:</strong> Skipping holding any lock on row access. Not effective on slave.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-master-skip-tx-api={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_max_background_compactions`

- <strong>Description:</strong> DBOptions::max_background_compactions for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-max-background-compactions=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` to `64`
- <strong>Removed:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_max_background_flushes`

- <strong>Description:</strong> DBOptions::max_background_flushes for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-max-background-flushes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` to `64`
- <strong>Removed:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_max_background_jobs`

- <strong>Description:</strong> DBOptions::max_background_jobs for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-max-background-jobs=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `2`
- <strong>Range:</strong> `-1` to `64`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_max_latest_deadlocks`

- <strong>Description:</strong> Maximum number of recent deadlocks to store.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-max-latest-deadlocks=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `5`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `rocksdb_max_log_file_size`

- <strong>Description:</strong> DBOptions::max_log_file_size for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-max-log-file-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_max_manifest_file_size`

- <strong>Description:</strong> DBOptions::max_manifest_file_size for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-manifest-log-file-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1073741824`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_max_manual_compactions`

- <strong>Description:</strong> Maximum number of pending + ongoing number of manual compactions..
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-manual_compactions=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/)

---

#### `rocksdb_max_open_files`

- <strong>Description:</strong> DBOptions::max_open_files for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-max-open-files=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-2`
- <strong>Range:</strong> `-2` to `2147483647`

---

#### `rocksdb_max_row_locks`

- <strong>Description:</strong> Maximum number of locks a transaction can have.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-max-row-locks=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1048576`
- <strong>Range:</strong>
<ul start="1"><li>`1` to `1073741824` (&gt;= [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/)) 
</li><li>`1` to `1048576` (&lt;= [MariaDB 10.3.9](/kb/en/mariadb-1039-release-notes/), [MariaDB 10.2.17](/kb/en/mariadb-10217-release-notes/))
</li></ul>

---

#### `rocksdb_max_subcompactions`

- <strong>Description:</strong> DBOptions::max_subcompactions for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-max-subcompactions=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` to `64`

---

#### `rocksdb_max_total_wal_size`

- <strong>Description:</strong> DBOptions::max_total_wal_size for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-max-total-wal-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `9223372036854775807`

---

#### `rocksdb_merge_buf_size`

- <strong>Description:</strong> Size to allocate for merge sort buffers written out to disk during inplace index creation.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-merge-buf-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `67108864`
- <strong>Range:</strong> `100` to `18446744073709551615`

---

#### `rocksdb_merge_combine_read_size`

- <strong>Description:</strong> Size that we have to work with during combine (reading from disk) phase of external sort during fast index creation.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-merge-combine-read-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1073741824`
- <strong>Range:</strong> `100` to `18446744073709551615`

---

#### `rocksdb_merge_tmp_file_removal_delay_ms`

- <strong>Description:</strong> Fast index creation creates a large tmp file on disk during index creation.  Removing this large file all at once when index creation is complete can cause trim stalls on Flash. This variable specifies a duration to sleep (in milliseconds) between calling chsize() to truncate the file in chunks. The chunk size is  the same as merge_buf_size.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-merge-tmp-file-removal-delay-ms=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_new_table_reader_for_compaction_inputs`

- <strong>Description:</strong> DBOptions::new_table_reader_for_compaction_inputs for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-new-table-reader-for-compaction-inputs={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_no_block_cache`

- <strong>Description:</strong> BlockBasedTableOptions::no_block_cache for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-no-block-cache={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_override_cf_options`

- <strong>Description:</strong> Option overrides per cf for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-override-cf-options=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty)

---

#### `rocksdb_paranoid_checks`

- <strong>Description:</strong> DBOptions::paranoid_checks for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-paranoid-checks={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_pause_background_work`

- <strong>Description:</strong> Disable all rocksdb background operations.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-pause-background-work={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_perf_context_level`

- <strong>Description:</strong> Perf Context Level for rocksdb internal timer stat collection.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-perf-context-level=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `5`

---

#### `rocksdb_persistent_cache_path`

- <strong>Description:</strong> Path for BlockBasedTableOptions::persistent_cache for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-persistent-cache-path=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty)

---

#### `rocksdb_persistent_cache_size_mb`

- <strong>Description:</strong> Size of cache in MB for BlockBasedTableOptions::persistent_cache for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-persistent-cache-size-mb=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_pin_l0_filter_and_index_blocks_in_cache`

- <strong>Description:</strong> pin_l0_filter_and_index_blocks_in_cache for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-pin-l0-filter-and-index-blocks-in-cache={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_print_snapshot_conflict_queries`

- <strong>Description:</strong> Logging queries that got snapshot conflict errors into *.err log.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-print-snapshot-conflict-queries={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_rate_limiter_bytes_per_sec`

- <strong>Description:</strong> DBOptions::rate_limiter bytes_per_sec for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-rate-limiter-bytes-per-sec=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `9223372036854775807`

---

#### `rocksdb_read_free_rpl_tables`

- <strong>Description:</strong> List of tables that will use read-free replication on the slave (i.e. not lookup a row during replication).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-read-free-rpl-tables=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty)
- <strong>Removed:</strong> [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/)

---

#### `rocksdb_records_in_range`

- <strong>Description:</strong> Used to override the result of records_in_range(). Set to a positive number to override.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-records-in-range=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `2147483647`

---

#### `rocksdb_remove_mariabackup_checkpoint`

- <strong>Description:</strong> Remove [mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/) checkpoint.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-remove-mariabackup-checkpoint={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.3.8](/kb/en/mariadb-1038-release-notes/), [MariaDB 10.2.16](/kb/en/mariadb-10216-release-notes/)

---

#### `rocksdb_reset_stats`

- <strong>Description:</strong> Reset the RocksDB internal statistics without restarting the DB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-reset-stats={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_rollback_on_timeout`

- <strong>Description:</strong> Whether to roll back the complete transaction or a single statement on lock wait timeout (a single statement by default).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-rollback-on-timeout={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/)

---

#### `rocksdb_seconds_between_stat_computes`

- <strong>Description:</strong> Sets a number of seconds to wait between optimizer stats recomputation. Only changed indexes will be refreshed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-seconds-between-stat-computes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `3600`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `rocksdb_signal_drop_index_thread`

- <strong>Description:</strong> Wake up drop index thread.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-signal-drop-index-thread={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_sim_cache_size`

- <strong>Description:</strong> Simulated cache size for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-sim-cache-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `9223372036854775807`

---

#### `rocksdb_skip_bloom_filter_on_read`

- <strong>Description:</strong> Skip using bloom filter for reads.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-skip-bloom-filter-on_read={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_skip_fill_cache`

- <strong>Description:</strong> Skip filling block cache on read requests.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-skip-fill-cache={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_skip_unique_check_tables`

- <strong>Description:</strong> Skip unique constraint checking for the specified tables.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-skip-unique-check-tables=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `.*`

---

#### `rocksdb_sst_mgr_rate_bytes_per_sec`

- <strong>Description:</strong> DBOptions::sst_file_manager rate_bytes_per_sec for RocksDB
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-sst-mgr-rate-bytes-per-sec=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_stats_dump_period_sec`

- <strong>Description:</strong> DBOptions::stats_dump_period_sec for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-stats-dump-period-sec=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `600`
- <strong>Range:</strong> `0` to `2147483647`

---

#### `rocksdb_stats_level`

- <strong>Description:</strong> Statistics Level for RocksDB. Default is 0 (kExceptHistogramOrTimers).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-stats-level=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4`
- <strong>Introduced:</strong> [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/)

---

#### `rocksdb_stats_recalc_rate`

- <strong>Description:</strong> The number of indexes per second to recalculate statistics for. 0 to disable background recalculation.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-stats-recalc_rate=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/) [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/)

---

#### `rocksdb_store_row_debug_checksums`

- <strong>Description:</strong> Include checksums when writing index/table records.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-store-row-debug-checksums={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_strict_collation_check`

- <strong>Description:</strong> Enforce case sensitive collation for MyRocks indexes.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-strict-collation-check={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_strict_collation_exceptions`

- <strong>Description:</strong> List of tables (using regex) that are excluded from the case sensitive collation enforcement.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-strict-collation-exceptions=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty)

---

#### `rocksdb_supported_compression_types`

- <strong>Description:</strong> Compression algorithms supported by RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-supported-compression-types=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `Snappy,Zlib`

---

#### `rocksdb_table_cache_numshardbits`

- <strong>Description:</strong> DBOptions::table_cache_numshardbits for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-table-cache-numshardbits=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `6`
- <strong>Range:</strong> `0` to `19`

---

#### `rocksdb_table_stats_sampling_pct`

- <strong>Description:</strong> Percentage of entries to sample when collecting statistics about table properties. Specify either 0 to sample everything or percentage [1..100]. By default 10% of entries are sampled.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-table-stats-sampling-pct=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `0` to `100`

---

#### `rocksdb_tmpdir`

- <strong>Description:</strong> Directory for temporary files during DDL operations.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-tmpdir[=value]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty)

---

#### `rocksdb_trace_sst_api`

- <strong>Description:</strong> Generate trace output in the log for each call to the SstFileWriter.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-trace-sst-api={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_two_write_queues`

- <strong>Description:</strong> DBOptions::two_write_queues for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-two-write-queues={0|1}</code>
- <strong>Scope:</strong> Global,
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/), [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/)

---

#### `rocksdb_unsafe_for_binlog`

- <strong>Description:</strong> Allowing statement based binary logging which may break consistency.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-unsafe-for-binlog={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_update_cf_options`

- <strong>Description:</strong> Option updates per column family for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-update-cf-options=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `varchar`
- <strong>Default Value:</strong> (Empty)
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_use_adaptive_mutex`

- <strong>Description:</strong> DBOptions::use_adaptive_mutex for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-use-adaptive-mutex={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_use_clock_cache`

- <strong>Description:</strong> Use ClockCache instead of default LRUCache for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-use-clock-cache={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_use_direct_io_for_flush_and_compaction`

- <strong>Description:</strong> DBOptions::use_direct_io_for_flush_and_compaction for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-use-direct-io-for-flush-and-compaction={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_use_direct_reads`

- <strong>Description:</strong> DBOptions::use_direct_reads for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-use-direct-reads={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_use_direct_writes`

- <strong>Description:</strong> DBOptions::use_direct_writes for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-use-direct-reads={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Removed:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `rocksdb_use_fsync`

- <strong>Description:</strong> DBOptions::use_fsync for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-use-fsync={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_validate_tables`

- <strong>Description:</strong> Verify all .frm files match all RocksDB tables (0 means no verification, 1 means verify and fail on error, and 2 means verify but continue.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-validate-tables=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `2`

---

#### `rocksdb_verify_row_debug_checksums`

- <strong>Description:</strong> Verify checksums when reading index/table records.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-verify-row-debug-checksums={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_wal_bytes_per_sync`

- <strong>Description:</strong> DBOptions::wal_bytes_per_sync for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-wal-bytes-per-sync=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_wal_dir`

- <strong>Description:</strong> DBOptions::wal_dir for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-wal-dir=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty)

---

#### `rocksdb_wal_recovery_mode`

- <strong>Description:</strong> DBOptions::wal_recovery_mode for RocksDB. Default is kAbsoluteConsistency.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-wal-recovery-mode=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `3`

---

#### `rocksdb_wal_size_limit_mb`

- <strong>Description:</strong> DBOptions::WAL_size_limit_MB for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-wal-size-limit-mb=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `9223372036854775807`

---

#### `rocksdb_wal_ttl_seconds`

- <strong>Description:</strong> DBOptions::WAL_ttl_seconds for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-wal-ttl-seconds=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `9223372036854775807`

---

#### `rocksdb_whole_key_filtering`

- <strong>Description:</strong> BlockBasedTableOptions::whole_key_filtering for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-whole-key-filtering={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `rocksdb_write_batch_max_bytes`

- <strong>Description:</strong> Maximum size of write batch in bytes. 0 means no limit.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-write-batch-max-bytes=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rocksdb_write_disable_wal`

- <strong>Description:</strong> WriteOptions::disableWAL for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-write-disable-wal={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_write_ignore_missing_column_families`

- <strong>Description:</strong> WriteOptions::ignore_missing_column_families for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-write-ignore-missing-column-families={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `rocksdb_write_policy`

- <strong>Description:</strong> DBOptions::write_policy for RocksDB.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rocksdb-write-policy=val</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `write_committed`
- <strong>Valid Values:</strong> `write_committed`, `write_prepared`, `write_unprepared`
- <strong>Introduced:</strong> [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/)

---