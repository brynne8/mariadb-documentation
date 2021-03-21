# MyISAM System Variables

This page documents system variables related to the [MyISAM](/kb/en/myisam/) storage engine. For options, see [MyISAM Options](/kb/en/mysqld-options/#myisam-options).

See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting them.

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `key_buffer_size`

- <strong>Description:</strong> Size of the buffer for the index blocks used by MyISAM tables and shared for all threads. See [Optimizing key_buffer_size](/replication/optimization-and-tuning/system-variables/optimizing-key_buffer_size/) for more on selecting the best value.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--key-buffer-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `134217728`
- <strong>Range:</strong> `8` upwards (upper limit determined by operating system per process limit)

---

#### `key_cache_age_threshold`

- <strong>Description:</strong> The lower the setting, the more quickly buffers move from the hot key cache sublist to the warm sublist.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--key-cache-age-threshold=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `300`
- <strong>Range:</strong> `100` to `4294967295`

---

#### `key_cache_block_size`

- <strong>Description:</strong> [MyISAM](/kb/en/myisam/) key cache block size in bytes .
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--key-cache-block-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1024`
- <strong>Range:</strong> `512` to `16384`

---

#### `key_cache_division_limit`

- <strong>Description:</strong> Percentage to use for the warm key cache buffer list (the remainder is allocated between the hot and cold caches).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--key-cache-division-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100`
- <strong>Range:</strong> `1` to `100`

---

#### `key_cache_file_hash_size`

- <strong>Description:</strong> Number of hash buckets for open and changed files. If you have many MyISAM files open you should increase this for faster flushing of changes. A good value is probably 1/10th of the number of possible open MyISAM files.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--key-cache-file-hash-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `512`
- <strong>Range:</strong> `128` to `16384`
- <strong>Introduced:</strong> [MariaDB 10.0.13](/kb/en/mariadb-10013-release-notes/)

---

#### `key_cache_segments`

- <strong>Description:</strong> The number of segments in a key cache. See [Segmented Key Cache](/replication/optimization-and-tuning/system-variables/segmented-key-cache/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--key-cache-segments=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> numeric
- <strong>Default Value:</strong> <code class="fixed" style="white-space:pre-wrap">0</code> <em>(non-segmented)</em>
- <strong>Range:</strong> `0` to `64`

---

#### `myisam_block_size`

- <strong>Description:</strong> Block size to be used for MyISAM index pages.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--myisam-block-size=# </code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1024`

---

#### `myisam_data_pointer_size`

- <strong>Description:</strong> Size in bytes of the default pointer, used in a [MyISAM](/kb/en/myisam/) [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) with no MAX_ROWS option.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--myisam-data-pointer-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `6`
- <strong>Range:</strong> `2` to `7`

---

#### `myisam_max_extra_sort_file_size`

- <strong>Description:</strong> Removed in MySQL 5.0.6, was used as a way to force long character keys in large tables to use the key cache method.
- <strong>Removed:</strong> MySQL 5.0.6

---

#### `myisam_max_sort_file_size`

- <strong>Description:</strong> Maximum size in bytes of the temporary file used while recreating a MyISAM index. If the this size is exceeded, the slower process of using the key cache is done instead.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--myisam-max-sort-file-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value - 32 bit:</strong> `2147483648`
- <strong>Default Value - 64 bit:</strong> `9223372036854775807`

---

#### `myisam_mmap_size`

- <strong>Description:</strong> Maximum memory in bytes that can be used for memory mapping compressed MyISAM files. Too high a value may result in swapping if there are many compressed MyISAM tables.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--myisam-mmap-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value - 32 bit:</strong> `4294967295`
- <strong>Default Value - 64 bit:</strong> `18446744073709547520`
- <strong>Range - 32-bit:</strong> `7` to `4294967295`
- <strong>Range - 64-bit:</strong> `7` to `18446744073709547520`

---

#### `myisam_recover_options`

- <strong>Description:</strong> MyISAM recovery mode. Multiple options can be selected, comma-delimited. Using no argument is equivalent to specifying `DEFAULT`, while specifying "" is equivalent to `OFF`. If enabled each time the server opens a MyISAM table, it checks whether it has been marked as crashed, or wasn't closed properly. If so, mysqld will run a check and then attempt to repair the table, writing to the error log beforehand.
<ul start="1"><li><strong>OFF</strong>: No recovery.
</li><li><strong>BACKUP</strong>: If the data file is changed while recovering, saves a backup of the .MYD data file. t.MYD will be saved as t.MYD-datetime.BAK.
</li><li><strong>BACKUP_ALL</strong>: Same as `BACKUP` but also backs up the .MYI index file. t.MYI will be saved as t.MYI-datetime.BAK. 
</li><li><strong>DEFAULT</strong>: Recovers without backing up, forcing, or quick checking.
</li><li><strong>FORCE</strong>: Runs the recovery even if it determines that more than one row from the data file will be lost.
</li><li><strong>QUICK</strong>: Does not check rows in the table if there are no delete blocks.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--myisam-recover-options[=name]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong>
<ul start="1"><li>`BACKUP,QUICK` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/))
</li><li>`DEFAULT` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))
</li><li>`OFF`
</li></ul>
- <strong>Valid Values:</strong> `OFF`, `DEFAULT`, `BACKUP`, `BACKUP_ALL`, `FORCE` or `QUICK`

---

#### `myisam_repair_threads`

- <strong>Description:</strong> If set to more than `1`, the default, MyISAM table indexes each have their own thread during repair and sorting. Increasing from the default will usually result in faster repair, but will use more CPU and memory.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--myisam-repair-threads=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range - 32-bit:</strong> `1` to `4294967295`
- <strong>Range - 64-bit:</strong> `1` to `18446744073709547520`

---

#### `myisam_sort_buffer_size`

- <strong>Description:</strong> Size in bytes of the buffer allocated when creating or sorting indexes on a MyISAM table.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--myisam-sort-buffer-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `134217720` (128MB), `8388608` (8MB - before [MariaDB 10.0.3](/kb/en/mariadb-1003-release-notes/))
- <strong>Range:</strong> `4096` to `18446744073709547520`

---

#### `myisam_stats_method`

- <strong>Description:</strong> Determines how NULLs are treated for [MyISAM](/kb/en/myisam/) index statistics purposes. If set to `nulls_equal`, the default, all NULL index values are treated as a single group. This is usually fine, but if you have large numbers of NULLs the average group size is slanted higher, and the optimizer may miss using the index for ref accesses when it would be useful. If set to `nulls_unequal`, the opposite approach is taken, with each NULL forming its own group of one. Conversely, the average group size is slanted lower, and the optimizer may use the index for ref accesses when not suitable. Setting to `nulls_ignored` ignores NULLs altogether from index group calculations. See also [Index Statistics](/replication/optimization-and-tuning/optimization-and-indexes/index-statistics/), [aria_stats_method](/kb/en/aria-server-system-variables/#aria_stats_method), [innodb_stats_method](/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_method).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--myisam-stats-method=name</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `nulls_equal`
- <strong>Valid Values:</strong> `nulls_equal`, `nulls_unequal`, `nulls_ignored`

---

#### `myisam_use_mmap`

- <strong>Description:</strong> If set to `1` (0 is default), memory mapping will be used to reading and writing MyISAM tables.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--myisam-use-mmap</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---