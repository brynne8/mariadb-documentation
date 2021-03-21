# PBXT System Variables

##### MariaDB until [5.3](/kb/en/what-is-mariadb-53/)

PBXT is no longer maintained, and is not part of [MariaDB 5.5](/kb/en/what-is-mariadb-55/) or later.

This page documents system variables related to the [PrimeBase XT  storage engine (PBXT)](/kb/en/pbxt/). PBXT is no longer maintained, and is not part of [MariaDB 5.5](/kb/en/what-is-mariadb-55/) or later.

See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables) for a complete list of system variables and instructions on setting them.

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables).

Variables that specify a number of bytes may include a unit indication after
the value. For example: 100KB, 64MB, etc. There should be no space between the
number and the unit. Units are case insensitive (KB = Kb = kb). If no unit is
specified then bytes is assumed. The recognized units are:

- <strong>KB</strong> (or <strong>K</strong>) - Kilobyte, 1024 bytes
- <strong>MB</strong> (or <strong>M</strong>) - Megabyte, 1024 KB
- <strong>GB</strong> (or <strong>G</strong>) - Gigabyte, 1024 MB
- <strong>TB</strong> (or <strong>T</strong>) - Terabyte, 1024 GB
- <strong>PB</strong> (or <strong>P</strong>) - Petabyte, 1024 TB

Variables which use this type of value are: `pbxt_index_cache_size`,
`pbxt_record_cache_size`, `pbxt_log_cache_size`,
`pbxt_log_file_threshold`, `pbxt_checkpoint_frequency`,
`pbxt_data_log_threshold`, `pbxt_log_buffer_size`,
`pbxt_data_file_grow_size`, and `pbxt_row_file_grow_size`.

#### PBXT Data Log Variables

PBXT stores part of the database in the data logs. This is mostly data from
rows containing long VARCHAR fields or BLOB data. The data logs are managed by
the "compactor" thread. When a record is deleted from a data log, the data is
marked as garbage. When the total garbage in a data log reaches a certain
threshold, the compactor thread compacts the data log by copying the valid data
to a new data log, and deleting the old data log.

#### Options for PBXT

<table><tbody><tr><th>Option</th><th>Default Value</th></tr>
<tr><td><code><code>--</code>pbxt</code></td><td><code>ON</code></td></tr>
<tr><td><code><code>--</code>pbxt-max-threads</code></td><td><code>0</code></td></tr>
<tr><td><code><code>--</code>pbxt-statistics</code></td><td><code>ON</code></td></tr>
</tbody></table>

#### `pbxt_auto_increment_mode`

- <strong>Description:</strong> The parameter determines how PBXT manages auto-increment values. Possible values are '`0`' (MySQL standard) or '`1`' (Previous IDs are never re-used).<br><br>In the standard 'MySQL' mode it is possible that an auto-increment value is re-issued. This occurs when the maximum auto-increment value is deleted, and then MariaDB is restarted. This occurs because the next auto-increment value to be issued is determined at startup by retrieving the current maximum auto-increment value from the table.<br><br>In mode 1, auto-increment values are never re-issued because PBXT automatically incrementing the table level AUTO_INCREMENT table option. The AUTO_INCREMENT table is incremented in steps of 100. Since this requires the table file to be flushed to disk, this can influence performance.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-auto-increment-mode=#</code>
- <strong>Default Value:</strong> `0`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_checkpoint_frequency`

- <strong>Description:</strong> The amount of data written to the transaction log before a checkpoint is performed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-checkpoint-frequency=#</code>
- <strong>Default Value:</strong> `24MB`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_data_file_grow_size`

- <strong>Description:</strong> The grow size of the handle data (.xtd) files.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-data-file-grow-size=#</code>
- <strong>Default Value:</strong> `2MB`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_data_log_threshold`

- <strong>Description:</strong> The maximum size of a data log file. PBXT can create a maximum of 32000 data logs, which are used by all tables. So the value of this variable can be increased to increase the total amount of data that can be stored in the database.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-data-log-threshold=#</code>
- <strong>Default Value:</strong> `64MB`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_flush_log_at_trx_commit`

- <strong>Description:</strong> This variable specifies the durability of recently committed transactions. By reducing the durability, the speed of write operations can be increased.<br><br>'`0`' - Lowest durability, the transaction log is not written or flushed on transaction commit. In this case it is possible to loose transactions if the server executable crashes.<br>'`1`' - Full-durability, the transaction log is written and flushed on every transaction commit.<br>'`2`' - Medium durabilty, the transaction log is written, but not flushed on transaction commit. In this case it is possible to loose transactions of the server machine crashes (for example, a power failer).<br><br>In all cases, the transaction log is flushed at least once every second. This means that it is only every possible to loose database changes that occurred within the last second.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-flush-log-at-trx-commit=#</code>
- <strong>Default Value:</strong> `1`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_garbage_threshold`

- <strong>Description:</strong> The percentage of garbage in a data log file before it is compacted. This is a value between 1 and 99.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-garbage-threshold=#</code>
- <strong>Default Value:</strong> `50`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_index_cache_size`

- <strong>Description:</strong> The amount of memory allocated to the index cache. The memory allocated here is used only for caching index pages (.xti files).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-index-cache-size=#</code>
- <strong>Default Value:</strong> `32MB`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_log_buffer_size`

- <strong>Description:</strong> This is the size of the buffer used when writing a data log. The engine allocates one buffer per thread, but only if the thread is required to write a data log.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-log-buffer-size=#</code>
- <strong>Default Value:</strong> `256MB`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_log_cache_size`

- <strong>Description:</strong> The size of a transaction log file (xlog-*.xt files) before "rollover", and a new log file is created.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-log-cache-size=#</code>
- <strong>Default Value:</strong> `32MB`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_log_file_count`

- <strong>Description:</strong> The number of transaction log files on disk before logs that are no longer required are deleted, default value is 3. The number of transaction logs on disk may exceed this number if the logs are still being read.<br><br>If a transaction log has been read (i.e. the log is offline), it will be recycled for writing again, unless it must be deleted because the number of logs on disk exceeds this threshold. Recycling logs is an optimization because the writing a pre-allocated file is faster then writing to the end of a file.<br><br>Note: an exception to this rule is Mac OS X. On Mac OS X old log files are not recycled because writing pre-allocated file is slower than writing to the end of file.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-log-file-count=#</code>
- <strong>Default Value:</strong> `3`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_log_file_threshold`

- <strong>Description:</strong> The size of a transaction log file (xlog-*.xt files) before "rollover", and a new log file is created.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-log-file-threshold=#</code>
- <strong>Default Value:</strong> `32MB`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_offline_log_function`

- <strong>Description:</strong> This variable determines what happens to a transaction log when it is offline. A log is offline if PBXT is no longer reading or writing to the log. There are 3 possibilities:<br><br>'`0`' - Recycle log (default). This means the log is renamed and written again.<br>'`1`' - Delete log (default on Mac OS X).<br>'`2`' - Keep log. The logs can be used to repeat all operations that were applied to the database.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-offline-log-function=#</code>
- <strong>Default Value:</strong> `0`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_record_cache_size`

- <strong>Description:</strong> This is the amount of memory allocated to the record cache used to cache table data. This memory is used to cache changes to the handle data (.xtd) and row index (.xtr) files.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-record-cache-size=#</code>
- <strong>Default Value:</strong> `32MB`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_row_file_grow_size`

- <strong>Description:</strong> The grow size of the row index (.xtr) files.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-row-file-grow-size=#</code>
- <strong>Default Value:</strong> `256KB`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_sweeper_priority`

- <strong>Description:</strong> Determines the priority of the background Sweeper thread. Possible values are '`0`' (Low), '`1`' (Normal), or '`2`' (High). The Sweeper is responsible for removing deleted records and index entries (deleted records also result from UPDATE statements). If many old deleted records accumulate search operations become slower. Therefore it may improve performance to increase the priority of the Sweeper on a machine with 4 or more cores.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-sweeper-priority=#</code>
- <strong>Default Value:</strong> `0`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_support_xa`

- <strong>Description:</strong> This variable determines if XA (2-phase commit) support is enabled.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-support-xa=#</code>
- <strong>Default Value:</strong> `TRUE`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---

#### `pbxt_transaction_buffer_size`

- <strong>Description:</strong> The size of the global transaction log buffer (the engine allocates 2 buffers of this size). Data to be written to a transaction log file is first written to the transaction log buffer. Since the buffer is flushed on transaction commit, it only makes sense to use a large transaction log buffer if you have longer running transactions, or many transaction running in parallel.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pbxt-transaction-buffer-size=#</code>
- <strong>Default Value:</strong> `1MB`
- <strong>Removed:</strong> <a undefined>MariaDB 5.5</a>

---