# TokuDB System Variables

TokuDB has been deprecated by its upstream maintainer. It is disabled from [MariaDB 10.5](/kb/en/what-is-mariadb-105/) and has been been removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/) - [MDEV-19780](https://jira.mariadb.org/browse/MDEV-19780). We recommend [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/) as a long-term migration path.

This page lists system variables that are related to [TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb/).

See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting them, and [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/) for a complete list of all options, statis variable and system variables in MariaDB.

## System Variables

#### `tokudb_alter_print_error`

- <strong>Description:</strong> Print errors for alter table operations.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `OFF`

---

#### `tokudb_analyze_time`

- <strong>Description:</strong> Time in seconds that [ANALYZE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) operations spend on each index when calculating cardinality.  Accurate cardinality helps in particular with the performance of complex queries. If no analyzes are run, cardinality will be 1 for primary indexes, and unknown (NULL) for other types of indexes.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `5`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `tokudb_block_size`

- <strong>Description:</strong> Uncompressed size of internal fractal tree and leaf nodes. Changing will only affect tables created after the new setting is in effect. Existing tables will keep the setting they were created with unless the table is dumped and reloaded.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `4194304` (4MB)
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `tokudb_bulk_fetch`

- <strong>Description:</strong> If set to `1` (the default), the bulk fetch algorithm is used for SELECT's and DELETE's, including related statements such as INSERT INTO.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `ON`

---

#### `tokudb_cache_size`

- <strong>Description:</strong> Size in bytes of the TokuDB cache. This variable is read-only and cannot be changed dynamically. To change the value, either set the value in the `my.cnf` file prior to loading TokuDB or
restart MariaDB after modifying the configuration. If you have loaded the
plugin but not used TokuDB yet, you can unload the plugin then reload it and
MariaDB will reload the plugin with the setting from the configuration file. Setting to at least half of the available memory is recommended, although if using directIO instead of buffered IO (see [tokudb_directio](#tokudb_directio)) , up to 80% of the available memory is recommended. Decrease if other applications require significant memory or swapping is degrading performance.
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> Half of the total system memory

---

#### `tokudb_check_jemalloc`

- <strong>Description:</strong> Check if jemalloc is linked.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `1`
- <strong>Valid Values:</strong> `0` and `1`

---

#### `tokudb_checkpoint_lock`

- <strong>Description:</strong> Mechanism to lock out TokuDB checkpoints. When set to `1`, TokuDB checkpoints are locked out. Setting to `0`, or disconnecting the client, releases the lock.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `OFF`

---

#### `tokudb_checkpoint_on_flush_logs`

- <strong>Description:</strong> TokuDB checkpoint on flush logs.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `OFF`

---

#### `tokudb_checkpointing_period`

- <strong>Description:</strong> Time in seconds between the beginning of each checkpoint. It is recommended to leave this at the default setting of 1 minute.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `60`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `tokudb_cleaner_iterations`

- <strong>Description:</strong> Number of internal nodes processed in each cleaner thread period (see [tokudb_cleaner_period](#tokudb_cleaner_period)). Setting to `0` turns off cleaner threads.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `5`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `tokudb_cleaner_period`

- <strong>Description:</strong> Frequency in seconds for the running of the cleaner thread. Setting to `0` turns off cleaner threads.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `tokudb_commit_sync`

- <strong>Description:</strong> Whether or not the transaction log is flushed upon transaction commit. Flushing has a minor performance penalty, but switching it off means that committed transactions may not survive a server crash.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `ON`

---

#### `tokudb_create_index_online`

- <strong>Description:</strong> Whether indexes are hot or not. Hot, or online, indexes (the default) mean that the table is available for inserting and updates while the index is being created. It is slower to create hot indexes.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `ON`

---

#### `tokudb_data_dir`

- <strong>Description:</strong> Directory where the TokuDB data is stored. By default the variable is empty, in which case the regular [datadir](/kb/en/server-system-variables/#datadir) is used.
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> string
- <strong>Default Value:</strong> Empty (the MariaDB datadir is used)

---

#### `tokudb_debug`

- <strong>Description:</strong> Setting to a non-zero value turns on various TokuDB debug traces.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `tokudb_directio`

- <strong>Description:</strong> When set to ON, TokuDB writes use Direct IO instead of Buffered IO. [tokudb_cache_size](#tokudb_cache_size) should be adjusted when using DirectIO.
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `OFF`

---

#### `tokudb_disable_hot_alter`

- <strong>Description:</strong> If set to `ON` (`OFF` is default), hot alter table is disabled.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `OFF`

---

#### `tokudb_disable_prefetching`

- <strong>Description:</strong> If prefetching is not disabled (the default), range queries usually benefit from aggressive prefetching of blocks of rows. For range queries with LIMIT clauses, this can create unnecessary IO, and so prefetching can be disabled if these make up a majority of range queries.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `OFF`

---

#### `tokudb_disable_slow_alter`

- <strong>Description:</strong> Usually, TokuDB permits column addition, deletion, expansion, and renaming with minimal locking, very quickly. This variable determines whether certain slow [alter|ALTER]] table statements that cannot take advantage of this feature are permitted. Statements that are slow are those that include a mix of column additions, deletions or expansions, for example, `ALTER TABLE t1 ADD COLUMN c1 int, DROP COLUMN c2`.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `OFF`

---

#### `tokudb_empty_scan`

- <strong>Description:</strong> TokuDB algorithm to check if the table is empty when opened. Setting to `disabled` will reduce this overhead.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> enum
- <strong>Default Value:</strong> `rl`
- <strong>Valid Values:</strong> `lr`, `rl`, `disabled`

---

#### `tokudb_fs_reserve_percent`

- <strong>Description:</strong> If this percentage of the filesystem is not free, inserts will be prohibited. Recommended value is half the size of the available memory. Once disabled, inserts will be re-enabled once twice the reserve is available. TokuDB will freeze entirely if the disk becomes entirely full.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `5`

---

#### `tokudb_fsync_log_period`

- <strong>Description:</strong> fsync() operations frequency in milliseconds. If set to `0`, the default, [tokudb_commit_sync](#tokudb_commit_sync) control fsync() behavior.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`

<strong>Warning</strong>: currently values in the 1000-2000 range seem to cause server crashes, see [MDEV-16732](https://jira.mariadb.org/browse/MDEV-16732)

---

#### `tokudb_hide_default_row_format`

- <strong>Description:</strong> Hide the default row format.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `ON`

---

#### `tokudb_killed_time`

- <strong>Description:</strong> Control lock tree kill callback frequency.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `4000`
- <strong>Range:</strong> `0` to `18446744073709551615`
- <strong>Introduced:</strong> TokuDB 7.1.5

---

#### `tokudb_last_lock_timeout`

- <strong>Description:</strong> Empty by default, when a lock deadlock is detected, or a lock request times out, set to a JSON document describing the most recent lock conflict. Only set when the first bit of  [tokudb_lock_timeout_debug](#tokudb_lock_timeout_debug) is set.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> text
- <strong>Default Value:</strong> Empty

---

#### `tokudb_load_save_space`

- <strong>Description:</strong> If set to `1`, the default, bulk loader intermediate data is compressed, otherwise it is uncompressed. Also see [tokudb_tmp_dir](#tokudb_tmp_dir).
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `ON`

---

#### `tokudb_loader_memory_size`

- <strong>Description:</strong> Memory limit for each loader instance used by the TokuDB bulk loader.  Memory is taken from the TokuDB cache ([tokudb_cache_size](#tokudb_cache_size)), so current cache data may need to be cleared for the loader to begin. Increase if tables are very larger, with multiple secondary indexes.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `100000000` (100M)
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `tokudb_lock_timeout`

- <strong>Description:</strong> Time in milliseconds that a transaction will wait for a lock held by another transaction to be released before timing out with a `lock wait timeout` error (-30994). Setting to `0` disables lock waits.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `4000` (4 seconds)
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `tokudb_lock_timeout_debug`

- <strong>Description:</strong> When bit zero is set (default `1`), a JSON document describing the most recent lock conflict is reported to [tokudb_last_lock_timeout](#tokudb_last_lock_timeout). When set to `0`, no lock conflicts are reported. When bit one is set, the JSON document is printed to the [error log](/mariadb-administration/server-monitoring-logs/error-log/).
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `1`

---

#### `tokudb_log_dir`

- <strong>Description:</strong> Directory where the TokuDB log files are stored. By default the variable is empty, in which case the regular [datadir](/kb/en/server-system-variables/#datadir) is used.
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> string
- <strong>Default Value:</strong> Empty (the MariaDB datadir is used)

---

#### `tokudb_max_lock_memory`

- <strong>Description:</strong> Max memory for locks.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `130653952`

---

#### `tokudb_optimize_index_fraction`

- <strong>Description:</strong> When deleting a percentage of the tree (useful when the left side of the tree has many deletions, such as a pattern with increasing ids or dates), it's possible to optimize a subset of the fractal tree, as determined by the value of this variable, which ranges from `0.0` to `1.0` (indicating the whole tree).
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `1.000000`
- <strong>Range:</strong> `0.0` to `1.0`
- <strong>Introduced:</strong> TokuDB 7.5.5

---

#### `tokudb_optimize_index_name`

- <strong>Description:</strong> If set to an index name, will optimize that single index in a table. Empty by default.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> string
- <strong>Default Value:</strong> None
- <strong>Introduced:</strong> TokuDB 7.5.5

---

#### `tokudb_optimize_throttle`

- <strong>Description:</strong> Table optimization utilizes all available resources by default. This variable allows the table optimization speed to be limited in order to reduce the overall resources used. The limit places an upper bound on the number of fractal tree leaf nodes that are optimized per second. `0`, the default, imposes no limit.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `18446744073709551615`
- <strong>Introduced:</strong> TokuDB 7.5.5

---

#### `tokudb_pk_insert_mode`

- <strong>Description:</strong> Mode for primary key inserts using either [REPLACE INTO](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace/) or [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/ignore/) on tables with no secondary index, or where all columns in the secondary index are in the primary key. For example `PRIMARY KEY (a,b,c), key (b,c)`
<ul start="1"><li>`0`: Fast inserts. [Triggers](/programming-customizing-mariadb/triggers-events/triggers/) may not work, and [row-based replication](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats/) will not work
</li><li>`1`: Fast inserts if no triggers are defined, otherwise inserts may be slow. Row-based replication will not work.
</li><li>`2`: Slow inserts. Triggers and row-based replication work normally.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> enumerated
- <strong>Default Value:</strong> `1`
- <strong>Valid Values:</strong> `0`, `1`, `2`

---

#### `tokudb_prelock_empty`

- <strong>Description:</strong> If set to `0` (`1` is default), fast bulk loading will  be switched off. Usually, TokuDB obtains a table lock on empty tables. If, as is usual, only one transaction is loading the table, this speeds up the inserts. However, if many transactions are loading, only one can have access at a time, so setting this to `0`, avoiding the lock, will speed inserts up in that situation.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `ON`

---

#### `tokudb_read_block_size`

- <strong>Description:</strong> Uncompressed size in bytes of the read blocks of the fractal tree leaves. Changing will only affect tables created after the new setting is in effect. Existing tables will keep the setting they were created with unless the table is dumped and reloaded. Larger values are better for large range scans and higher compressions, while smaller values are better for point and small range scans.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `65536` (64KB)
- <strong>Range:</strong> `4096` to `4294967295`

---

#### `tokudb_read_buf_size`

- <strong>Description:</strong> Per-client size in bytes of the buffer used for storing bulk fetched values as part of a large range query. Reduce if there are many simultaneous clients. Setting to `0` disables bulk fetching.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `131072` (128KB)
- <strong>Range:</strong> `0` to `1048576`

---

#### `tokudb_read_status_frequency`

- <strong>Description:</strong> Progress is measured every this many reads for display by [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/). Useful to set to `1` to examine slow queries.
- <strong>Scope:</strong> Global,
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `10000`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `tokudb_row_format`

- <strong>Description:</strong> Compression algorithm used by default to compress data. Can be overridden by a row format specified in the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement. note that the library can be specified directly, or an alias used, the mapping of which may change in future. Note that in [MariaDB 5.5](/kb/en/what-is-mariadb-55/), and before [MariaDB 10.0.10](/kb/en/mariadb-10010-release-notes/), the compression type did not default to this value. See [TokuDB Differences](/columns-storage-engines-and-plugins/storage-engines/tokudb/tokudb-differences/).
<ul><li>`tokudb_default`, `tokudb_zlib`: Use the zlib library, 
</li><li>`tokudb_fast`, `tokudb_quicklz`: Use the quicklz library, the lightest compression with low CPU usage,
</li><li>`tokudb_small`, `tokudb_lzma`: Use the lzma library. the highest compression and highest CPU usage
</li><li>`tokudb_uncompressed`: No compression is used.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> enumerated
- <strong>Default Value:</strong> `tokudb_zlib`
- <strong>Valid Values:</strong> `tokudb_default`, `tokudb_fast`, `tokudb_small`, `tokudb_zlib`, `tokudb_quicklz`, `tokudb_lzma`, `tokudb_uncompressed`

---

#### `tokudb_rpl_check_readonly`

- <strong>Description:</strong> By default, when the slave is in read only mode, row events will be run from the binary log using TokuDB's read-free replication (RFR). Setting this variable to `OFF` turns off the slave read only check, allowing RFR to run when the slave is not read-only. Be careful that you understand the consequences if setting this variable.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `ON`

---

#### `tokudb_rpl_lookup_rows`

- <strong>Description:</strong> If set to `OFF` (`ON` is default), and [binlog_format](/kb/en/replication-and-binary-log-server-system-variables/#binlog_format) to `ROW` and [read_only](/kb/en/server-system-variables/#read_only) to `ON`, TokuDB replication slaves will not perform row lookups for update or delete row log events, removing the need for the associated IO.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `ON`

---

#### `tokudb_rpl_lookup_rows_delay`

- <strong>Description:</strong> Can be used to simulate long disk reads by sleeping for the specified time, in microseconds, before the row lookup query. Only useful to change in a test environment.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `0`

---

#### `tokudb_rpl_unique_checks`

- <strong>Description:</strong> If set to `OFF` (`ON` is default), and [binlog_format](/kb/en/replication-and-binary-log-server-system-variables/#binlog_format) to `ROW` and [read_only](/kb/en/server-system-variables/#read_only) to `ON`, TokuDB replication slaves will skip uniqueness checks on inserts and updates, removing the associated IO.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `ON`

---

#### `tokudb_rpl_unique_checks_delay`

- <strong>Description:</strong> Can be used to simulate long disk reads by sleeping for the specified time, in microseconds, before the row lookup query. Only useful to change in a test environment.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `0`

---

#### `tokudb_support_xa`

- <strong>Description:</strong> Whether or not the prepare phase of an XA transaction performs an fsync().
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong> `ON`

---

#### `tokudb_tmp_dir`

- <strong>Description:</strong> Directory where the TokuDB bulk loaders temporary files are stored. Can be very large, and useful to place on a separate disk. By default the variable is empty, in which case the regular [datadir](/kb/en/server-system-variables/#datadir) is used. [tokudb_load_save_space](#tokudb_load_save_space) determines whether the data is compressed or not. The error message `ERROR 1030 (HY000): Got error 1 from storage engine` could indicate that the disk has run out of space.
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `directory name`
- <strong>Default Value:</strong> Empty (the MariaDB datadir is used)

---

#### `tokudb_version`

- <strong>Description:</strong> The TokuDB version of the plugin included on MariaDB.
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

#### `tokudb_write_status_frequency`

- <strong>Description:</strong> Progress is measured every this many writes for display by [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/). Useful to set to `1` to examine slow queries.
- <strong>Scope:</strong> Global,
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1000`
- <strong>Range:</strong> `0` to `4294967295`

---