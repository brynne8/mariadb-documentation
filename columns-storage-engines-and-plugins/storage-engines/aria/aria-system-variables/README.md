# Aria System Variables

This page documents system variables related to the [Aria storage engine](/columns-storage-engines-and-plugins/storage-engines/aria/). For options that are not system variables, see [Aria Options](/kb/en/mysqld-options/#aria-storage-engine-options).

See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting system variables.

Also see the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `aria_block_size`

- <strong>Description:</strong> Block size to be used for Aria index pages. Changing this requires dumping, deleting old tables and deleting all log files, and then restoring your Aria tables. If key lookups take too long (and one has to search roughly 8192/2 by default to find each key), can be made smaller, e.g. `2048` or `4096`.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-block-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8192`

---

#### `aria_checkpoint_interval`

- <strong>Description:</strong> Interval in seconds between automatic checkpoints. 0 means 'no automatic checkpoints' which makes sense only for testing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-checkpoint-interval=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `30`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `aria_checkpoint_log_activity`

- <strong>Description:</strong> Number of bytes that the transaction log has to grow between checkpoints before a new checkpoint is written to the log.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">aria-checkpoint-log-activity=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1048576`
- <strong>Range</strong> `0` to `4294967295`

---

#### `aria_encrypt_tables`

- <strong>Description:</strong> Enables automatic encryption of all user-created Aria tables that have the <a undefined>ROW_FORMAT</a> table option set to <a undefined>PAGE</a>. See [Data at Rest Encryption](/kb/en/data-at-rest-encryption/) and [Enabling Encryption for User-created Tables](/kb/en/encrypting-data-for-aria/#enabling-encryption-for-user-created-tables).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">aria-encrypt-tables={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `aria_force_start_after_recovery_failures`

- <strong>Description:</strong> Number of consecutive log recovery failures after which logs will be automatically deleted to cure the problem; 0 (the default) disables the feature.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-force-start-after-recovery-failures=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong> Default Value:</strong> `0`

---

#### `aria_group_commit`

- <strong>Description:</strong> Specifies Aria [group commit mode](/columns-storage-engines-and-plugins/storage-engines/aria/aria-group-commit/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria_group_commit="value"</code>
- <strong>Alias:</strong> `maria_group_commit`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Valid values:</strong>
<ul start="1"><li>`none` - <em>Group commit is disabled.</em>
</li><li>`hard` - <em>Wait the number of microseconds specified by
   aria_group_commit_interval before actually doing the commit. If the interval
   is 0 then just check if any other threads have requested a commit during the
   time this commit was preparing (just before sync() file) and send their data to
   disk also before sync().</em>
</li><li>`soft` - <em>The service thread will wait the specified time and then sync()
   to the log. If the interval is 0 then it won't wait for any commits (this is
   dangerous and should generally not be used in production)</em>
</li></ul>
- <strong>Default Value:</strong> `none`

---

#### `aria_group_commit_interval`

- <strong>Description:</strong> Interval between [Aria group commits](/columns-storage-engines-and-plugins/storage-engines/aria/aria-group-commit/) in microseconds (1/1000000 second) for other threads to come and do a commit in "hard" mode and sync()/commit at all in "soft" mode. Option only has effect if [aria_group_commit](#aria_group_commit) is used.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria_group_commit_interval=#</code>
- <strong>Alias:</strong> `maria_group_commit_interval`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> numeric
- <strong>Valid Values:</strong>
<ul start="1"><li><strong>Default Value:</strong> `0` <em>(no waiting)</em>
</li><li><strong>Range:</strong> `0-4294967295`
</li></ul>

---

#### `aria_log_file_size`

- <strong>Description:</strong> Limit for Aria transaction log size
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-log-file-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1073741824`

---

#### `aria_log_purge_type`

- <strong>Description:</strong> Specifies how the Aria transactional log will be purged. Set to `at_flush` to keep a copy of the transaction logs (good as an extra backup). The logs will stay until the next [FLUSH LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/);
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-log-purge-type=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `immediate`
- <strong>Valid Values:</strong> `immediate`, `external`, `at_flush`

---

#### `aria_max_sort_file_size`

- <strong>Description:</strong> Don't use the fast sort index method to created index if the temporary file would get bigger than this.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-max-sort-file-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `9223372036853727232`
- <strong>Range:</strong> `0` to `9223372036854775807`

---

#### `aria_page_checksum`

- <strong>Description:</strong> Determines whether index and data should use page checksums for extra safety. Can be overridden per table with PAGE_CHECKSUM clause in [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-page-checksum=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `aria_pagecache_age_threshold`

- <strong>Description:</strong> This characterizes the number of hits a hot block has to be untouched until it is considered aged enough to be downgraded to a warm block. This specifies the percentage ratio of that number of hits to the total number of blocks in the page cache.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-pagecache-age-threshold=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `300`
- <strong>Range:</strong> `100` to `9999900`

---

#### `aria_pagecache_buffer_size`

- <strong>Description:</strong> The size of the buffer used for index blocks for Aria tables. Increase this to get better index handling (for all reads and multiple writes) to as much as you can afford.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-pagecache-buffer-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `134217720` (128MB)
- <strong>Range:</strong> `131072` (128KB) upwards

---

#### `aria_pagecache_division_limit`

- <strong>Description:</strong> The minimum percentage of warm blocks in the key cache.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-pagecache-division-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100`
- <strong>Range:</strong> `1` to `100`

---

#### `aria_pagecache_file_hash_size`

- <strong>Description:</strong> Number of hash buckets for open and changed files.  If you have many Aria files open you should increase this for faster flushing of changes. A good value is probably 1/10th of the number of possible open Aria files.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-pagecache-file-hash-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `512`
- <strong>Range:</strong> `128` to `16384`
- <strong>Introduced:</strong> [MariaDB 10.0.13](/kb/en/mariadb-10013-release-notes/)

---

#### `aria_recover`

- <strong>Description:</strong> `aria_recover` has been renamed to `aria_recover_options` in [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/). See [aria_recover_options](#aria_recover_options) for the description.

---

#### `aria_recover_options`

- <strong>Description:</strong> Specifies how corrupted tables should be automatically repaired. More than one option can be specified, for example `FORCE,BACKUP`.
<ul start="1"><li>`NORMAL`: Normal automatic repair, the default until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)
</li><li>`OFF`: Autorecovery is disabled, the equivalent of not using the option
</li><li>`QUICK`: Does not check rows in the table if there are no delete blocks.
</li><li>`FORCE`: Runs the recovery even if it determines that more than one row from the data file will be lost. 
</li><li>`BACKUP`: Keeps a backup of the data files.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-recover-options[=#]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong>
<ul start="1"><li>`BACKUP,QUICK` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/))
</li><li>`NORMAL` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))
</li></ul>
- <strong>Valid Values:</strong> `NORMAL`, `BACKUP`, `FORCE`, `QUICK`, `OFF`
- <strong>Introduced:</strong> [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/)

---

#### `aria_repair_threads`

- <strong>Description:</strong> Number of threads to use when repairing Aria tables. The value of 1 disables parallel repair. Increasing from the default will usually result in faster repair, but will use more CPU and memory.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-repair-threads=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`

---

#### `aria_sort_buffer_size`

- <strong>Description:</strong> The buffer that is allocated when sorting the index when doing a [REPAIR](/sql-statements-structure/sql-statements/table-statements/repair-table/) or when creating indexes with [CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index/) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-sort-buffer-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `268434432` (from [MariaDB 10.0.3](/kb/en/mariadb-1003-release-notes/)), `134217728` (before [MariaDB 10.0.3](/kb/en/mariadb-1003-release-notes/))

---

#### `aria_stats_method`

- <strong>Description:</strong> Determines how NULLs are treated for Aria index statistics purposes. If set to `nulls_equal`, all NULL index values are treated as a single group. This is usually fine, but if you have large numbers of NULLs the average group size is slanted higher, and the optimizer may miss using the index for ref accesses when it would be useful. If set to `nulls_unequal`, the default, the opposite approach is taken, with each NULL forming its own group of one. Conversely, the average group size is slanted lower, and the optimizer may use the index for ref accesses when not suitable. Setting to `nulls_ignored` ignores NULLs altogether from index group calculations. Statistics need to be recalculated after this method is changed. See also [Index Statistics](/replication/optimization-and-tuning/optimization-and-indexes/index-statistics/), [myisam_stats_method](/kb/en/myisam-server-system-variables/#myisam_stats_method) and [innodb_stats_method](/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_method).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-stats-method=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `nulls_unequal`
- <strong>Valid Values:</strong> `nulls_equal`, `nulls_unequal`, `nulls_ignored`

---

#### `aria_sync_log_dir`

- <strong>Description:</strong> Controls syncing directory after log file growth and new file creation.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-sync-log-dir=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `NEWFILE`
- <strong>Valid Values:</strong> `NEWFILE`, `NEVER`, `ALWAYS`

---

#### `aria_used_for_temp_tables`

- <strong>Description:</strong> Readonly variable indicating whether the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine is used for temporary tables. If set to `ON`, the default, the Aria storage engine is used. If set to `OFF`, MariaDB reverts to using [MyISAM](/kb/en/myisam/) for on-disk temporary tables. The [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) storage engine is used for temporary tables regardless of this variable's setting where appropriate. The default can be changed by not using the `--with-aria-tmp-tables` option when building MariaDB.
- <strong>Commandline:</strong> No
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `deadlock_search_depth_long`

- <strong>Description:</strong> Long search depth for the [two-step deadlock detection](/columns-storage-engines-and-plugins/storage-engines/aria/aria-two-step-deadlock-detection/). Only used by the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--deadlock-search-depth-long=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `15`
- <strong>Range:</strong> `0` to `33`

---

#### `deadlock_search_depth_short`

- <strong>Description:</strong> Short search depth for the [two-step deadlock detection](/columns-storage-engines-and-plugins/storage-engines/aria/aria-two-step-deadlock-detection/). Only used by the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--deadlock-search-depth-short=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4`
- <strong>Range:</strong> `0` to `32`

---

#### `deadlock_timeout_long`

- <strong>Description:</strong> Long timeout in microseconds for the [two-step deadlock detection](/columns-storage-engines-and-plugins/storage-engines/aria/aria-two-step-deadlock-detection/). Only used by the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--deadlock-timeout-long=# </code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `50000000`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `deadlock_timeout_short`

- <strong>Description:</strong> Short timeout in microseconds for the [two-step deadlock detection](/columns-storage-engines-and-plugins/storage-engines/aria/aria-two-step-deadlock-detection/). Only used by the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--deadlock-timeout-short=# </code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10000`
- <strong>Range:</strong> `0` to `4294967295`

---