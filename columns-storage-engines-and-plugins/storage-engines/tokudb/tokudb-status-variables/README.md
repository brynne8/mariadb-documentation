# TokuDB Status Variables

TokuDB has been deprecated by its upstream maintainer. It is disabled from [MariaDB 10.5](/kb/en/what-is-mariadb-105/) and has been been removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/) - [MDEV-19780](https://jira.mariadb.org/browse/MDEV-19780). We recommend [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/) as a long-term migration path.

This page documents status variables related to the [TokuDB storage engine](/columns-storage-engines-and-plugins/storage-engines/tokudb/). See [Server Status Variables](/replication/optimization-and-tuning/system-variables/server-status-variables/) for a complete list of status variables that can be viewed with [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status/).

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `Tokudb_basement_deserialization_fixed_key`

- <strong>Description:</strong> Number of deserialized basement nodes where all keys were the same size. This leaves the basement in an optimal format  for in-memory workloads.

---

#### `Tokudb_basement_deserialization_variable_key`

- <strong>Description:</strong> Number of deserialized basement nodes where all keys had different size keys, which are not eligible for in-memory optimization.

---

#### `Tokudb_basements_decompressed_for_write`

- <strong>Description:</strong> Number of basement nodes decompressed for write operations.

---

#### `Tokudb_basements_decompressed_prefetch`

- <strong>Description:</strong> Number of basement nodes decompressed by a prefetch thread. See [tokudb_disable_prefetching](/kb/en/tokudb-system-variables/#tokudb_disable_prefetching).

---

#### `Tokudb_basements_decompressed_prelocked_range`

- <strong>Description:</strong> Number of basement nodes aggressively decompressed by queries.

---

#### `Tokudb_basements_decompressed_target_query`

- <strong>Description:</strong> Number of basement nodes decompressed for queries

---

#### `Tokudb_basements_fetched_for_write`

- <strong>Description:</strong> Number of basement nodes fetched for writes off the disk.

---

#### `Tokudb_basements_fetched_for_write_bytes`

- <strong>Description:</strong> Total basement node bytes fetched for writes off the disk.

---

#### `Tokudb_basements_fetched_for_write_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while fetching basement nodes from disk for writes.

---

#### `Tokudb_basements_fetched_prefetch`

- <strong>Description:</strong> Number of basement nodes fetched off the disk by a prefetch thread.

---

#### `Tokudb_basements_fetched_prefetch_bytes`

- <strong>Description:</strong> Total basement node bytes fetched off the disk by a prefetch thread..

---

#### `Tokudb_basements_fetched_prefetch_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while fetching off the disk by a prefetch thread.

---

#### `Tokudb_basements_fetched_prelocked_range`

- <strong>Description:</strong> Number of basement nodes aggressively fetched from disk.

---

#### `Tokudb_basements_fetched_prelocked_range_bytes`

- <strong>Description:</strong> Total basement node bytes aggressively fetched off the disk.

---

#### `Tokudb_basements_fetched_prelocked_range_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while aggressively fetching basement nodes from disk.

---

#### `Tokudb_basements_fetched_target_query`

- <strong>Description:</strong> Number of basement nodes fetched for queries off the disk.

---

#### `Tokudb_basements_fetched_target_query_bytes`

- <strong>Description:</strong> Total basement node bytes fetched for queries off the disk.

---

#### `Tokudb_basements_fetched_target_query_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while fetching basement nodes from disk for queries.

---

#### `Tokudb_broadcase_messages_injected_at_root`

- <strong>Description:</strong> Number of broadcast messages injected at root.

---

#### `Tokudb_buffers_decompressed_for_write`

- <strong>Description:</strong> Number of buffers decompressed for writes.

---

#### `Tokudb_buffers_decompressed_prefetch`

- <strong>Description:</strong> Number of buffers decompressed by a prefetch thread.

---

#### `Tokudb_buffers_decompressed_prelocked_range`

- <strong>Description:</strong> Number of buffers decompressed for queries.

---

#### `Tokudb_buffers_decompressed_target_query`

- <strong>Description:</strong> Number of buffers aggressively decompressed by queries.

---

#### `Tokudb_buffers_fetched_for_write`

- <strong>Description:</strong> Number of buffers fetched for write off the disk.

---

#### `Tokudb_buffers_fetched_for_write_bytes`

- <strong>Description:</strong> Total buffer bytes fetched for writes off the disk.

---

#### `Tokudb_buffers_fetched_for_write_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while fetching buffers from disk for writes.

---

#### `Tokudb_buffers_fetched_prefetch`

- <strong>Description:</strong> Number of buffers fetched for queries off the disk by a prefetch thread.

---

#### `Tokudb_buffers_fetched_prefetch_bytes`

- <strong>Description:</strong> Total buffer bytes fetched for queries off the disk by a prefetch thread.

---

#### `Tokudb_buffers_fetched_prefetch_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while fetching buffers from disk for queries by a prefetch thread.

---

#### `Tokudb_buffers_fetched_prelocked_range`

- <strong>Description:</strong> Number of buffers aggressively fetched for queries off the disk.

---

#### `Tokudb_buffers_fetched_prelocked_range_bytes`

- <strong>Description:</strong> Total buffer bytes aggressively fetched for queries off the disk.

---

#### `Tokudb_buffers_fetched_prelocked_range_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while aggressively fetching buffers from disk for queries.

---

#### `Tokudb_buffers_fetched_target_query`

- <strong>Description:</strong> Number of buffers fetched for queries off the disk.

---

#### `Tokudb_buffers_fetched_target_query_bytes`

- <strong>Description:</strong> Total buffer bytes fetched for queries off the disk.

---

#### `Tokudb_buffers_fetched_target_query_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while fetching buffers from disk for queries.

---

#### `Tokudb_cachetable_cleaner_executions`

- <strong>Description:</strong> Number of times the cleaner thread loop has executed.

---

#### `Tokudb_cachetable_cleaner_iterations`

- <strong>Description:</strong> Number of cleaner operations performed each cleaner period.

---

#### `Tokudb_cachetable_cleaner_period`

- <strong>Description:</strong> Time in seconds between the end of a group of cleaner operations and the beginning of the next. The TokuDB cleaner thread runs in the background, optimizing indexes and performing work not needing to be done by the client thread.

---

#### `Tokudb_cachetable_evictions`

- <strong>Description:</strong> Number of blocks evicted from the cache.

---

#### `Tokudb_cachetable_long_wait_pressure_count`

- <strong>Description:</strong> Number of times a thread was stalled for more than one second due to cache pressure.

---

#### `Tokudb_cachetable_long_wait_pressure_time`

- <strong>Description:</strong> Time in microseconds spent waiting for more than one second for cache pressure to ease.

---

#### `Tokudb_cachetable_miss`

- <strong>Description:</strong> Number of times the system failed to access data in the internal cache.

---

#### `Tokudb_cachetable_miss_time`

- <strong>Description:</strong> Total time in microseconds spent waiting for disk reads (when the cache could not supply the data) to finish.

---

#### `Tokudb_cachetable_prefetches`

- <strong>Description:</strong> Total number of times that a block of memory was prefetched into the database cache. This happens when it's determined that a block of memory is likely to be accessed by the application.

---

#### `Tokudb_cachetable_size_cachepressure`

- <strong>Description:</strong> Size in bytes of data causing cache pressure, or the sum of the buffers and workdone counters.

---

#### `Tokudb_cachetable_size_cloned`

- <strong>Description:</strong> Memory in bytes currently used for cloned nodes. Dirty nodes are cloned before serialization/compression during checkpoint operations, after which they are written to disk and the memory freed for re-use.

---

#### `Tokudb_cachetable_size_current`

- <strong>Description:</strong> Size in bytes of the uncompressed data in the cache.

---

#### `Tokudb_cachetable_size_leaf`

- <strong>Description:</strong> Size in bytes of the leaf nodes in the cache.

---

#### `Tokudb_cachetable_size_limit`

- <strong>Description:</strong> Size in bytes of the uncompressed data that could fit in the cache.

---

#### `Tokudb_cachetable_size_nonleaf`

- <strong>Description:</strong> Size in bytes of the nonleaf nodes in the cache.

---

#### `Tokudb_cachetable_size_rollback`

- <strong>Description:</strong> Size in bytes of the rollback nodes in the cache.

---

#### `Tokudb_cachetable_size_writing`

- <strong>Description:</strong> Size in bytes currently queued to be written to disk.

---

#### `Tokudb_cachetable_wait_pressure_count`

- <strong>Description:</strong> Number of times a thread was stalled due to cache pressure.

---

#### `Tokudb_cachetable_wait_pressure_time`

- <strong>Description:</strong> Time in microseconds spent waiting for cache pressure to ease.

---

#### `Tokudb_checkpoint_begin_time`

- <strong>Description:</strong> Cumulative time in microseconds needed to mark all dirty nodes as pending a checkpoint.

---

#### `Tokudb_checkpoint_duration`

- <strong>Description:</strong> Time in seconds needed to complete all checkpoints.

---

#### `Tokudb_checkpoint_duration_last`

- <strong>Description:</strong> Time in seconds needed to complete the last checkpoint.

---

#### `Tokudb_checkpoint_failed`

- <strong>Description:</strong> Total number of checkpoints failed.

---

#### `Tokudb_checkpoint_last_began`

- <strong>Description:</strong> Date and time the most recent checkpoint began, for example `Wed May 14 11:26:42 2014` Will be `Dec 31, 1969` on Linux if no checkpoint has ever begun.

---

#### `Tokudb_checkpoint_last_complete_began`

- <strong>Description:</strong> Date and time the last complete checkpoint started.

---

#### `Tokudb_checkpoint_last_complete_ended`

- <strong>Description:</strong>  Date and time the last complete checkpoint ended.

---

#### `Tokudb_checkpoint_long_begin_count`

- <strong>Description:</strong> Number of long checkpoint begins (checkpoint begins taking more than 1 second).

---

#### `Tokudb_checkpoint_long_begin_time`

- <strong>Description:</strong> Total time in microseconds of long checkpoint begins (checkpoint begins taking more than 1 second).

---

#### `Tokudb_checkpoint_period`

- <strong>Description:</strong> Time in seconds between the end of one automatic checkpoint and the beginning of the next.

---

#### `Tokudb_checkpoint_taken`

- <strong>Description:</strong> Total number of checkpoints taken.

---

#### `Tokudb_cursor_skip_deleted_leaf_entry`

- <strong>Description:</strong> Deleted leaf entries skipped during a range scan.
- <strong>Introduced:</strong> TokuDB 7.5.4

---

#### `Tokudb_db_closes`

- <strong>Description:</strong> Number of db_close operations.

---

#### `Tokudb_db_open_current`

- <strong>Description:</strong> Number of databases currently open.

---

#### `Tokudb_db_open_max`

- <strong>Description:</strong> Maximum of number of databases open at the same time.

---

#### `Tokudb_db_opens`

- <strong>Description:</strong> Number of db_open operations.

---

#### `Tokudb_descriptor_set`

- <strong>Description:</strong> Number of times a descriptor was updated when the entire dictionary was updated.

---

#### `Tokudb_dictionary_broadcast_updates`

- <strong>Description:</strong> Number of successful broadcast updates (an update that affects all rows in a dictionary).

---

#### `Tokudb_dictionary_updates`

- <strong>Description:</strong> Total number of rows updated in all primary and secondary indexes that have been done with a separate recovery log entry per index.

---

#### `Tokudb_filesystem_fsync_num`

- <strong>Description:</strong> Total number of times the database has flushed the operating system’s file buffers to
disk.

---

#### `Tokudb_filesystem_fsync_time`

- <strong>Description:</strong> Total time in microseconds used to fsync to disk.

---

#### `Tokudb_filesystem_long_fsync_num`

- <strong>Description:</strong> Total number of times the database has flushed the operating system’s file buffers to
disk when the operation took more than one second.

---

#### `Tokudb_filesystem_long_fsync_time`

- <strong>Description:</strong> Total time in microseconds used to fsync to disk when the operation took more than one second.

---

#### `Tokudb_filesystem_threads_blocked_by_full_disk`

- <strong>Description:</strong> Number of threads currently blocked due to attempting to write to a full disk. A warning appears in the `disk free space` field if not zero.

---

#### `Tokudb_leaf_compression_to_memory_seconds`

- <strong>Description:</strong> Time in seconds spent on compressing leaf nodes.

---

#### `Tokudb_leaf_decompression_to_memory_seconds`

- <strong>Description:</strong> Time in seconds spent on decompressing leaf nodes.

---

#### `Tokudb_leaf_deserialization_to_memory_seconds`

- <strong>Description:</strong> Time in seconds spent on deserializing leaf nodes.

---

#### `Tokudb_leaf_node_compression_ratio`

- <strong>Description:</strong> Ratio of uncompressed bytes in-memory to compressed bytes on-disk for leaf nodes.

---

#### `Tokudb_leaf_node_full_evictions`

- <strong>Description:</strong> Number of times a full leaf node was evicted from the cache.

---

#### `Tokudb_leaf_node_full_evictions_bytes`

- <strong>Description:</strong> Total bytes freed when a full leaf node was evicted from the cache.

---

#### `Tokudb_leaf_node_partial_evictions`

- <strong>Description:</strong> Number of times part of a leaf node (a partition) was evicted from the cache.

---

#### `Tokudb_leaf_node_partial_evictions_bytes`

- <strong>Description:</strong> Total bytes freed when part of a leaf node (a partition) was evicted from the cache.

---

#### `Tokudb_leaf_nodes_created`

- <strong>Description:</strong> Total number of leaf nodes created.

---

#### `Tokudb_leaf_nodes_destroyed`

- <strong>Description:</strong> Total number of leaf nodes destroyed.

---

#### `Tokudb_leaf_nodes_flushed_checkpoint`

- <strong>Description:</strong> Number of leaf nodes flushed to disk as part of a checkpoint.

---

#### `Tokudb_leaf_nodes_flushed_checkpoint_bytes`

- <strong>Description:</strong> Size in bytes of leaf nodes flushed to disk as part of a checkpoint.

---

#### `Tokudb_leaf_nodes_flushed_checkpoint_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while writing leaf nodes flushed to disk as part of a checkpoint.

---

#### `Tokudb_leaf_nodes_flushed_checkpoint_uncompressed_bytes`

- <strong>Description:</strong> Size in uncompressed bytes of leaf nodes flushed to disk as part of a checkpoint.

---

#### `Tokudb_leaf_nodes_flushed_not_checkpoint`

- <strong>Description:</strong> Number of leaf nodes flushed to disk not as part of a checkpoint.

---

#### `Tokudb_leaf_nodes_flushed_not_checkpoint_bytes`

- <strong>Description:</strong> Size in bytes of leaf nodes flushed to disk not as part of a checkpoint.

---

#### `Tokudb_leaf_nodes_flushed_not_checkpoint_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while writing leaf nodes flushed to disk not as part of a checkpoint.

---

#### `Tokudb_leaf_nodes_flushed_not_checkpoint_uncompressed_bytes`

- <strong>Description:</strong> Size in uncompressed bytes of leaf nodes flushed to disk not as part of a checkpoint.

---

#### `Tokudb_leaf_serialization_to_memory_seconds`

- <strong>Description:</strong> Time in seconds spent on serializing leaf nodes.

---

#### `Tokudb_loader_num_created`

- <strong>Description:</strong> Number of times a loader has been created.

---

#### `Tokudb_loader_num_current`

- <strong>Description:</strong> Number of currently existing loaders.

---

#### `Tokudb_loader_num_max`

- <strong>Description:</strong> Maximum number of loaders that existed at one time.

---

#### `Tokudb_locktree_escalation_num`

- <strong>Description:</strong> Number of times the locktree needed reduce its memory footprint by running lock escalation.

---

#### `Tokudb_locktree_escalation_seconds`

- <strong>Description:</strong> Time in seconds spent performing locktree escalation.

---

#### `Tokudb_locktree_latest_post_escalation_memory_size`

- <strong>Description:</strong> Memory size in bytes of the locktree after most recent locktree escalation.

---

#### `Tokudb_locktree_long_wait_count`

- <strong>Description:</strong> Number of times of more than one second duration that a lock could not be acquired as a result of a conflict with another transaction.

---

#### `Tokudb_locktree_long_wait_escalation_count`

- <strong>Description:</strong> Number of times a client thread waited for more than one second for lock escalation to free up memory.

---

#### `Tokudb_locktree_long_wait_escalation_time`

- <strong>Description:</strong> Time in microseconds of long waits (more than one second) for lock escalation to free up memory.

---

#### `Tokudb_locktree_long_wait_time`

- <strong>Description:</strong>Total time in microseconds spent by clients waiting for more than one second for a lock conflict to be resolved.

---

#### `Tokudb_locktree_memory_size`

- <strong>Description:</strong> Memory in bytes currently being used by the locktree.

---

#### `Tokudb_locktree_memory_size_limit`

- <strong>Description:</strong> Maximum memory in bytes the locktree can use.

---

#### `Tokudb_locktree_open_current`

- <strong>Description:</strong> Number of currently open locktrees.

---

#### `Tokudb_locktree_pending_lock_requests`

- <strong>Description:</strong> Number of requests waiting for a lock to be granted.

---

#### `Tokudb_locktree_sto_eligible_num`

- <strong>Description:</strong> Number of locktrees eligible for single transaction optimizations.

---

#### `Tokudb_locktree_sto_ended_num`

- <strong>Description:</strong> Total number of times a single transaction optimization completed early as a result of another transaction beginning.

---

#### `Tokudb_locktree_sto_ended_seconds`

- <strong>Description:</strong> Time in seconds spent ending single transaction optimizations.

---

#### `Tokudb_locktree_timeout_count`

- <strong>Description:</strong> Number of times a lock request timed out.

---

#### `Tokudb_locktree_wait_count`

- <strong>Description:</strong> Number of times a lock could not be acquired as a result of a conflict with another transaction.

---

#### `Tokudb_locktree_wait_escalation_count`

- <strong>Description:</strong> Number of times a client thread has waited on lock escalation. Lock escalation is run on a background thread when the sum of the acquired lock sizes reaches the lock tree limit.

---

#### `Tokudb_locktree_wait_escalation_time`

- <strong>Description:</strong> Time in microseconds that a client thread spent waiting for lock escalation.

---

#### `Tokudb_locktree_wait_time`

- <strong>Description:</strong> Total time in microseconds spent by clients waiting for a lock conflict to be resolved.

---

#### `Tokudb_logger_wait_long`

- <strong>Description:</strong>

---

#### `Tokudb_logger_writes`

- <strong>Description:</strong> Number of times the logger has written to disk.

---

#### `Tokudb_logger_writes_bytes`

- <strong>Description:</strong> Total bytes the logger has written to disk.

---

#### `Tokudb_logger_writes_seconds`

- <strong>Description:</strong>  Time in seconds spent waiting for IO while writing logs to disk.

---

#### `Tokudb_logger_writes_uncompressed_bytes`

- <strong>Description:</strong> Total uncompressed bytes the logger has written to disk.

---

#### `Tokudb_mem_estimated_maximum_memory_footprint`

- <strong>Description:</strong>

---

#### `Tokudb_messages_flushed_from_h1_to_leaves_bytes`

- <strong>Description:</strong> Total bytes of the messages flushed from h1 nodes to leaves.

---

#### `Tokudb_messages_ignored_by_leaf_due_to_msn`

- <strong>Description:</strong> Number of messages ignored by a leaf because it had already been applied.

---

#### `Tokudb_messages_in_trees_estimate_bytes`

- <strong>Description:</strong> Estimate of the total bytes of messages currently in trees.

---

#### `Tokudb_messages_injected_at_root`

- <strong>Description:</strong> Number of messages injected at root.

---

#### `Tokudb_messages_injected_at_root_bytes`

- <strong>Description:</strong> Total bytes of messages injected at root for all trees.

---

#### `Tokudb_nonleaf_compression_to_memory_seconds`

- <strong>Description:</strong> Time in seconds spent on compressing non-leaf nodes.

---

#### `Tokudb_nonleaf_decompression_to_memory_seconds`

- <strong>Description:</strong> Time in seconds spent on decompressing non-leaf nodes.

---

#### `Tokudb_nonleaf_deserialization_to_memory_seconds`

- <strong>Description:</strong> Time in seconds spent on deserializing non-leaf nodes.

---

#### `Tokudb_nonleaf_node_compression_ratio`

- <strong>Description:</strong> Ratio of uncompressed bytes in-memory to compressed bytes on-disk for nonleaf nodes.

---

#### `Tokudb_nonleaf_node_full_evictions`

- <strong>Description:</strong> Number of times a full non-leaf node was evicted from the cache.

---

#### `Tokudb_nonleaf_node_full_evictions_bytes`

- <strong>Description:</strong> Total bytes freed when a full non-leaf node was evicted from the cache.

---

#### `Tokudb_nonleaf_node_partial_evictions`

- <strong>Description:</strong> Number of times part of a non-leaf node (a partition) was evicted from the cache.

---

#### `Tokudb_nonleaf_node_partial_evictions_bytes`

- <strong>Description:</strong> Total bytes freed when part of a non-leaf node (a partition) was evicted from the cache.

---

#### `Tokudb_nonleaf_nodes_created`

- <strong>Description:</strong> Total number of non-leaf nodes created.

---

#### `Tokudb_nonleaf_nodes_destroyed`

- <strong>Description:</strong> Total number of non-leaf nodes destroyed.

---

#### `Tokudb_nonleaf_nodes_flushed_to_disk_checkpoint`

- <strong>Description:</strong> Number of non-leaf nodes flushed to disk  as part of a checkpoint.

---

#### `Tokudb_nonleaf_nodes_flushed_to_disk_checkpoint_bytes`

- <strong>Description:</strong> Size in bytes of non-leaf nodes flushed to disk  as part of a checkpoint.

---

#### `Tokudb_nonleaf_nodes_flushed_to_disk_checkpoint_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while writing non-leaf nodes flushed to disk as part of a checkpoint.

---

#### `Tokudb_nonleaf_nodes_flushed_to_disk_checkpoint_uncompressed_bytes`

- <strong>Description:</strong> Size in uncompressed bytes of non-leaf nodes flushed to disk as part of a checkpoint.

---

#### `Tokudb_nonleaf_nodes_flushed_to_disk_not_checkpoint`

- <strong>Description:</strong> Number of non-leaf nodes flushed to disk not as part of a checkpoint.

---

#### `Tokudb_nonleaf_nodes_flushed_to_disk_not_checkpoint_bytes`

- <strong>Description:</strong> Size in bytes of non-leaf nodes flushed to disk not as part of a checkpoint.

---

#### `Tokudb_nonleaf_nodes_flushed_to_disk_not_checkpoint_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while writing non-leaf nodes flushed to disk not as part of a checkpoint.

---

#### `Tokudb_nonleaf_nodes_flushed_to_disk_not_checkpoint_uncompressed_bytes`

- <strong>Description:</strong> Size in uncompressed bytes of non-leaf nodes flushed to disk not as part of a checkpoint.

---

#### `Tokudb_nonleaf_serialization_to_memory_seconds`

- <strong>Description:</strong> Time in seconds spent on serializing nonleaf nodes.

---

#### `Tokudb_overall_node_compression_ratio`

- <strong>Description:</strong> Ratio of uncompressed bytes in-memory to compressed bytes on-disk for all nodes.

---

#### `Tokudb_pivots_fetched_for_query`

- <strong>Description:</strong> Total number of pivot nodes fetched for queries.

---

#### `Tokudb_pivots_fetched_for_query_bytes`

- <strong>Description:</strong> Number of bytes of pivot nodes fetched for queries.

---

#### `Tokudb_pivots_fetched_for_query_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while fetching pivot nodes for queries.

---

#### `Tokudb_pivots_fetched_for_prefetch`

- <strong>Description:</strong> Total number of pivot nodes fetched by a prefetch thread.

---

#### `Tokudb_pivots_fetched_for_prefetch_bytes`

- <strong>Description:</strong> Number of bytes of pivot nodes fetched by a prefetch thread.

---

#### `Tokudb_pivots_fetched_for_prefetch_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while fetching pivot nodes by a prefetch thread.

---

#### `Tokudb_pivots_fetched_for_write`

- <strong>Description:</strong> Total number of pivot nodes fetched for writes.

---

#### `Tokudb_pivots_fetched_for_write_bytes`

- <strong>Description:</strong> Number of bytes of pivot nodes fetched for writes.

---

#### `Tokudb_pivots_fetched_for_write_seconds`

- <strong>Description:</strong> Time in seconds spent waiting for IO while fetching pivot nodes for writes.

---

#### `Tokudb_promotion_h1_roots_injected_into`

- <strong>Description:</strong> Number of times a message stopped at a root with a height of 1.

---

#### `Tokudb_promotion_injections_at_depth_0`

- <strong>Description:</strong> Number of times a message stopped at a depth of zero.

---

#### `Tokudb_promotion_injections_at_depth_1`

- <strong>Description:</strong> Number of times a message stopped at a depth of one.

---

#### `Tokudb_promotion_injections_at_depth_2`

- <strong>Description:</strong> Number of times a message stopped at a depth of two.

---

#### `Tokudb_promotion_injections_at_depth_3`

- <strong>Description:</strong> Number of times a message stopped at a depth of three.

---

#### `Tokudb_promotion_injections_lower_than_depth_3`

- <strong>Description:</strong> Number of times a message stopped at a depth of greater than three.

---

#### `Tokudb_promotion_leaf_roots_injected_into`

- <strong>Description:</strong> Number of times a message stopped at a root with a height of 0.

---

#### `Tokudb_promotion_roots_split`

- <strong>Description:</strong> Number of times the root split during promotion.

---

#### `Tokudb_promotion_stopped_after_locking_child`

- <strong>Description:</strong> Number of times a message stopped before a locked child.

---

#### `Tokudb_promotion_stopped_at_height_1`

- <strong>Description:</strong> Number of times a message stopped due to reaching a height of one.

---

#### `Tokudb_promotion_stopped_child_locked_or_not_in_memory`

- <strong>Description:</strong> Number of times a message stopped due to being unable to cheaply access (locked or not stored in memory) a child.

---

#### `Tokudb_promotion_stopped_child_not_fully_in_memory`

- <strong>Description:</strong> Number of times a message stopped due to being unable to cheaply access (not fully stored in memory) a child.

---

#### `Tokudb_promotion_stopped_nonempty_buffer`

- <strong>Description:</strong> Number of times a message stopped due to reaching a buffer that wasn't empty.

---

#### `Tokudb_txn_aborts`

- <strong>Description:</strong> Number of transactions that have been aborted.

---

#### `Tokudb_txn_begin`

- <strong>Description:</strong> Number of transactions that have been started.

---

#### `Tokudb_txn_begin_read_only`

- <strong>Description:</strong> Number of read-only transactions that have been started.

---

#### `Tokudb_txn_commits`

- <strong>Description:</strong> Number of transactions that have been committed.

---