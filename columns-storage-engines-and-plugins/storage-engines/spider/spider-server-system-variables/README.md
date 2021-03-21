# Spider Server System Variables

The following variables are available when the [Spider](/columns-storage-engines-and-plugins/storage-engines/spider/) storage engine has been installed.

See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting them.

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `spider_auto_increment_mode`

- <strong>Description:</strong> The [auto increment](/columns-storage-engines-and-plugins/data-types/auto_increment/) mode.
<ul start="1"><li>`-1` Uses the table parameter.
</li><li>`0` Normal Mode. Uses a counter that Spider gets from the remote backend server with an exclusive lock for the auto-increment value.  This mode is slow.  Use Quick Mode (`2`), if you use Spider tables with the table partitioning feature and the auto-increment column is the first column of the index. Before [MariaDB 10.3](/kb/en/what-is-mariadb-103/), this value works as "1" for partitioned Spider tables.
</li><li>`1` Quick Mode. Uses an internal Spider counter for the auto-increment value. This mode is fast, but it is possible for duplicates to occur when updating the same table from multiple Spider proxies.
</li><li>`2` Set Zero Mode. The auto-increment value is given by the remote backend.  Sets the column to `0`, even if you set the value to the auto-increment column in your statement.  If you use the table with the table partitioning feature, it sets to zero after choosing an inserted partition.
</li><li>`3` When the auto-increment column is set to `NULL`, the value is given by the remote backend server.  If you set the auto-increment column to `0`,the value is given by the local server.  Set [spider_reset_auto_incremnet](#spider_reset_auto_increment) to `2` or `3` if you want to use an auto-increment column on the remote server.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `3`
- <strong>DSN Parameter Name:</strong> `aim`

---

#### `spider_bgs_first_read`

- <strong>Description:</strong> Number of first read records to use when performing a concurrent background search. To start a range scan on the remote backend, the storage engine first needs to send the first record. Fetching a second record in the same query can save a network round trip stopping the plan if the backend has a single record. The first and second reads are used to warm up for background search.  When not using [spider_split_read](#spider_split_read) and [spider_semi_split_read](#spider_semi_split_read), the third read fetches the remaining data source in a single fetch.
<ul start="1"><li>`-1` Uses the table parameter.
</li><li>`0` Records are usually retrieved.
</li><li>`1` and greater: Number of records.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `2`
- <strong>Range:</strong> `-1` to `9223372036854775807`
- <strong>DSN Parameter Name:</strong> `bfr`

---

#### `spider_bgs_mode`

- <strong>Description:</strong> Background search mode. This enables the use of a thread per data server connection if the query is not shard-based and must be distributed across shards. The partitioning plugin scans partitions one after the other to optimize memory usage. Because the shards are external, reading all shards can be performed in parallel when the plan prunes multiple partitions. 
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Disables background search. 
</li><li>`1`  Uses background search when searching without locks
</li><li>`2` Uses background search when searching without locks or with shared locks.
</li><li>`3` Uses background search regardless of locks. 
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong>  `0`
- <strong>Range:</strong> `-1` to `3`
- <strong>DSN Parameter Name:</strong> `bmd`

---

#### `spider_bgs_second_read`

- <strong>Description:</strong> Number of second read records on the backend server when using background search. When the first records are found from [spider_bgs_first_read](#spider_bgs_first_read), the engine continues scanning a range adding a `LIMIT` of [spider_bgs_first_read](#spider_bgs_first_read) and [spider_bgs_second_read](#spider_bgs_second_read).
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Records are usually retrieved.
</li><li>`1` and greater: Number of records.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `100`
- <strong>Range:</strong> `-1` to `9223372036854775807`
- <strong>DSN Parameter Name:</strong> `bsr`

---

#### `spider_bka_engine`

- <strong>Description:</strong> Storage engine used with temporary tables when the [spider_bka_mode](#spider_bka_mode) system variable is set to `1`.  Defaults to the value of the table parameter, which is [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) by default.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Session Value:</strong>  `""`
- <strong>Default Table Value:</strong> `Memory`
- <strong>DSN Parameter Name:</strong> `bke`

---

#### `spider_bka_mode`

- <strong>Description:</strong> Internal action to perform when multi-split reads are disabled.  If the [spider_multi_split_read](#spider_multi_split_read) system variable is set to `0`, Spider uses this variable to determine how to handle statements when the optimizer resolves range retrieval to multiple conditions.
<ul start="1"><li>` -1` Uses the table parameter. 
</li><li>`0` Uses "union all".
</li><li>`1` Uses a temporary table, if it is judged acceptable.
</li><li>`2` Uses a temporary table, if it is judged acceptable and avoids replication delay.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `1`
- <strong>Range:</strong> `-1` to `2`
- <strong>DSN Parameter Name:</strong> `bkm`

---

#### `spider_bka_table_name_type`

- <strong>Description:</strong> The type of temporary table name for bka.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1`
- <strong>Introduced:</strong> [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/)

---

#### `spider_block_size`

- <strong>Description:</strong> Size of memory block used in MariaDB. Can usually be left unchanged.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `16384`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>DSN Parameter Name:</strong> `bsz`

---

#### `spider_buffer_size`

- <strong>Description:</strong> Buffer size. `-`, the default, will use the table parameter.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `spider_bulk_size`

- <strong>Description:</strong> Size in bytes of the buffer when multiple grouping multiple `INSERT` statements in a batch, (that is, bulk inserts).
<ul start="1"><li>`-1` The table parameter is adopted.
</li><li>`0` or greater: Size of the buffer.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `16000`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>DSN Parameter Name:</strong> `bsz`

---

#### `spider_bulk_update_mode`

- <strong>Description:</strong> Bulk update and delete mode.  Note: If you use a non-default value for the [spider_bgs_mode](#spider_bgs_mode) or [spider_split_read](#spider_split_read) system variables, Spider sets this variable to `2`.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Sends `UPDATE` and `DELETE` statements one by one.
</li><li>`1` Collects multiple `UPDATE` and `DELETE` statements, then sends the collected statements one by one.
</li><li>`2` Collects multiple `UPDATE` and `DELETE` statements and sends them together.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `2`
- <strong>DSN Parameter Name:</strong> `bum`

---

#### `spider_bulk_update_size`

- <strong>Description:</strong> Size in bytes for `UPDATE` and `DELETE` queries when generating bulk updates.
<ul start="1"><li>`-1` The table parameter is adopted.
</li><li>`0` or greater:  Size of buffer.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `16000`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>DSN Parameter Name:</strong> `bus`

---

#### `spider_casual_read`

- <strong>Description:</strong> Casual Reads enables all isolation levels, (such as repeatable reads) to work with transactions on multiple backends.  With auto-commit queries, you can relax read consistency and run on multiple connections to the backends.  This enables parallel queries across partitions, even if multiple shards are stored on the same physical server.
<ul start="1"><li>`-1` Use table parameter.
</li><li>`0` Use casual read. 
</li><li>`1` Choose connection channel automatically.
</li><li>`2` to `63` Number of connection channels.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `63`
- <strong>DSN Parameter Name:</strong> `##`

---

#### `spider_conn_recycle_mode`

- <strong>Description:</strong> Connection recycle mode.
<ul start="1"><li>`0` Disconnect.
</li><li>`1` Recycle by all sessions.
</li><li>`2` Recycle in the same session.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Range:</strong> `0` to `2`
- <strong>Default Session Value:</strong> `0`

---

#### `spider_conn_recycle_strict`

- <strong>Description:</strong> Whether to force the creation of new connections.
<ul start="1"><li>`1` Don't force.
</li><li>`0` Force new connection 
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `0`
- <strong>Range:</strong> `0` to `1`

---

#### `spider_conn_wait_timeout`

- <strong>Description:</strong> Max waiting time in seconds for Spider to get a remote connection.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `10`
- <strong>Range:</strong> `0` to `1000`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `spider_connect_error_interval`

- <strong>Description:</strong> Return same error code until interval passes if connection is failed
- <strong>Scope:</strong> Global,
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `spider_connect_mutex`

- <strong>Description:</strong> Whether to serialize remote servers connections (use mutex at connecting). Use this parameter if you get an error or slowdown due to too many connection processes.
<ul start="1"><li>`0`  Not serialized.
</li><li>`1`  : Serialized.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Session Value:</strong> `0`

---

#### `spider_connect_retry_count`

- <strong>Description:</strong> Number of times to retry connections that fail due to too many connection processes.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `1000`
- <strong>Range:</strong> `0` to `2147483647`

---

#### `spider_connect_retry_interval`

- <strong>Description:</strong> Interval in microseconds for connection failure due to too many connection processes.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `1000`
- <strong>Range:</strong> `-1` to `9223372036854775807`

---

#### `spider_connect_timeout`

- <strong>Description:</strong> Timeout in seconds to declare remote backend unresponsive when opening a connection. Change for high-latency networks.
<ul start="1"><li>`-1` The table parameter is adopted.
</li><li>`0` Less than 1.
</li><li>`1` and greater: Number of seconds.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>DSN Parameter Name:</strong> `cto`

---

#### `spider_crd_bg_mode`

- <strong>Description:</strong> Indexes cardinality statistics in the background. Disable when the [spider_crd_mode](#spider_crd_mode) system variable is set to `3` or when the [spider_crd_interval](#spider_crd_interval) variable is set to `0`.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Disables background confirmation.
</li><li>`2` Enables background confirmation.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `1`
- <strong>Range:</strong> `-1` to `2`
- <strong>DSN Parameter Name:</strong> `cbm`

---

#### `spider_crd_interval`

- <strong>Description:</strong> Time interval in seconds of index cardinality statistics. Set to `0` to always get the latest information from remote servers. 
<ul start="1"><li>`-1` The table parameter is adopted.
</li><li>`1` or more: Interval in seconds table state confirmation.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `51`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>DSN Parameter Name:</strong> `civ`

---

#### `spider_crd_mode`

- <strong>Description:</strong> Mode for index cardinality statistics. By default, uses `SHOW` at the table-level.
<ul start="1"><li>`-1,0` Uses the table parameter.
</li><li>`1` Uses the `SHOW` command.
</li><li>`2` Uses the Information Schema. 
</li><li>`3` Uses the `EXPLAIN` command.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `1`
- <strong>Range:</strong> `-1` to `3`
- <strong>DSN Parameter Name:</strong> `cmd`

---

#### `spider_crd_sync`

- <strong>Description:</strong>  Synchronize index cardinality statistics in partitioned tables.
<ul start="1"><li>`-1` Uses the table parameter.
</li><li>`0` Disables synchronization.
</li><li>`1` Uses table state synchronization when opening a table, but afterwards performs no synchronization. 
</li><li>`2` Enables synchronization.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `2`
- <strong>DSN Parameter Name:</strong> `csy`

---

#### `spider_crd_type`

- <strong>Description:</strong> Type of cardinality calculation. Only effective when the [spider_crd_mode](#spider_crd_mode) system variable is set to use `SHOW` (`1`) or to use the Information Schema (`2`).
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Uses the value of the [spider_crd_weight](#spider_crd_weight) system variable, as a fixed value.
</li><li>`1` Uses the value of the [spider_crd_weight](#spider_crd_weight) system variable, as an addition value.
</li><li>`2` Uses the value of the [spider_crd_weight](#spider_crd_weight) system variable, as a multiplication value.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `2`
- <strong>Range:</strong> `-1` to `2`
- <strong>DSN Parameter Name:</strong> `ctp`

---

#### `spider_crd_weight`

- <strong>Description:</strong> Weight coefficient used to calculate effectiveness of index from the cardinality of column. For more information, see the description for the [spider_crd_type](#spider_crd_type) system variable.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` or greater: Weight.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `2`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>DSN Parameter Name:</strong> `cwg`

---

#### `spider_delete_all_rows_type`

- <strong>Description:</strong> The type of delete_all_rows.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1`

---

#### `spider_direct_dup_insert`

- <strong>Description:</strong> Manages duplicate key check for [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace/), [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/ignore/) and [LOAD DATA LOCAL INFILE](/kb/en/load-data-infile/) to remote servers. This can save on network roundtrips if the key always maps to a single partition.  For bulk operations, records are checked for duplicate key errors one by one on the remote server, unless you set it to avoid duplicate checks on local servers (`1`).
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Performs duplicate checks on the local server.
</li><li>`1` Avoids duplicate checks on the local server.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `1`
- <strong>DSN Parameter Name:</strong> `ddi`

---

#### `spider_direct_order_limit`

- <strong>Description:</strong> Pushes `ORDER BY` and `LIMIT` operations to the remote server.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Uses local execution.
</li><li>`1` Uses push down execution.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `9223372036854775807`
- <strong>DSN Parameter Name:</strong> `dol`

---

#### `spider_dry_access`

- <strong>Description:</strong> Simulates an empty result-set. No queries are sent to the backend. Use for performance tuning.
<ul><li>`0` Normal access.
</li><li>`1` All access from Spider to data node is suppressed.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `spider_error_read_mode`

- <strong>Description:</strong> Sends an empty result-set when reading a backend server raises an error.  Useful with applications that don't implement transaction replays.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Returns an error.
</li><li>`1` Returns an empty result.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `1`
- <strong>DSN Parameter Name:</strong> `erm`

---

#### `spider_error_write_mode`

- <strong>Description:</strong> Sends an empty result-set when writing to a backend server raises an error.  Useful with applications that don't implement transaction replays.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Returns an error. 
</li><li>`1` Returns an empty result-set on error. 
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `1`
- <strong>DSN Parameter Name:</strong> `ewm`

---

#### `spider_first_read`

- <strong>Description:</strong> Number of first read records to start a range scan on the backend server.  Spider needs to send the first record.  Fetching the second record saves network round-trips, stopping the plan if the backend has a single record.  First read and second read are used to warm up for background searches, third reads without using the [spider_split_read](#spider_split_read) and [spider_semi_split_read](#spider_semi_split_read) system variables fetches the remaining data source in a single last fetch.
<ul start="1"><li>`-1` Use the table parameter. 
</li><li>`0` Usually retrieves records.
</li><li>`1` and greater: Sets the number of first read records.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `2`
- <strong>Range:</strong> `-1` to `9223372036854775807`
- <strong>DSN Parameter Name:</strong> `frd`

---

#### `spider_force_commit`

- <strong>Description:</strong> Behavior when error occurs on `XA PREPARE`, `XA COMMIT`, and `XA ROLLBACK` statements.
<ul start="1"><li>`0` Returns the error.
</li><li>`1` Returns the error when the `xid` doesn't exist, otherwise it continues processing the XA transaction.
</li><li>`2` Continues processing the XA transaction, disregarding all errors.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `0`
- <strong>Range:</strong> `0` to `2`

---

#### `spider_general_log`

- <strong>Description:</strong>  Whether Spider logs all commands to the General Log.  Spider logs error codes according to the [spider_log_result_errors](#spider_log_result_errors) system variable.
<ul start="1"><li>`OFF` Logs no commands.
</li><li>`ON` Logs commands to the General Log.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Session Value:</strong> `OFF`

---

#### `spider_index_hint_pushdown`

- <strong>Description:</strong> Whether to use pushdown index hints, like `force_index`.
<ul><li>`0` Do not use pushdown hints
</li><li>`1` Use pushdown hints
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Session Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `spider_init_sql_alloc_size`

- <strong>Description:</strong> Initial size of the local SQL buffer.
<ul start="1"><li>`-1` Uses the table parameter.
</li><li>`0` or greater: Size of the buffer.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `1024`
- <strong>DSN Parameter Name:</strong> `isa`
- <strong>Range:</strong> `-1` to `2147483647`

---

#### `spider_internal_limit`

- <strong>Description:</strong> Limits the number of records when acquired from a remote server.
<ul start="1"><li>`-1` The table parameter is adopted.
</li><li>`0` or greater: Records limit. 
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `9223372036854775807`
- <strong>Range:</strong> `-1` to `9223372036854775807`
- <strong>DSN Parameter Name:</strong> `ilm`

---

#### `spider_internal_offset`

- <strong>Description:</strong> Skip records when acquired from the remote server.
<ul start="1"><li>`-1` Uses the table parameter.
</li><li>`0` or more : Number of records to skip.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `9223372036854775807`
- <strong>DSN Parameter Name:</strong> `ios`

---

#### `spider_internal_optimize`

- <strong>Description:</strong> Whether to perform push down operations for [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/) statements.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Transmitted.
</li><li>`1` Not transmitted.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `1`
- <strong>DSN Parameter Name:</strong> `iom`

---

#### `spider_internal_optimize_local`

- <strong>Description:</strong> Whether to transmit to remote servers when [OPTIMIZE TABLE](optimize_table) statements are executed on the local server.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Not transmitted.
</li><li>`1` Transmitted.
</li></ul>
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1`
- <strong>DSN Parameter Name:</strong> `iol`

---

#### `spider_internal_sql_log_off`

- <strong>Description:</strong> Whether to log SQL statements sent to the remote server in the [General Query Log](/mariadb-administration/server-monitoring-logs/general-query-log/). 
<ul start="1"><li>Explicitly setting this system variable to either `ON` or `OFF` causes the Spider node to send a `SET sql_log_off` statement to each of the data nodes using the `SUPER` privilege.
</li><li>`-1` Don't know or does not matter; don't send 'SET SQL_LOG_OFF' statement
</li><li>`0` Send 'SET SQL_LOG_OFF 0' statement to data nodes (logs SQL statements to the remote server)
</li><li>`1` Send 'SET SQL_LOG_OFF 1' statement to data nodes (doesn't log SQL statements to the remote server)
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric` (previously `boolean`)
- <strong>Range:</strong> `-1` to `1`
- <strong>Default Session Value:</strong> `-1` (previously `ON`)

---

#### `spider_internal_unlock`

- <strong>Description:</strong> Whether to transmit unlock tables to the connection of the table used with `SELECT` statements.
<ul start="1"><li>`0` Not transmitted.
</li><li>`1` Transmitted.
</li></ul>
- <strong>Data Type:</strong> `boolean`
- <strong>Default Session Value:</strong> `0`

---

#### `spider_internal_xa`

- <strong>Description:</strong> Whether to implement XA at the server- or storage engine-level.  When using the server-level, set different values for the [server_id](/kb/en/server-system-variables/#server_id) system variable on all server instances to generate different `xid` values.
<ul start="1"><li>`OFF` Uses the storage engine protocol.
</li><li>`ON` Uses the server protocol.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Session Value:</strong> `OFF`

---

#### `spider_internal_xa_id_type`

- <strong>Description:</strong> The type of internal_xa id.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `0`
- <strong>Range:</strong> `-1` to `1`

---

#### `spider_internal_xa_snapshot`

- <strong>Description:</strong> Limitation for reading consistent data on all backend servers when using MariaDB's internal XA implementation and `START TRANSACTION WITH CONSISTENT SNAPSHOT`.
<ul start="1"><li>`0` Raise error when using a Spider table.
</li><li>`1` Raise error when issued a `START TRANSACTION` statement.
</li><li>`2` Takes a consistent snapshot on each backend, but loses global consistency.
</li><li>`3` Starts transactions with XA, but removes `CONSISTENT SNAPSHOT`.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Range:</strong> `0` to `3`
- <strong>Default Session Value:</strong> `0`

---

#### `spider_load_crd_at_startup`

- <strong>Description:</strong> Whether to load CRD from the system table at startup.
<ul start="1"><li>`-1` Use table parameter
</li><li>`0` Do not load
</li><li>`1` Load
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `spider_load_sts_at_startup`

- <strong>Description:</strong> Whether to load STS from the system table at startup.
<ul start="1"><li>`-1` Use table parameter
</li><li>`0` Do not load
</li><li>`1` Load
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `spider_local_lock_table`

- <strong>Description:</strong> Whether to push [LOCK TABLES](/kb/en/transactions-lock/) statements down to the remote server.
<ul start="1"><li>`0` Transmitted.
</li><li>`1` Not transmitted.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `spider_lock_exchange`

- <strong>Description:</strong> 
Whether to convert [SELECT... LOCK IN SHARE MODE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/lock-in-share-mode/) and [SELECT... FOR UPDATE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/for-update/) statements into a [LOCK TABLE](/kb/en/lock/) statement.
<ul start="1"><li>`0` Not converted.
</li><li>`1` Converted.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `spider_log_result_error_with_sql`

- <strong>Description:</strong> How to log SQL statements with result errors.
<ul start="1"><li>`0` No log
</li><li>`1` Log error
</li><li>`2` Log warning summary
</li><li>`3` Log warning
</li><li>`4` Log info (Added in [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/))
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4`

---

#### `spider_log_result_errors`

- <strong>Description:</strong> Log results from data nodes to the Spider node in the [Error Log](/mariadb-administration/server-monitoring-logs/error-log/).  Performs no logging by default.
<ul start="1"><li>`0` : Logs no errors from data nodes.
</li><li>`1` : Logs errors from data nodes.
</li><li>`2` : Logs errors from data nodes, as well as warning summaries.
</li><li>`3` : Logs errors from data nodes, as well as warning summaries and details.
</li><li>`4` : Logs errors from data nodes, as well as warning summaries and details, and result summaries.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4`

---

#### `spider_low_mem_read`

- <strong>Description:</strong> Whether to use low memory mode when executing queries issued internally to remote servers that return result-sets.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Doesn't use low memory mode.
</li><li>`1` Uses low memory mode. 
</li></ul>
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1`

---

#### `spider_max_connections`

- <strong>Description:</strong> Maximum number of connections from Spider to a remote MariaDB servers. Defaults to `0`, which is no limit.
- <strong>Command-line:</strong> `--spider-max-connections`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `0`
- <strong>Range:</strong> `0` to `99999`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `spider_max_order`

- <strong>Description:</strong> Maximum number of columns for `ORDER BY` operations.
<ul start="1"><li>`-1` The table parameter is adopted.
</li><li>`0` and greater: Maximum number of columns.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `32767`
- <strong>Range:</strong> `-1` to `32767`
- <strong>DSN Parameter Name:</strong> `mod`

---

#### `spider_multi_split_read`

- <strong>Description:</strong> 
Whether to divide a statement into multiple SQL statements sent to the remote backend server when the optimizer resolves range retrievals to multiple conditions.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Doesn't divide statements.
</li><li>`1` Divides statements.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>DSN Parameter Name:</strong> `msr`

---

#### `spider_net_read_timeout`

- <strong>Description:</strong> TCP timeout in seconds to declare remote backend servers unresponsive when reading from a connection.  Change for high latency networks.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Less than 1 second timeout.
</li><li>`1` and greater: Timeout in seconds.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>DSN Parameter Name:</strong> `nrt`

---

#### `spider_net_write_timeout`

- <strong>Description:</strong> TCP timeout in seconds to declare remote backend servers unresponsive when writing to a connection.  Change for high latency networks.
<ul start="1"><li>`-1` The table parameter is adopted.
</li><li>`0` Less than 1 second timeout.
</li><li>`1` and more: Timeout in seconds.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>DSN Parameter Name:</strong> `nwt`

---

#### `spider_ping_interval_at_trx_start`

- <strong>Description:</strong> Resets the connection with keepalive timeout in seconds by sending a ping. 
<ul start="1"><li>`0` At every transaction.
</li><li>`1` and greater: Number of seconds.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `3600`
- <strong>Range:</strong> `0` to `2147483647`

---

#### `spider_quick_mode`

- <strong>Description:</strong> Sets the backend query buffering to cache on the remote backend server or in the local buffer.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Local buffering, it acquires records collectively with `store_result`.
</li><li>`1` Remote buffering, it acquires records one by one.  Interrupts don't wait and recovery on context switch back.
</li><li>`2` Remote buffering, it acquires records one by one.  Interrupts wait to the end of the acquisition.
</li><li>`3` Local buffering, uses a temporary table on disk when the result-set is greater than the value of the [spider_quick_page_size](#spider_quick_page_size) system variable.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `3`
- <strong>DSN Parameter Name:</strong> `qmd`

---

#### `spider_quick_page_byte`

- <strong>Description:</strong> Memory limit by size in bytes in a page when acquired record by record.
<ul start="1"><li>`-1` The table parameter is used. When quick_mode is 1 or 2, Spider stores at least 1 record even if quick_page_byte is smaller than 1 record. When quick_mode is 3, quick_page_byte is used for judging using temporary tables. That is given priority when spider_quick_page_byte is set.
</li><li>`0` or greater: Memory limit.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `9223372036854775807`
- <strong>Introduced:</strong> [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/), [MariaDB 10.3.13](/kb/en/mariadb-10313-release-notes/)

---

#### `spider_quick_page_size`

- <strong>Description:</strong> Number of records in a page when acquired record by record. 
<ul start="1"><li>`-1` The table parameter is adopted.
</li><li>`0` or greater: Number of records.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `100`
- <strong>Range:</strong> `-1` to `9223372036854775807`
- <strong>DSN Parameter Name:</strong> `qps`

---

#### `spider_read_only_mode`

- <strong>Description:</strong> Whether to allow writes on Spider tables. 
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Allows writes to Spider tables. 
</li><li>`1` Makes tables read- only.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `1`
- <strong>DSN Parameter Name:</strong> `rom`

---

#### `spider_remote_access_charset`

- <strong>Description:</strong> Forces the specified session [character set](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/) when connecting to the backend server.  This can improve connection time performance.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Session Value:</strong> `null`

---

#### `spider_remote_autocommit`

- <strong>Description:</strong> Sets the auto-commit mode when connecting to backend servers.  This can improve connection time performance.
<ul start="1"><li>`-1` Doesn't change the auto-commit mode.
</li><li>`0` Sets the auto-commit mode to `0`.
</li><li>`1` Sets the auto-commit mode to `1`.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1`

---

#### `spider_remote_default_database`

- <strong>Description:</strong> Sets the local default database when connecting to backend servers.  This can improve connection time performance.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Session Value:</strong> Empty string

---

#### `spider_remote_sql_log_off`

- <strong>Description:</strong> Sets the [sql_log_off](/kb/en/server-system-variables/#sql_log_off) system variable to use when connecting to backend servers.
<ul start="1"><li>`-1` Doesn't set the value. 
</li><li>`0` Doesn't log Spider SQL statements to remote backend servers.
</li><li>`1` Logs SQL statements on remote backend
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1`

---

#### `spider_remote_time_zone`

- <strong>Description:</strong> Forces the specified [time zone](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones/) setting when connecting to backend servers. 
This can improve connection performance when you know the time zone.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Session Value:</strong> `null`

---

#### `spider_remote_trx_isolation`

- <strong>Description:</strong>  Sets the [Transaction Isolation Level](/kb/en/set-transaction/#isolation-levels) when connecting to the backend server. 
<ul start="1"><li>`-1` Doesn't set the Isolation Level.
</li><li>`0` Sets to the `READ UNCOMMITTED` level.
</li><li>`1` Sets to the `READ COMMITTED` level.
</li><li>`2` Sets to the `REPEATABLE READ` level.
</li><li>`3` Sets to the `SERIALIZABLE` level.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `3`

---

#### `spider_remote_wait_timeout`

- <strong>Description:</strong>  Wait timeout in seconds on remote server. `-1` means not set.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>Introduced:</strong> [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/)

---

#### `spider_reset_sql_alloc`

- <strong>Description:</strong> Resets the per connection SQL buffer after an SQL statement executes.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Doesn't reset.
</li><li>`1` Resets.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `1`
- <strong>Range:</strong> `-1` to `1`
- <strong>DSN Parameter Name:</strong> `rsa`

---

#### `spider_same_server_link`

- <strong>Description:</strong> Enables the linking of a table to the same local instance.
<ul start="1"><li>`0` Disables linking. 
</li><li>`1` Enables linking. 
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Session Value:</strong> `OFF`

---

#### `spider_second_read`

- <strong>Description:</strong> Number of second read records on the backend server when the first records are found from the first read.  Spider continues scanning a range, adding a `LIMIT` using the [spider_first_read](#spider_first_read) and [spider_second_read](#spider_second_read) variables.
<ul start="1"><li>`-1` Uses the table parameter.
</li><li>`0` Usually retrieves records. 
</li><li>`1` and greater: Number of records.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `9223372036854775807`
- <strong>Range:</strong> `-1` to `9223372036854775807`
- <strong>DSN Parameter Name:</strong> `srd`

---

#### `spider_select_column_mode`

- <strong>Description:</strong> Mode for column retrieval from remote backend server.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Uses index columns when the `SELECT` statement can resolve with an index, otherwise uses all columns.
</li><li>`1` Uses all columns judged necessary to resolve the query.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `1`
- <strong>Range:</strong> `-1` to `1`
- <strong>DSN Parameter Name:</strong> `scm`

---

#### `spider_selupd_lock_mode`

- <strong>Description:</strong> Local lock mode on `INSERT SELECT`.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Takes no locks. 
</li><li>`1` Takes shared locks. 
</li><li>`2` Takes exclusive locks.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `1`
- <strong>Range:</strong> `-1` to `2`
- <strong>DSN Parameter Name:</strong> `slm#`

---

#### `spider_semi_split_read`

- <strong>Description:</strong> 
Whether to use chunk retrieval with offset and limit parameters on SQL statements sent to the remote backend server when using the [spider_split_read](#spider_split_read) system variable.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Doesn't use chunk retrieval.
</li><li>`1 or more ` Uses chunk retrieval. 
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>DSN Parameter Name:</strong> `ssr#`

---

#### `spider_semi_split_read_limit`

- <strong>Description:</strong> Sets the limit value for the [spider_semi_split_read](#spider_semi_split_read) system variable.
<ul start="1"><li>`-1` Uses the table parameter.
</li><li>`0` or more: The limit value.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `9223372036854775807`
- <strong>Range:</strong> `-1` to `9223372036854775807`
- <strong>DSN Parameter Name:</strong> `ssl#`

---

#### `spider_semi_table_lock`

- <strong>Description:</strong> Enables semi-table locking.  This adds a [LOCK TABLES](/kb/en/transactions-lock/) statement to SQL executions sent to the remote backend server when using non-transactional storage engines to preserve consistency between roundtrips.
<ul start="1"><li>`0` Disables semi-table locking. 
</li><li>`1` Enables semi-table locking. 
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `1`
- <strong>Range:</strong> `0` to `1`
- <strong>DSN Parameter Name:</strong> `stl#`

---

#### `spider_semi_table_lock_connection`

- <strong>Description:</strong> Whether to use multiple connections with semi-table locking.  To enable semi-table locking, use the [spider_semi_table_lock](#spider_semi_table_lock) system variable.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Uses the same connection.
</li><li>`1` Uses different connections.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `1`
- <strong>Range:</strong> `-1` to `1`
- <strong>DSN Parameter Name:</strong> `stc#`

---

#### `spider_semi_trx`

- <strong>Description:</strong> Enables semi-transactions.  This controls transaction consistency when an SQL statement is split into multiple statements issued to the backend servers.  You can preserve or relax consistency as need.  Spider encapsulates auto-committed SQL statements within a transaction on the remote backend server.  When using `READ COMMITTED` or `READ UNCOMMITTED` [transaction isolation levels](/kb/en/set-transaction/#isolation-levels) to force consistency, set the [spider_semi_trx_isolation](#spider_semi_trx_isolation) system variable to `2`.
<ul start="1"><li>`0` Disables semi-transaction consistency.
</li><li>`1` Enables semi-transaction consistency.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Session Value:</strong> `ON`

---

#### `spider_semi_trx_isolation`

- <strong>Description:</strong> Set consistency during range SQL execution when [spider_sync_trx_isolation](#spider_sync_trx_isolation) is 1
<ul><li>`-1` OFF
</li><li>`0` READ UNCOMMITTED
</li><li>`1` READ COMMITTED
</li><li>`2` REPEATABLE READ
</li><li>`3` SERIALIZABLE
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `3`

---

#### `spider_skip_default_condition`

- <strong>Description:</strong> Whether to compute condition push downs.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Computes condition push downs.
</li><li>`1` Doesn't compute condition push downs.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `1`
- <strong>DSN Parameter Name:</strong> `sdc`

---

#### `spider_skip_parallel_search`

- <strong>Description:</strong> Whether to skip parallel search by specific conditions.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--spider-skip-parallel-search=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `3`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `spider_slave_trx_isolation`

- <strong>Description:</strong> Transaction isolation level when Spider table is used by slave SQL thread.
<ul><li>`-1` off
</li><li>`0` read uncommitted
</li><li>`1` read committed
</li><li>`2` repeatable read
</li><li>`3` serializable
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--spider-slave-trx-isolation=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `3`
- <strong>Introduced:</strong> [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/), [MariaDB 10.3.13](/kb/en/mariadb-10313-release-notes/)

---

#### `spider_split_read`

- <strong>Description:</strong> Number of records in chunk to retry the result when a range query is sent to remote backend servers.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` or more: Number of records.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `9223372036854775807`
- <strong>Range:</strong> `-1` to `9223372036854775807`
- <strong>DSN Parameter Name:</strong> `srd`

---

#### `spider_store_last_crd`

- <strong>Description:</strong> Whether to store last CRD result in the system table.
<ul><li>`-1` Use table parameter.
</li><li>`0` Do not store last CRD result in the system table.
</li><li>`1` Store last CRD result in the system table.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--spider-store-last-crd=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `spider_store_last_sts`

- <strong>Description:</strong> Whether to store last STS result in the system table.
<ul><li>`-1` Use table parameter.
</li><li>`0` Do not store last STS result in the system table.
</li><li>`1` Store last STS result in the system table.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--spider-store-last-sts=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `spider_strict_group_by`

- <strong>Description:</strong> 
Whether to use columns in select clause strictly for group by clause
<ul start="1"><li>`-1` Use the table parameter. 
</li><li>`0` Do not strictly use columns in select clause for group by clause
</li><li>`1` Use columns in select clause strictly for group by clause
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `spider_sts_bg_mode`

- <strong>Description:</strong> 
Enables background confirmation for table statistics.  When background confirmation is enabled, Spider uses one thread per partition to maintain table status.  Disable when the [spider_sts_interval](#spider_sts_interval) system variable is set to `0`, which causes Spider to always retrieve the latest information as need.  It is effective, when the [spider_sts_interval](#spider_sts_interval) system variable is set to `10`.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Disables background confirmation. 
</li><li>`1` Enables background confirmation (create thread per table/partition).
</li><li>`2`  Enables background confirmation (use static threads). (from MariaDB 10.)
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `1`
- <strong>Range:</strong> `-1` to `2`
- <strong>DSN Parameter Name:</strong> `sbm`

---

#### `spider_sts_interval`

- <strong>Description:</strong> Time interval of table statistics from the remote backend servers.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Retrieves the latest table statistics on request.
</li><li>`1` or more: Interval in seconds for table state confirmation.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `10`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>DSN Parameter Name:</strong> `siv`

---

#### `spider_sts_mode`

- <strong>Description:</strong>  Table statistics mode.
Mode for table statistics. The [SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/) command is used at the table level default.
<ul start="1"><li>`-1,0` Uses the table parameter. 
</li><li>`1` Uses the `SHOW` command.
</li><li>`2` Uses the Information Schema. 
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `1`
- <strong>Range:</strong> `-1` to `2`
- <strong>DSN Parameter Name:</strong> `smd`

---

#### `spider_sts_sync`

- <strong>Description:</strong> Synchronizes table statistics in partitioned tables.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Doesn't synchronize table statistics in partitioned tables. 
</li><li>`1` Synchronizes table state when opening a table, doesn't synchronize after opening.
</li><li>`2` Synchronizes table statistics. 
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Session Value:</strong> `-1`
- <strong>Default Table Value:</strong> `0`
- <strong>Range:</strong> `-1` to `2`
- <strong>DSN Parameter Name:</strong> `ssy`

---

#### `spider_support_xa`

- <strong>Description:</strong> XA Protocol for mirroring and for multi-shard transactions.
<ul start="1"><li>`1` Enables XA Protocol for these Spider operations. 
</li><li>`0` Disables XA Protocol for these Spider operations.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Table Value:</strong> `1`

---

#### `spider_sync_autocommit`

- <strong>Description:</strong> Whether to push down local auto-commits to remote backend servers.
<ul start="1"><li>`OFF` Pushes down local auto-commits.
</li><li>`ON` Doesn't push down local auto-commits. 
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Session Value:</strong> `ON`

---

#### `spider_sync_sql_mode`

- <strong>Description:</strong> Whether to sync [sql_mode](/mariadb-administration/variables-and-modes/sql-mode/).
<ul start="1"><li>`OFF` No sync
</li><li>`ON` Sync
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/)

---

#### `spider_sync_time_zone`

- <strong>Description:</strong> Whether to push the local time zone down to remote backend servers.
<ul start="1"><li>`OFF` Doesn't synchronize time zones.
</li><li>`ON` Synchronize time zones.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Session Value:</strong> `OFF`
- <strong>Removed:</strong> [MariaDB 10.3.9](/kb/en/mariadb-1039-release-notes/)

---

#### `spider_sync_trx_isolation`

- <strong>Description:</strong> Pushes local transaction isolation levels down to remote backend servers.
<ul start="1"><li>`ON` Doesn't push down local isolation levels.
</li><li>`OFF` Pushes down local isolation levels.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Session Value:</strong> `ON`

---

#### `spider_table_crd_thread_count`

- <strong>Description:</strong> Static thread count of table crd.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--spider-table-crd-thread-count=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `1` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `spider_table_init_error_interval`

- <strong>Description:</strong> Interval in seconds where the same error code is returned if table initialization fails.  Use to protect against infinite loops in table links.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `spider_table_sts_thread_count`

- <strong>Description:</strong> Static thread count of table sts.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--spider-table-sts-thread-count=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `1` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `spider_udf_ct_bulk_insert_interval`

- <strong>Description:</strong> Interval in milliseconds between bulk inserts at copying. For use with the UDF spider_copy_tables, which copies table data linked to a Spider table from the source server to destination server using bulk insert. If this interval is 0, it may cause higher write traffic.
<ul start="1"><li>`-1` Uses the UDF parameter. 
</li><li>`0` and more: Time in milliseconds.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Default Table Value:</strong> `2147483647`
- <strong>Range:</strong> `-1` to `2147483647`

---

#### `spider_udf_ct_bulk_insert_rows`

- <strong>Description:</strong>  Number of rows to insert at a time when copying during bulk inserts.
<ul start="1"><li>`-1, 0`: Uses the table parameter. 
</li><li>`1` and more: Number of rows
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Default Table Value:</strong> `18446744073709551615`
- <strong>Range:</strong> `-1` to `9223372036854775807`

---

#### `spider_udf_ds_bulk_insert_rows`

- <strong>Description:</strong> Number of rows inserted at a time during bulk inserts when the result-set is stored in a temporary table on executing a UDF.
<ul start="1"><li>`-1, 0` Uses the UDF parameter.
</li><li>`1` or more: Number of rows
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Default Table Value:</strong> `9223372036854775807`
- <strong>Range:</strong> `-1` to `9223372036854775807`

---

#### `spider_udf_ds_table_loop_mode`

- <strong>Description:</strong> Whether to store the result-set in the same temporary table when the temporary table list count for UDF is less than the result-set count on UDF execution.
<ul start="1"><li>`-1` Uses the table parameter.
</li><li>`0` Drops records.
</li><li>`1` Inserts the last table.
</li><li>`2` Inserts the first table and loops again.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `2`

---

#### `spider_udf_ds_use_real_table`

- <strong>Description:</strong> Whether to use real table for temporary table list.
<ul start="1"><li>`-1` Use UDF parameter.
</li><li>`0` Do not use real table.
</li><li>`1` Use real table.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1`

---

#### `spider_udf_table_lock_mutex_count`

- <strong>Description:</strong> Mutex count of table lock for Spider UDFs.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `20`
- <strong>Range:</strong> `1` to `4294967295`

#### `spider_udf_table_mon_mutex_count`

- <strong>Description:</strong> Mutex count of table mon for Spider UDFs.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `20`
- <strong>Range:</strong> `1` to `4294967295`

---

#### `spider_use_all_conns_snapshot`

- <strong>Description:</strong> Whether to pass `START TRANSACTION WITH SNAPSHOT` statements to all connections.
<ul start="1"><li>`OFF` Doesn't pass statement to all connections. 
</li><li>`ON` Passes statement to all connections.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Session Value:</strong> `OFF`

---

#### `spider_use_cond_other_than_pk_for_update`

- <strong>Description:</strong> Whether to use all conditions even if condition has a primary key. 
<ul start="1"><li>`0` Don't use all conditions
</li><li>`1` Use all conditions
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `1`
- <strong>Introduced:</strong> [MariaDB 10.3.13](/kb/en/mariadb-10313-release-notes/), [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)

---

#### `spider_use_consistent_snapshot`

- <strong>Description:</strong> 
Whether to push a local `START TRANSACTION WITH CONSISTENT` statement down to remote backend servers.
<ul start="1"><li>`OFF` Doesn't push the local statement down. 
</li><li>`ON` Pushes the local statement down.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `spider_use_default_database`

- <strong>Description:</strong> Whether to use the default database.
<ul start="1"><li>`OFF` Doesn't use the default database. 
</li><li>`ON` Uses the default database.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `spider_use_flash_logs`

- <strong>Description:</strong> Whether to push [FLUSH LOGS](flush-logs) statements down to remote backend servers.
<ul start="1"><li>`OFF` Doesn't push the statement down. 
</li><li>`ON` Pushes the statement down.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `spider_use_handler`

- <strong>Description:</strong> Converts [HANDLER](/sql-statements-structure/nosql/handler/) SQL statements.
When the [spider_sync_trx_isolation](#spider_sync_trx_isolation) system variable is set to `0`, Spider disables [HANDLER](/sql-statements-structure/nosql/handler/) conversions to prevent use of the statement on the [SERIALIZABLE](/kb/en/set-transaction/#serializable) isolation level.
<ul start="1"><li>`-1` Uses table parameter
</li><li>`0` Converts [HANDLER](/sql-statements-structure/nosql/handler/) statements into [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statements.
</li><li>`1` Passes [HANDLER](/sql-statements-structure/nosql/handler/) to the remote backend server.
</li><li>`2` Converts SQL statements to [HANDLER](/sql-statements-structure/nosql/handler/) statements.
</li><li>`3` Converts SQL statements to [HANDLER](/sql-statements-structure/nosql/handler/) statements and [HANDLER](/sql-statements-structure/nosql/handler/) statements to SQL statements.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `3`
- <strong>DSN Parameter Name:</strong> `uhd`

---

#### `spider_use_pushdown_udf`

- <strong>Description:</strong> 
When using a UDF function in a condition and the [engine_condition_pushdown](/kb/en/server-system-variables/#engine_condition_pushdown) system variable is set to `1`, whether to execute the UDF function locally or push it down.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Doesn't transmit the UDF 
</li><li>`1` Transmits the UDF. 
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Default Table Value:</strong> `1`
- <strong>Range:</strong> `-1` to `1`
- <strong>DSN Parameter Name:</strong> `upu`

---

#### `spider_use_snapshot_with_flush_tables`

- <strong>Description:</strong> 
Whether to encapsulate [FLUSH LOGS](flush-logs) and [UNLOCK TABLES](/kb/en/lock-tables-and-unlock-tables/) statements when `START TRANSACTION WITH CONSISTENT` and `FLUSH TABLE WITH READ LOCK` statements are sent to the remote backend servers.
<ul start="1"><li>`0` : No encapsulation. 
</li><li>`1` : Encapsulates, only when the [spider_use_all_conns_snapshot](#spider_use_all_conns_snapshot) system variable i set to `1`.  
</li><li>`2` : 
Synchronizes the snapshot using a [LOCK TABLES](/kb/en/lock-tables-and-unlock-tables/) statement and [flush|FLUSH TABLES]] at the XA transaction level.  This is only effective when the [spider_use_all_cons_snapshot](#spider_use_all_cons_snapshot) system variable is set to `1`.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `2`

---

#### `spider_use_table_charset`

- <strong>Description:</strong> Whether to use the local table [character set](/kb/en/data-types-character-sets-and-collations/) for the remote backend server connections.
<ul start="1"><li>`-1` Uses the table parameter. 
</li><li>`0` Use `utf8`.
</li><li>`1` Uses the table character set.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Default Table Value:</strong> `1`
- <strong>Range:</strong> `-1` to `1`
- <strong>DSN Parameter Name:</strong> `utc`

---

#### `spider_version`

- <strong>Description:</strong> The current Spider version.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---

#### `spider_wait_timeout`

- <strong>Description:</strong>  Wait timeout in seconds of setting to remote server. `-1` means not set.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `604800`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>Introduced:</strong> [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/)

---

#### `spider_xa_register_mode`

- <strong>Description:</strong> Mode of XA transaction register into system table.
<ul start="1"><li>`0` Register all XA transactions
</li><li>`1` Register only write XA transactions
</li></ul>
- <strong>Command-line:</strong> <code class="fixed" style="white-space:pre-wrap">--spider-xa-register-mode=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `1`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)