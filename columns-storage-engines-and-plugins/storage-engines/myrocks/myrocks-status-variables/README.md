# MyRocks Status Variables

This page documents status variables related to the [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks) storage engine. See [Server Status Variables](/replication/optimization-and-tuning/system-variables/server-status-variables) for a complete list of status variables that can be viewed with [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status).

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables).

#### `Rocksdb_block_cache_add`

- <strong>Description:</strong> Number of blocks added to the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_block_cache_add_failures`

- <strong>Description:</strong> Number of failures when adding blocks to Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_block_cache_bytes_read`

- <strong>Description:</strong> Bytes read from Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_block_cache_bytes_write`

- <strong>Description:</strong> Bytes written to Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_block_cache_data_add`

- <strong>Description:</strong> Number of data blocks added to the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_block_cache_data_bytes_insert`

- <strong>Description:</strong> Bytes added to the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_block_cache_data_hit`

- <strong>Description:</strong> Number of hits when accessing the data block from the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_block_cache_data_miss`

- <strong>Description:</strong> Number of misses when accessing the data block from the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_block_cache_filter_add`

- <strong>Description:</strong> Number of bloom filter blocks added to the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_block_cache_filter_bytes_evict`

- <strong>Description:</strong> Bytes of bloom filter blocks evicted from the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_block_cache_filter_bytes_insert`

- <strong>Description:</strong> Bytes of bloom filter blocks added to the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_block_cache_filter_hit`

- <strong>Description:</strong> Number of hits when accessing the filter block from the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_block_cache_filter_miss`

- <strong>Description:</strong>  Number of misses when accessing the filter block from the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_block_cache_hit`

- <strong>Description:</strong>  Total number of hits for the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_block_cache_index_add`

- <strong>Description:</strong> Number of index blocks added to Block Cache index.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_block_cache_index_bytes_evict`

- <strong>Description:</strong> Bytes of index blocks evicted from the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_block_cache_index_bytes_insert`

- <strong>Description:</strong> Bytes of index blocks added to the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_block_cache_index_hit`

- <strong>Description:</strong> Number of hits for the Block Cache index.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_block_cache_index_miss`

- <strong>Description:</strong> Number of misses for the Block Cache index.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_block_cache_miss`

- <strong>Description:</strong> Total number of misses for the Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_block_cachecompressed_hit`

- <strong>Description:</strong> Number of hits for the compressed Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_block_cachecompressed_miss`

- <strong>Description:</strong> Number of misses for the compressed Block Cache.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_bloom_filter_full_positive`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/), [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/)

---

#### `Rocksdb_bloom_filter_full_true_positive`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/), [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/)

---

#### `Rocksdb_bloom_filter_prefix_checked`

- <strong>Description:</strong> Number of times the Bloom Filter checked before creating an iterator on a file.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_bloom_filter_prefix_useful`

- <strong>Description:</strong> Number of times the Bloom Filter check used to avoid creating an iterator on a file.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_bloom_filter_useful`

- <strong>Description:</strong>  Number of times the Bloom Filter used instead of reading form file.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_bytes_read`

- <strong>Description:</strong> Total number of uncompressed bytes read from memtables, cache or table files.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_bytes_written`

- <strong>Description:</strong> Total number of uncompressed bytes written.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_compact_read_bytes`

- <strong>Description:</strong>  Number of bytes read during compaction.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_compact_write_bytes`

- <strong>Description:</strong> Number of bytes written during compaction.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_compaction_key_drop_new`

- <strong>Description:</strong> Number of keys dropped during compaction due their being overwritten by new values.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_compaction_key_drop_obsolete`

- <strong>Description:</strong> Number of keys dropped during compaction due to their being obsolete.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_compaction_key_drop_user`

- <strong>Description:</strong>  Number of keys dropped during compaction due to user compaction.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_covered_secondary_key_lookups`

- <strong>Description:</strong> Incremented when avoiding reading a record via a keyread. This indicates lookups that were performed via a secondary index containing a field that is only a prefix of the [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar) column, and that could return all requested fields directly from the secondary index.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_flush_write_bytes`

- <strong>Description:</strong> Number of bytes written during flush.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_get_hit_l0`

- <strong>Description:</strong> Number of times reads got data from the L0 compaction layer.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_get_hit_l1`

- <strong>Description:</strong> Number of times reads got data from the L1 compaction layer.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_get_hit_l2_and_up`

- <strong>Description:</strong> Number of times reads got data from the L2 and up compaction layer.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_getupdatessince_calls`

- <strong>Description:</strong> Number of calls to the `GetUpdatesSince` function. You may find this useful when monitoring refreshes of the transaction log.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_iter_bytes_read`

- <strong>Description:</strong> Total uncompressed bytes read from an iterator, including the size of both key and value.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_l0_num_files_stall_micros`

- <strong>Description:</strong>  Shows how long in microseconds throttled due to too mnay files in L0.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `Rocksdb_l0_slowdown_micros`

- <strong>Description:</strong> Total time spent waiting in microseconds while performing L0-L1 compactions.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `Rocksdb_manual_compactions_processed`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/), [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/)

---

#### `Rocksdb_manual_compactions_running`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/), [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/)

---

#### `Rocksdb_memtable_compaction_micros`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/)

---

#### `Rocksdb_memtable_hit`

- <strong>Description:</strong> Number of memtable hits.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_memtable_miss`

- <strong>Description:</strong> Number of memtable misses.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_memtable_total`

- <strong>Description:</strong> Memory used, in bytes, of all memtables.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_memtable_unflushed`

- <strong>Description:</strong> Memory used, in bytes, of all unflushed memtables.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_no_file_closes`

- <strong>Description:</strong> Number of times files were closed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_no_file_errors`

- <strong>Description:</strong> Number of errors encountered while trying to read data from an SST file.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_no_file_opens`

- <strong>Description:</strong>  Number of times files were opened.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_num_iterators`

- <strong>Description:</strong> Number of iterators currently open.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_block_not_compressed`

- <strong>Description:</strong> Number of uncompressed blocks.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_db_next`

- <strong>Description:</strong> Number of `next` calls.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_number_db_next_found`

- <strong>Description:</strong> Number of `next` calls that returned data.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_number_db_prev`

- <strong>Description:</strong> Number of `prev` calls.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_number_db_prev_found`

- <strong>Description:</strong> Number of `prev` calls that returned data.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_number_db_seek`

- <strong>Description:</strong> Number of `seek` calls.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_number_db_seek_found`

- <strong>Description:</strong> Number of `seek` calls that returned data.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_number_deletes_filtered`

- <strong>Description:</strong> Number of deleted records were not written to storage due to a nonexistent key.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_keys_read`

- <strong>Description:</strong>  Number of keys have been read.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_keys_updated`

- <strong>Description:</strong> Number of keys have been updated.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_keys_written`

- <strong>Description:</strong> Number of keys have been written.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_merge_failures`

- <strong>Description:</strong> Number of failures encountered while performing merge operator actions.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_multiget_bytes_read`

- <strong>Description:</strong> Number of bytes read during RocksDB `MultiGet()` calls.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_multiget_get`

- <strong>Description:</strong> Number of RocksDB `MultiGet()` requests made.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_multiget_keys_read`

- <strong>Description:</strong> Number of keys read through RocksDB `MultiGet()` calls.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_reseeks_iteration`

- <strong>Description:</strong> Number of reseeks that have occurred inside an iteration that skipped over a large number of keys with the same user key.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_sst_entry_delete`

- <strong>Description:</strong> Number of delete markers written.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_sst_entry_merge`

- <strong>Description:</strong> Number of merge keys written.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_sst_entry_other`

- <strong>Description:</strong> Number of keys written that are not delete, merge or put keys.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_sst_entry_put`

- <strong>Description:</strong> Number of put keys written.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_sst_entry_singledelete`

- <strong>Description:</strong> Number of single-delete keys written.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_superversion_acquires`

- <strong>Description:</strong> Number of times the superversion structure acquired. This is useful when tracking files for the database.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_superversion_cleanups`

- <strong>Description:</strong> Number of times the superversion structure performed cleanups.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_number_superversion_releases`

- <strong>Description:</strong> Number of times the superversion structure released.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_queries_point`

- <strong>Description:</strong> Number of single-row queries.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_queries_range`

- <strong>Description:</strong> Number of multi-row queries.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_row_lock_deadlocks`

- <strong>Description:</strong> Number of deadlocks.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_row_lock_wait_timeouts`

- <strong>Description:</strong> Number of row lock wait timeouts.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_rows_deleted`

- <strong>Description:</strong> Number of rows deleted.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_rows_deleted_blind`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_rows_expired`

- <strong>Description:</strong> Number of expired rows.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_rows_filtered`

- <strong>Description:</strong> Number of TTL filtered rows.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Rocksdb_rows_inserted`

- <strong>Description:</strong> Number of rows inserted.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_rows_read`

- <strong>Description:</strong> Number of rows read.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_rows_updated`

- <strong>Description:</strong> Number of rows updated.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_snapshot_conflict_errors`

- <strong>Description:</strong> Number of snapshot conflict errors that have occurred during transactions that forced a rollback.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_stall_l0_file_count_limit_slowdowns`

- <strong>Description:</strong> Write slowdowns due to L0 being near to full.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_stall_l0_file_count_limit_stops`

- <strong>Description:</strong> Write stops due to L0 being to full.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_stall_locked_l0_file_count_limit_slowdowns`

- <strong>Description:</strong> Write slowdowns due to L0 being near to full and L0 compaction in progress.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_stall_locked_l0_file_count_limit_stops`

- <strong>Description:</strong> Write stops due to L0 being full and L0 compaction in progress.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_stall_memtable_limit_slowdowns`

- <strong>Description:</strong> Write slowdowns due to approaching maximum permitted number of memtables.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.10](/kb/en/mariadb-10210-release-notes/), [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `Rocksdb_stall_memtable_limit_stops`

- <strong>Description:</strong> * <strong>Description:</strong> Write stops due to reaching maximum permitted number of memtables.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.10](/kb/en/mariadb-10210-release-notes/), [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `Rocksdb_stall_micros`

- <strong>Description:</strong> Time in microseconds that the writer had to wait for the compaction or flush to complete.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_stall_pending_compaction_limit_slowdowns`

- <strong>Description:</strong> Write slowdowns due to nearing the limit for the maximum number of pending compaction bytes.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_stall_pending_compaction_limit_stops`

- <strong>Description:</strong> Write stops due to reaching the limit for the maximum number of pending compaction bytes.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_stall_total_slowdowns`

- <strong>Description:</strong> Total number of write slowdowns.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_stall_total_stops`

- <strong>Description:</strong> Total number of write stops.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_system_rows_deleted`

- <strong>Description:</strong> Number of rows deleted from system tables.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_system_rows_inserted`

- <strong>Description:</strong> Number of rows inserted into system tables.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_system_rows_read`

- <strong>Description:</strong> Number of rows read from system tables.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_system_rows_updated`

- <strong>Description:</strong> Number of rows updated for system tables.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_wal_bytes`

- <strong>Description:</strong> Number of bytes written to WAL.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_wal_group_syncs`

- <strong>Description:</strong> Number of group commit WAL file syncs have occurred. This is provided by MyRocks and is not a view of a RocksDB counter. Increased in `rocksdb_flush_wal()`  when doing the `rdb-&gt;FlushWAL()` call.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_wal_synced`

- <strong>Description:</strong> Number of syncs made on RocksDB WAL file.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_write_other`

- <strong>Description:</strong> Number of writes processed by a thread other than the requesting thread.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_write_self`

- <strong>Description:</strong>  Number of writes processed by requesting thread.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_write_timedout`

- <strong>Description:</strong> Number of writes that timed out.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rocksdb_write_wal`

- <strong>Description:</strong> Number of write calls that requested WAL.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---