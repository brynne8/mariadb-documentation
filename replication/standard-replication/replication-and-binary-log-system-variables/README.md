# Replication and Binary Log System Variables

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

This page lists system variables that are related to [binary logging](/mariadb-administration/server-monitoring-logs/binary-log/) and [replication](/replication/).

See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting them, as well as [System variables for global transaction ID](/kb/en/gtid/#system-variables-for-global-transaction-id).

Also see [mysqld replication options](/kb/en/mysqld-options/#replication-and-binary-logging-options) for related options that are not system variables (such as [binlog_do_db](/kb/en/mysqld-options/#-binlog-do-db) and [binlog_ignore_db](/kb/en/mysqld-options/#-binlog-ignore-db)).

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `auto_increment_increment`

- <strong>Description:</strong> The increment for all [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) values on the server, by default `1`. Intended for use in master-to-master [replication](/replication/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--auto-increment-increment[=#]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` to `65535`

---

#### `auto_increment_offset`

- <strong>Description:</strong> The offset for all [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) values on the server, by default `1`. Intended for use in master-to-master [replication](/replication/). Should be smaller than <code class="fixed" style="white-space:pre-wrap">auto_increment_increment</code>, except when both variables are 1 (default setup).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--auto-increment-offset[=#]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` to `65535`

---

#### `binlog_annotate_row_events`

- <strong>Description:</strong> This option tells the master to write [annotate_rows_events](/clients-utilities/mysqlbinlog/annotate_rows_log_event/) to the binary log.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-annotate-row-events[={0|1}]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> boolean
- <strong>Default Value:</strong>
<ul start="1"><li>`ON` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/))
</li><li>`OFF` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))
</li></ul>

#### `binlog_cache_size`

- <strong>Description:</strong> If the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is active, this variable determines the size in bytes, per-connection, of the cache holding a record of binary log changes during a transaction. A separate variable, [binlog_stmt_cache_size](#binlog_stmt_cache_size), sets the upper limit for the statement cache. The [binlog_cache_disk_use](/kb/en/server-status-variables/#binlog_cache_disk_use) and [binlog_cache_use](/kb/en/server-status-variables/#binlog_cache_use) [server status variables](/replication/optimization-and-tuning/system-variables/server-status-variables/) will indicate whether this variable needs to be increased (you want a low ratio of binlog_cache_disk_use to binlog_cache_use).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-cache-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `32768`
- <strong>Range - 32 bit:</strong> `4096` to `4294967295`
- <strong>Range - 64 bit:</strong> `4096` to `18446744073709547520`

---

#### `binlog_checksum`

- <strong>Description:</strong> Specifies the type of BINLOG_CHECKSUM_ALG for log events in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/).
- <strong>Commandline: </strong>
<ul start="1"><li><code class="fixed" style="white-space:pre-wrap">--binlog-checksum=name</code>
</li><li><code class="fixed" style="white-space:pre-wrap">--binlog-checksum=[0|1]</code> 
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong>
<ul start="1"><li>`CRC32` (&gt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
</li><li>`NONE` (&lt;= [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/))
</li></ul>
- <strong>Valid Values:</strong> `NONE` (`0`), `CRC32` (`1`)

---

#### `binlog_commit_wait_count`

- <strong>Description:</strong> Configures the behavior of [group commit for the binary log](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/), which can help increase transaction throughput and is used to enable [conservative mode of in-order parallel replication](/kb/en/parallel-replication/#conservative-mode-of-in-order-parallel-replication).
With [group commit for the binary log](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/), the server can delay flushing a committed transaction into [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) until the given number of transactions are ready to be flushed as a group. The delay will however not be longer
than the value set by [binlog_commit_wait_usec](#binlog_commit_wait_usec).
The default value of 0 means that no delay is introduced.
Setting this value can reduce I/O on the binary log and give an increased opportunity for parallel apply on the slave when [conservative mode of in-order parallel replication](/kb/en/parallel-replication/#conservative-mode-of-in-order-parallel-replication) is enabled, but too high a value will decrease the transaction throughput. By monitoring the status variable [binlog_group_commit_trigger_count](/kb/en/replication-and-binary-log-status-variables/#binlog_group_commit_trigger_count) (&gt;=[MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/)) it is possible to see how often this is occurring.
- Starting with [MariaDB 10.0.18](/kb/en/mariadb-10018-release-notes/) and [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/):
If the server detects that one of the committing transactions T1 holds an
[InnoDB](/kb/en/xtradb-and-innodb/) row lock that another transaction T2 is waiting for, then the
commit will complete immediately without further delay. This helps avoid
losing throughput when many transactions need conflicting locks. This often
makes it safe to use this option without losing
throughput on a slave with [conservative mode of in-order parallel replication](/kb/en/parallel-replication/#conservative-mode-of-in-order-parallel-replication), provided the value of
[slave_parallel_threads](#slave_parallel_threads) is sufficiently high.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-commit-wait-count=#]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` - `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)

---

#### `binlog_commit_wait_usec`

- <strong>Description:</strong> Configures the behavior of [group commit for the binary log](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/), which can help increase transaction throughput and is used to enable [conservative mode of in-order parallel replication](/kb/en/parallel-replication/#conservative-mode-of-in-order-parallel-replication).
With [group commit for the binary log](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/), the server can delay flushing a committed transaction into [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) until the transaction has waited the configured number of microseconds. By monitoring the status variable [binlog_group_commit_trigger_timeout](/kb/en/replication-and-binary-log-status-variables/#binlog_group_commit_trigger_timeout) (&gt;=[MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/)) it is possible to see how often group commits are made due to `binlog_commit_wait_usec`.  As soon as the number of pending commits reaches [binlog_commit_wait_count](#binlog_commit_wait_count), the wait will be terminated, though. Thus, this setting only takes effect if `binlog_commit_wait_count` is non-zero.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-commit-wait-usec#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100000`
- <strong>Range:</strong> `0` - `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)

---

#### `binlog_direct_non_transactional_updates`

- <strong>Description:</strong> [Replication](/replication/) inconsistencies can occur due when a transaction updates both transactional and non-transactional tables and the updates to the non-transactional tables are visible before being written to the binary log. This is because, to preserve causality, the non-transactional statements are written to the transaction cache, which is only flushed on commit. Setting binlog_direct_non_transactional_updates to 1 (0 is default) will cause non-transactional tables to be written straight to the binary log, rather than the transaction cache. This setting has no effect when row-based binary logging is used, as it requires statement-based logging. See [binlog_format](#binlog_format). Use with care, and only in situations where no dependencies exist between the non-transactional and transactional tables, for example INSERTing into a non-transactional table based upon the results of a SELECT from a transactional table.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-direct-non-transactional-updates[=value]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF (0)`

---

#### `binlog_file_cache_size`

- <strong>Description:</strong> Size of in-memory cache that is allocated when reading [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) and [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) files.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-file-cache-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `16384`
- <strong>Range:</strong> `8192` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `binlog_format`

- <strong>Description:</strong> Determines whether [replication](/replication/) is row-based, statement-based or mixed. Statement-based was the default until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/). Be careful of changing the binary log format when a replication environment is already running. See [Binary Log Formats](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats/). Starting from [MariaDB 10.0.22](/kb/en/mariadb-10022-release-notes/) a slave will apply any events it gets from the master, regardless of the binary log format. `binlog_format` only applies to normal (not replicated) updates.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-format=format</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong>
<ul start="1"><li>`MIXED` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/))
</li><li>`STATEMENT` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))
</li></ul>
- <strong>Valid Values:</strong> `ROW`, `STATEMENT` or `MIXED`

---

#### `binlog_optimize_thread_scheduling`

- <strong>Description:</strong> Run fast part of group commit in a single thread, to optimize kernel thread scheduling. On by default. Disable to run each transaction in group commit in its own thread, which can be slower at very high concurrency. This option is mostly for testing one algorithm versus another, and it should not normally be necessary to change it.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-optimize-thread-scheduling</code> or <code class="fixed" style="white-space:pre-wrap">--skip-binlog-optimize-thread-scheduling</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `binlog_row_image`

- <strong>Description:</strong> Controls whether, in [row-based](/kb/en/binary-log-formats/#row-based) [replication](/replication/), rows should be logged in 'FULL', 'NOBLOB' or 'MINIMAL' formats. In row-based replication (the variable has no effect with [statement-based replication](/kb/en/binary-log-formats/#statement-based)), each row change event contains an image for matching against when choosing the row to be updated, and another image containing the changes. Before the introduction of this variable, all columns were logged for both of these images. In certain circumstances, this is not necessary, and memory, disk and network resources can be saved by partial logging. Note that to safely change this setting from the default, the table being replicated to must contain identical primary key definitions, and columns must be present, in the same order, and use the same data types as the original table. If these conditions are not met, matches may not be correctly determined and updates and deletes may diverge on the slave, with no warnings or errors returned.
<ul start="1"><li>`FULL`: All columns in the before and after image are logged. This is the default, and the only behavior in earlier versions.
</li><li>`NOBLOB`: mysqld avoids logging blob and text columns whenever possible (eg, blob column was not changed or is not part of primary key).
</li><li>`MINIMAL`: A PK equivalent (PK columns or full row if there is no PK in the table) is logged in the before image, and only changed columns are logged in the after image.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-row-image=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `FULL`
- <strong>Valid Values:</strong> `FULL`, `NOBLOB` or `MINIMAL`
- <strong>Introduced:</strong> [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)

---

#### `binlog_row_metadata`

- <strong>Description:</strong> Controls the format used for binlog metadata logging.
<ul start="1"><li>`NO_LOG`: No metadata is logged (default).
</li><li>`MINIMAL`: Only metadata required by a slave is logged.
</li><li>`FULL`: All metadata is logged.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-row-metadata=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `NO_LOG`
- <strong>Valid Values:</strong> `NO_LOG`, `MINIMAL`, `FULL`
- <strong>Introduced:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `binlog_stmt_cache_size`

- <strong>Description:</strong> If the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is active, this variable determines the size in bytes of the cache holding a record of binary log changes outside of a transaction. The variable [binlog_cache_size](#binlog_cache_size), determines the cache size for binary log statements inside a transaction. The [binlog_stmt_cache_disk_use](/kb/en/server-status-variables/#binlog_stmt_cache_disk_use) and [binlog_stmt_cache_use](/kb/en/server-status-variables/#binlog_stmt_cache_use) [server status variables](/replication/optimization-and-tuning/system-variables/server-status-variables/) will indicate whether this variable needs to be increased (you want a low ratio of binlog_stmt_cache_disk_use to binlog_stmt_cache_use).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-stmt-cache-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `32768`
- <strong>Range - 32 bit:</strong> `4096` to `4294967295`
- <strong>Range - 64 bit:</strong> `4096` to `18446744073709547520`

---

#### `default_master_connection`

- <strong>Description:</strong> In [multi-source replication](/replication/standard-replication/multi-source-replication/), specifies which connection will be used for commands and variables if you don't specify a connection.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `''` (empty string)
- <strong>Introduced:</strong> [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/)

---

#### `encrypt_binlog`

- <strong>Description:</strong> Encrypt [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/) (including [relay logs](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/)). See [Data at Rest Encryption](/kb/en/data-at-rest-encryption/) and [Encrypting Binary Logs](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/encrypting-binary-logs/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--encrypt-binlog[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)

---

#### `expire_logs_days`

- <strong>Description:</strong> Number of days after which the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) can be automatically removed. By default 0, or no automatic removal. When using [replication](/replication/), should always be set higher than the maximum lag by any slave. Removals take place when the server starts up, when the binary log is flushed, when the next binary log is created after the previous one reaches the maximum size, or when running [PURGE BINARY LOGS](/kb/en/sql-commands-purge-logs/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--expire-logs-days=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `99`

---

#### `init_slave`

- <strong>Description:</strong> Similar to [init_connect](/kb/en/server-system-variables/#init_connect), but the string contains one or more SQL statements, separated by semicolons, that will be executed by a slave server each time the SQL thread starts. These statements are only executed after the acknowledgement is sent to the slave and [START SLAVE](/kb/en/start-slave/) completes.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--init-slave=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`

---

#### `log_bin`

- <strong>Description:</strong> Whether [binary logging](/mariadb-administration/server-monitoring-logs/binary-log/) is enabled or not. If the --log-bin [option](/kb/en/mysqld-options-full-list/) is used, log_bin will be set to ON, otherwise it will be OFF.  If no `name` option is given for `--log-bin`, `datadir/'log-basename'-bin` or `'datadir'/mysql-bin` will be used (the latter if [--log-basename](/kb/en/mysqld-options/#-log-basename) is not specified). We strongly recommend you use either <code class="fixed" style="white-space:pre-wrap">--log-basename</code> or specify a filename to ensure that [replication](/replication/) doesn't stop if the real hostname of the computer changes. The name option can optionally include an absolute path. If no path is specified, the log will be written to the [data directory](/kb/en/server-system-variables/#datadir). The name can optionally include the file extension; it will be stripped and only the file basename will be used.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-bin[=name]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `log_bin_basename`

- <strong>Description:</strong> The full path of the binary log file names, excluding the extension. Its value is derived from the rules specified in `log_bin` system variable.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">No commandline option</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Read Only:</strong> `Yes`
- <strong>Introduced:</strong> [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)

---

#### `log_bin_compress`

- <strong>Description:</strong> Whether or not the binary log can be compressed. `0` (the default) means no compression. See [Compressing Events to Reduce Size of the Binary Log](/replication/standard-replication/compressing-events-to-reduce-size-of-the-binary-log/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-bin-compress</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)

---

#### `log_bin_compress_min_len`

- <strong>Description:</strong> Minimum length of sql statement (in statement mode) or record (in row mode) that can be compressed. See [Compressing Events to Reduce Size of the Binary Log](/replication/standard-replication/compressing-events-to-reduce-size-of-the-binary-log/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-bin-compress-min-len</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `256`
- <strong>Range: `10` to `1024`</strong>
- <strong>Introduced:</strong> [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)

---

#### `log_bin_index`

- <strong>Description:</strong> File that holds the names for last binlog files.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-bin-index=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Introduced:</strong> [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)

---

#### `log_bin_trust_function_creators`

- <strong>Description:</strong> Functions and triggers can be dangerous when used with [replication](/kb/en/high-availability-performance-tuning-mariadb-replication/). Certain types of functions and triggers may have unintended consequences when the statements are applied on a replication slave. For that reason, there are some restrictions on the creation of functions and triggers when the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is enabled by default, such as:
<ul start="1"><li>When `log_bin_trust_function_creators` is `OFF` and <a undefined>log_bin</a> is `ON`, [CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function/) and [ALTER FUNCTION](/sql-statements-structure/sql-statements/data-definition/alter/alter-function/) statements will trigger an error if the function is defined with any of the `NOT DETERMINISTIC`, `CONTAINS SQL` or `MODIFIES SQL DATA` characteristics.
</li><li>This means that when `log_bin_trust_function_creators` is `OFF` and <a undefined>log_bin</a> is `ON`, [CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function/) and [ALTER FUNCTION](/sql-statements-structure/sql-statements/data-definition/alter/alter-function/) statements will only succeed if the function is defined with any of the `DETERMINISTIC`, `NO SQL`, or `READS SQL DATA` characteristics.
</li><li>When `log_bin_trust_function_creators` is `OFF` and <a undefined>log_bin</a> is `ON`, the <a undefined>SUPER</a> privilege is also required to execute the following statements:
<ul start="1"><li>[CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function/)
</li><li>[CREATE TRIGGER](/programming-customizing-mariadb/triggers-events/triggers/create-trigger/)
</li><li>[DROP TRIGGER](/sql-statements-structure/sql-statements/data-definition/drop/drop-trigger/)
</li></ul>
</li><li>Setting `log_bin_trust_function_creators` to `ON` removes these requirements around functions characteristics and the <a undefined>SUPER</a> privileges.
</li><li>See [Binary Logging of Stored Routines](/programming-customizing-mariadb/stored-routines/binary-logging-of-stored-routines/) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-bin-trust-function-creators[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `log_slow_slave_statements`

- <strong>Description:</strong> Log slow statements executed by slave thread to the [slow log](/mariadb-administration/server-monitoring-logs/slow-query-log/) if it is open. Before [MariaDB 10.1.13](/kb/en/mariadb-10113-release-notes/), this was only available as a mysqld option, not a server variable.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-slow-slave-statements</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong>
<ul start="1"><li>`ON ` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/))
</li><li>`OFF` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.1.13](/kb/en/mariadb-10113-release-notes/) (variable)

---

#### `log_slave_updates`

- <strong>Description:</strong> If set to `0`, the default, updates on a slave received from a master during [replication](/replication/) are not logged in the slave's binary log. If set to `1`, they are. The slave's binary log needs to be enabled for this to have an effect. Set to `1` if you want to daisy-chain the slaves.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-slave-updates</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `master_verify_checksum`

- <strong>Description:</strong> Verify [binlog checksums](/replication/standard-replication/binlog-event-checksums/) when reading events from the binlog on the master.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-verify-checksum=[0|1]</code>
- <strong>Scope:</strong> Global
- <strong>Access Type:</strong> Can be changed dynamically
- <strong>Data Type:</strong> `bool`
- <strong>Default Value:</strong> `OFF (0)`

---

#### `max_binlog_cache_size`

- <strong>Description:</strong> Restricts the size in bytes used to cache a multi-transactional query. If more bytes are required, a `Multi-statement transaction required more than 'max_binlog_cache_size' bytes of storage` error is generated. If the value is changed, current sessions are unaffected, only sessions started subsequently. See [max_binlog_stmt_cache_size](#max_binlog_stmt_cache_size) and [binlog_cache_size](#binlog_cache_size).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-binlog-cache-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `18446744073709547520`
- <strong>Range:</strong> `4096` to `18446744073709547520`

---

#### `max_binlog_size`

- <strong>Description:</strong> If the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) exceeds this size after a write, the server rotates it by closing it and opening a new binary log. Single transactions will always be stored in the same binary log, so the server will wait for open transactions to complete before rotating. This figure also applies to the size of [relay logs](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) if [max_relay_log_size](#max_relay_log_size) is set to zero.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-binlog-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1073741824` (1GB)
- <strong>Range:</strong> `4096 to 1073741824` (4KB to 1GB)

---

#### `max_binlog_stmt_cache_size`

- <strong>Description:</strong> Restricts the size used to cache non-transactional statements. See [max_binlog_cache_size](#max_binlog_cache_size) and [binlog_stmt_cache_size](#binlog_stmt_cache_size).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-binlog-stmt-cache-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `18446744073709547520` (64 bit), `4294963200` (32 bit)
- <strong>Range:</strong> `4096` to `18446744073709547520`

---

#### `max_relay_log_size`

- <strong>Description:</strong> Slave will rotate its [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) if it exceeds this size after a write. If set to 0, the [max_binlog_size](#max_binlog_size) setting is used instead.  Before [MariaDB 10.0](/kb/en/what-is-mariadb-100/), `max_binlog_size` was global only, but with the implementation of [multi-source replication](/replication/standard-replication/multi-source-replication/) in [MariaDB 10.0](/kb/en/what-is-mariadb-100/), it could be set per session as well.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-relay-log-size=#</code>
- <strong>Scope:</strong> Global, Session (from [MariaDB 10.0](/kb/en/what-is-mariadb-100/) only)
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0`, or `4096 to 1073741824` (4KB to 1GB)

---

#### `read_binlog_speed_limit`

- <strong>Description:</strong> Used to restrict the speed at which a [replication](/replication/) slave can read the binlog from the master. This can be used to reduce the load on a master if many slaves need to download large amounts of old binlog files at the same time. The network traffic will be restricted to the specified number of kilobytes per second.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--read-binlog-speed-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0` (no limit)
- <strong>Range:</strong> `0` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)

---

#### `relay_log`

- <strong>Description:</strong> [Relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) basename. If not set, the basename will be hostname-relay-bin.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--relay-log=file_name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `filename`
- <strong>Default Value:</strong> `''` (none)

---

#### `relay_log_basename`

- <strong>Description:</strong> The full path of the relay log file names, excluding the extension. Its value is derived from the [relay-log](#relay_log) variable value.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">No commandline option</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Read Only:</strong> `Yes`
- <strong>Introduced:</strong> [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)

---

#### `relay_log_index`

- <strong>Description:</strong> Name and location of the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) index file, the file that keeps a list of the last relay logs. Defaults to <em>hostname</em>-relay-bin.index.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--relay-log-index=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---

#### `relay_log_info_file`

- <strong>Description:</strong> Name and location of the file where the `RELAY_LOG_FILE` and `RELAY_LOG_POS` options (i.e. the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) position) for the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) statement  are written. The [slave's SQL thread](/kb/en/replication-threads/#slave-sql-thread) keeps this [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) position updated as it applies events.
<ul start="1"><li>See [CHANGE MASTER TO: Option Persistence](/kb/en/change-master-to/#option-persistence) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--relay-log-info-file=file_name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `relay-log.info`

---

#### `relay_log_purge`

- <strong>Description:</strong> If set to `1` (the default), [relay logs](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) will be purged as soon as they are no longer necessary.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--relay-log-purge={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Note:</strong> In MySQL and in MariaDB before version 10.0.8 this variable was silently changed if you did [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/).

---

#### `relay_log_recovery`

- <strong>Description:</strong> If set to `1` (`0` is default), on startup the slave will drop all [relay logs](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) that haven't yet been processed, and retrieve relay logs from the master. Can be useful after the slave has crashed to prevent the processing of corrupt relay logs. relay_log_recovery should always be set together with [relay_log_purge](#relay_log_purge). Setting <code class="fixed" style="white-space:pre-wrap">relay-log-recovery=1</code> with <code class="fixed" style="white-space:pre-wrap">relay-log-purge=0</code> can cause the relay log to be read from files that were not purged, leading to data inconsistencies.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--relay-log-recovery</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `relay_log_space_limit`

- <strong>Description:</strong> Specifies the maximum space to be used for the [relay logs](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/). The IO thread will stop until the SQL thread has cleared the backlog. By default `0`, or no limit.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--relay-log-space-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range - 32 bit:</strong> `0` to `4294967295`
- <strong>Range - 64 bit:</strong> `0` to `18446744073709547520`

---

#### `replicate_annotate_row_events`

- <strong>Description:</strong> Tells the slave to reproduce [annotate_rows_events](/clients-utilities/mysqlbinlog/annotate_rows_log_event/) received from the master in its own binary log. This option is sensible only when used in tandem with the [log_slave_updates](#log_slave_updates) option.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--replicate-annotate-row-events</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong>
<ul start="1"><li>`ON` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/))
</li><li>`OFF` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))
</li></ul>

---

#### `replicate_do_db`

- <strong>Description:</strong> This system variable allows you to configure a [replication slave](/replication/) to apply statements and transactions affecting databases that match a specified name.
<ul start="1"><li>This system variable will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](/kb/en/replication-filters/#statement-based-logging) section for more information.
</li><li>When setting it dynamically with <a undefined>SET GLOBAL</a>, the system variable accepts a comma-separated list of filters.
</li><li>When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), the system variable does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the system variable multiple times.
</li><li>See [Replication Filters](/replication/standard-replication/replication-filters/) for more information. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--replicate-do-db=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `''` (empty)

---

#### `replicate_do_table`

- <strong>Description:</strong> This system variable allows you to configure a [replication slave](/replication/) to apply statements and transactions that affect tables that match a specified name. The table name is specified in the format: `dbname.tablename`.
<ul start="1"><li>This system variable will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](/kb/en/replication-filters/#statement-based-logging) section for more information.
</li><li>When setting it dynamically with <a undefined>SET GLOBAL</a>, the system variable accepts a comma-separated list of filters.
</li><li>When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), the system variable does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the system variable multiple times.
</li><li>See [Replication Filters](/replication/standard-replication/replication-filters/) for more information. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--replicate-do-table=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `''` (empty)

---

#### `replicate_events_marked_for_skip`

- <strong>Description:</strong> Tells the slave whether to [replication](/replication/) events that are marked with the `@@skip_replication` flag. See [Selectively skipping replication of binlog events](/replication/standard-replication/selectively-skipping-replication-of-binlog-events/) for more information.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--replicate-events-marked-for-skip</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `replicate`
- <strong>Valid Values:</strong> `REPLICATE`, `FILTER_ON_SLAVE`, `FILTER_ON_MASTER`

---

#### `replicate_ignore_db`

- <strong>Description:</strong> This system variable allows you to configure a [replication slave](/replication/) to ignore statements and transactions affecting databases that match a specified name.
<ul start="1"><li>This system variable will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](/kb/en/replication-filters/#statement-based-logging) section for more information.
</li><li>When setting it dynamically with <a undefined>SET GLOBAL</a>, the system variable accepts a comma-separated list of filters.
</li><li>When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), the system variable does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the system variable multiple times.
</li><li>See [Replication Filters](/replication/standard-replication/replication-filters/) for more information. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--replicate-ignore-db=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `''` (empty)

---

#### `replicate_ignore_table`

- <strong>Description:</strong> This system variable allows you to configure a [replication slave](/replication/) to ignore statements and transactions that affect tables that match a specified name. The table name is specified in the format: `dbname.tablename`.
<ul start="1"><li>This system variable will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](/kb/en/replication-filters/#statement-based-logging) section for more information.
</li><li>When setting it dynamically with <a undefined>SET GLOBAL</a>, the system variable accepts a comma-separated list of filters.
</li><li>When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), the system variable does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the system variable multiple times.
</li><li>See [Replication Filters](/replication/standard-replication/replication-filters/) for more information. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--replicate-ignore-table=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `''` (empty)

---

#### `replicate_rewrite_db`

- <strong>Description:</strong> `replicate_rewrite_db` is not available as a system variable, only as a mysqld option. See [the description on that page](/kb/en/mysqld-options/#-replicate-rewrite-db).
<ul start="1"><li>See [Replication Filters](/replication/standard-replication/replication-filters/) for more information. 
</li></ul>

---

#### `replicate_wild_do_table`

- <strong>Description:</strong> This system variable allows you to configure a [replication slave](/replication/) to apply statements and transactions that affect tables that match a specified wildcard pattern. The wildcard pattern uses the same semantics as the [LIKE](/built-in-functions/string-functions/like/) operator.
<ul start="1"><li>This system variable will work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](/kb/en/replication-filters/#statement-based-logging) section for more information.
</li><li>When setting it dynamically with <a undefined>SET GLOBAL</a>, the system variable accepts a comma-separated list of filters.
</li><li>When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), the system variable does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the system variable multiple times.
</li><li>See [Replication Filters](/replication/standard-replication/replication-filters/) for more information. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--replicate-wild-do-table=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `''` (empty)

---

#### `replicate_wild_ignore_table`

- <strong>Description:</strong> This system variable allows you to configure a [replication slave](/replication/) to ignore statements and transactions that affect tables that match a specified wildcard pattern. The wildcard pattern uses the same semantics as the [LIKE](/built-in-functions/string-functions/like/) operator. 
<ul start="1"><li>This system variable will work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](/kb/en/replication-filters/#statement-based-logging) section for more information.
</li><li>When setting it dynamically with <a undefined>SET GLOBAL</a>, the system variable accepts a comma-separated list of filters.
</li><li>When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), the system variable does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the system variable multiple times.
</li><li>See [Replication Filters](/replication/standard-replication/replication-filters/) for more information. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--replicate-wild-ignore-table=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `''` (empty)

---

#### `report_host`

- <strong>Description:</strong> The host name or IP address the slave reports to the master when it registers. If left unset, the slave will not register itself. Reported by [SHOW SLAVE HOSTS](/kb/en/show-slave-hosts/). Note that it is not sufficient for the master to simply read the IP of the slave from the socket once the slave connects. Due to NAT and other routing issues, that IP may not be valid for connecting to the slave from the master or other hosts.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--report-host=host_name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---

#### `report_password`

- <strong>Description:</strong> Slave password reported to the master when it registers. Reported by [SHOW SLAVE HOSTS](/kb/en/show-slave-hosts/) if `--show-slave-auth-info` is set. This password has no connection with user privileges or with the [replication](/replication/) user account password.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--report-password=password</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---

#### `report_port`

- <strong>Description:</strong> The commandline option sets the TCP/IP port for connecting to the slave that will be reported to the [replicating](/replication/) master during the slave's registration. Viewing the variable will show this value.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--report-port=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `65535`

---

#### `report_user`

- <strong>Description:</strong> Slave's account user name reported to the master when it registers. Reported by [SHOW SLAVE HOSTS](/kb/en/show-slave-hosts/) if `--show-slave-auth-info` is set. This username has no connection with user privileges or with the [replication](/replication/) user account.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--report-user=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---

#### `server_id`

- <strong>Description:</strong> This system variable is used with [MariaDB replication](/replication/) to identify unique master and slave servers in a topology. This system variable is also used with the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) to determine which server a specific transaction originated on.
<ul start="1"><li>When [MariaDB replication](/replication/) is used with standalone MariaDB Server, each server in the replication topology must have a unique `server_id` value.
</li><li>When [MariaDB replication](/replication/) is used with [MariaDB Galera Cluster](/kb/en/galera/), see [Using MariaDB Replication with MariaDB Galera Cluster: Setting server_id on Cluster Nodes](/kb/en/using-mariadb-replication-with-mariadb-galera-cluster-using-mariadb-replica/#setting-server_id-on-cluster-nodes) for more information on how to set the `server_id` values.
</li><li>In [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) and below, the default `server_id` value is `0`. If a slave's `server_id` value is `0`, then all masters will refuse its connection attempts. If a master's `server_id` value is `0`, then it will refuse all slave connection attempts.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-id =#</code>
- <strong>Scope:</strong> Global, Session (&gt;= [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/) only - see [Global Transaction ID](/kb/en/global-transaction-id/#server_id))
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)), `0` (&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
- <strong>Range:</strong> `1` to `4294967295` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)), `0` to `4294967295` (&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))

---

#### `skip_parallel_replication`

- <strong>Description:</strong> If set when a transaction is written to the binlog, parallel apply of that transaction will be avoided on a slave where [slave_parallel_mode](#slave_parallel_mode) is not `aggressive`. Can be used to avoid unnecessary rollback and retry for transactions that are likely to cause a conflict if replicated in parallel. See [parallel replication](/replication/standard-replication/parallel-replication/).
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `skip_replication`

- <strong>Description:</strong>  Changes are logged into the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) with the @@skip_replication flag set. Such events will not be [replicated](/replication/) by slaves that run with <code class="fixed" style="white-space:pre-wrap">--replicate-events-marked-for-skip</code> set different from its default of `REPLICATE`. See [Selectively skipping replication of binlog events](/replication/standard-replication/selectively-skipping-replication-of-binlog-events/) for more information.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `slave_compressed_protocol`

- <strong>Description:</strong> If set to 1 (0 is the default), will use compression for the slave/master protocol if both master and slave support this.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-compressed-protocol</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `slave_ddl_exec_mode`

- <strong>Description:</strong>  Modes for how [replication](/replication/) of DDL events should be executed. Legal values are <code class="highlight fixed" style="white-space:pre-wrap">STRICT</code> and <code class="highlight fixed" style="white-space:pre-wrap">IDEMPOTENT</code> (default). In <code class="highlight fixed" style="white-space:pre-wrap">IDEMPOTENT</code> mode, the slave will not stop for failed DDL operations that would not cause a difference between the master and the slave. In particular [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) is treated as [CREATE OR REPLACE TABLE](/kb/en/create-table/#create-or-replace) and [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/) is treated as <code class="highlight fixed" style="white-space:pre-wrap">DROP TABLE IF EXISTS</code>.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-ddl-exec-mode=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `IDEMPOTENT`
- <strong>Valid Values:</strong> `IDEMPOTENT`, `STRICT`
- <strong>Introduced:</strong> [MariaDB 10.0.8](/kb/en/mariadb-1008-release-notes/)

---

#### `slave_domain_parallel_threads`

- <strong>Description:</strong> When set to a non-zero value, each [replication](/replication/) domain in one master connection can reserve at most that many worker threads at any one time, leaving the rest (up to the value of
[slave_parallel_threads](#slave_parallel_threads)) free for other master connections
or replication domains to use in parallel. See [Parallel Replication](/kb/en/parallel-replication/#configuration-variable-slave_domain_parallel_threads) for details.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-domain-parallel-threads=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Valid Values:</strong> `0` to `16383`
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)

---

#### `slave_exec_mode`

- <strong>Description:</strong> Determines the mode used for [replication](/replication/) error checking and conflict resolution. STRICT mode is the default, and catches all errors and conflicts. IDEMPOTENT mode suppresses duplicate key or no key errors, which can be useful in certain replication scenarios, such as when there are multiple masters, or circular replication.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `IDEMPOTENT` (NDB), `STRICT` (All)
- <strong>Valid Values:</strong> `IDEMPOTENT`, `STRICT`

---

#### `slave_load_tmpdir`

- <strong>Description:</strong> Directory where the replica stores temporary files for [replicating](/replication/) [LOAD DATA INFILE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/load-data-into-tables-or-index/load-data-infile/) statements. If not set, the replica will use [tmpdir](/kb/en/server-system-variables/#tmpdir). Should be set to a disk-based directory that will survive restarts, or else replication may fail.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-load-tmpdir=path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `file name`
- <strong>Default Value:</strong> `/tmp`

---

#### `slave_max_allowed_packet`

- <strong>Description:</strong> Maximum packet size in bytes for replica SQL and I/O threads. This value overrides [max_allowed_packet](#max_allowed_packet) for [replication](/replication/) purposes. Set in multiples of 1024 (the minimum) up to 1GB
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-max-allowed-packet=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1073741824`
- <strong>Range:</strong> `1024` to `1073741824`

---

#### `slave_net_timeout`

- <strong>Description:</strong> Time in seconds for the replica to wait for more data from the master before considering the connection broken, after which it will abort the read and attempt to reconnect. The retry interval is determined by the MASTER_CONNECT_RETRY open for the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) statement, while the maximum number of reconnection attempts is set by the [master-retry-count](/kb/en/mysqld-options/#-master-retry-count) option. The first reconnect attempt takes place immediately.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-net-timeout=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>
<ul start="1"><li>`60 (1 minute)` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/))
</li><li>`3600 (1 hour)` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))
</li></ul>
- <strong>Range:</strong> `1` to `31536000`

---

#### `slave_parallel_max_queued`

- <strong>Description:</strong> When [parallel_replication](/replication/standard-replication/parallel-replication/) is used, the [SQL thread](/kb/en/replication-threads/#slave-sql-thread) will read ahead in the relay logs, queueing events in memory while looking for opportunities for executing events in parallel. This system variable sets a
limit for how much memory it will use for this.
<ul start="1"><li>The configured value of this system variable is actually allocated for each [worker thread](/kb/en/replication-threads/#worker-threads), so the total allocation is actually equivalent to the following:
<ul start="1"><li><a undefined>slave_parallel_max_queued</a> * <a undefined>slave_parallel_threads</a>
</li></ul>
</li><li>This system variable is only meaningful when parallel
replication is configured (i.e. when <a undefined>slave_parallel_threads</a> &gt; `0`).
</li><li>See [Parallel Replication: Configuring the Maximum Size of the Parallel Slave Queue](/kb/en/parallel-replication/#configuring-the-maximum-size-of-the-parallel-slave-queue) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-parallel-max-queued=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `131072`
- <strong>Range:</strong> `0` to `2147483647`
- <strong>Introduced:</strong> [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)

---

#### `slave_parallel_mode`

- <strong>Description:</strong> Controls what transactions are applied in parallel when using [parallel replication](/replication/standard-replication/parallel-replication/). 
<ul start="1"><li>`optimistic`: tries to apply most transactional DML in parallel, and handles any conflicts with rollback and retry. See [optimistic mode](/kb/en/parallel-replication/#optimistic-mode-of-in-order-parallel-replication).
</li><li>`conservative`: limits parallelism in an effort to avoid any conflicts. See  [conservative mode](/kb/en/parallel-replication/#conservative-mode-of-in-order-parallel-replication).
</li><li>`aggressive`: tries to maximize the parallelism, possibly at the cost of increased conflict rate.
</li><li>`minimal`: only parallelizes the commit steps of transactions.
</li><li>`none` disables parallel apply completely.
</li></ul>
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `optimistic` (&gt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)), `conservative` (&lt;= [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/))
- <strong>Valid Values:</strong> `conservative`, `optimistic`, `none`, `aggressive` and `minimal`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `slave_parallel_threads`

- <strong>Description:</strong> This system variable is used to configure [parallel replication](/replication/standard-replication/parallel-replication/).
<ul start="1"><li>If this system variable is set to a value greater than `0`, then its value will determine how many slave [worker threads](/kb/en/replication-threads/#worker-threads) will be created to apply [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) events in parallel.
</li><li>If this system variable is set to `0` (which is the default value), then no slave [worker threads](/kb/en/replication-threads/#worker-threads) will be created. Instead, when replication is enabled, [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) events are applied by the slave's [SQL thread](/kb/en/replication-threads/#slave-sql-thread).
</li><li>The [slave threads](/kb/en/replication-threads/#threads-on-the-slave) must be [stopped](/kb/en/stop-slave/) in order to change this option's value dynamically.
</li><li>Events that were logged with [GTIDs](/replication/standard-replication/gtid/) with different <a undefined>gtid_domain_id</a> values can be applied in parallel in an [out-of-order](/kb/en/parallel-replication/#out-of-order-parallel-replication) manner. Each <a undefined>gtid_domain_id</a> can use the number of threads configured by <a undefined>slave_domain_parallel_threads</a>.
</li><li>Events that were [group-committed](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/) on the master can be applied in parallel in an [in-order](/kb/en/parallel-replication/#what-can-be-run-in-parallel) manner, and the specific behavior can be configured by setting <a undefined>slave_parallel_mode</a>. 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-parallel-threads=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `16383`
- <strong>Introduced:</strong> [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)

---

#### `slave_parallel_workers`

- <strong>Description:</strong> Alias for [slave_parallel_threads](#slave_parallel_threads).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-parallel-workers=#</code>
- <strong>Introduced:</strong> [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/)

---

#### `slave_run_triggers_for_rbr`

- <strong>Description:</strong> See [Running triggers on the slave for Row-based events](/replication/standard-replication/running-triggers-on-the-slave-for-row-based-events/) for a description and use-case for this setting.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-run-triggers-for-rbr=value</code>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `NO`
- <strong>Valid Values:</strong> `NO`, `YES`, `LOGGING`, or `ENFORCE` (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/))
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `slave_skip_errors`

- <strong>Description:</strong> When an error occurs on the slave, [replication](/replication/) usually halts. This option permits a list of [error codes](/sql-statements-structure/sql-language-structure/mariadb-error-codes/) to ignore, and for which replication will continue. This option should never be needed in normal use, and careless use could lead to slaves that are out of sync with masters. Error codes are in the format of the number from the slave error log. Using `all` as an option permits the slave the keep replicating no matter what error it encounters, an option you would never normally need in production  and which could rapidly lead to data inconsistencies. A count of these is kept  in [slave_skipped_errors](/kb/en/replication-and-binary-log-status-variables/#slave_skipped_errors).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-skip-errors=[error_code1,error_code2,...|all|ddl_exist_errors]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `OFF`
- <strong>Valid Values:</strong> `[list of error codes]`, `ALL`, `OFF`

---

#### `slave_sql_verify_checksum`

- <strong>Description:</strong> Verify [binlog checksums](/replication/standard-replication/binlog-event-checksums/) when the slave SQL thread reads events from the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-sql-verify-checksum=[0|1]</code>
- <strong>Scope:</strong> Global
- <strong>Access Type:</strong> Can be changed dynamically
- <strong>Data Type:</strong> `bool`
- <strong>Default Value:</strong> `ON (1)`

---

#### `slave_transaction_retries`

- <strong>Description:</strong> Number of times a [replication](/replication/) slave retries to execute an SQL thread after it fails due to InnDB deadlock or by exceeding the transaction execution time limit. If after this number of tries the SQL thread has still failed to execute, the slave will stop with an error. See also the [innodb_lock_wait_timeout](/kb/en/xtradbinnodb-server-system-variables/#innodb_lock_wait_timeout) system variable.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-transaction-retries=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range - 32 bit:</strong> `0` to `4294967295`
- <strong>Range - 64 bit:</strong> `0` to `18446744073709547520`

---

#### `slave_transaction_retry_errors`

- <strong>Description:</strong> When an error occurs during a transaction on the slave, [replication](/replication/) usually halts. By default, transactions that caused a deadlock or elapsed lock wait timeout will be retried. One can add other errors to the the list of errors that should be retried by adding a comma-separated list of [error numbers](/sql-statements-structure/sql-language-structure/mariadb-error-codes/) to this variable. This is particularly useful in some [Spider](/columns-storage-engines-and-plugins/storage-engines/spider/) setups. Some recommended errors to retry for Spider are 1158,1159,1160,1161,1429,2013,12701.(From [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/), these are in the default value)
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-transaction_retry-errors=[error_code1,error_code2,...]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> 
<ul><li>`1158,1159,1160,1161,1205,1213,1429,2013,12701` (&gt;= [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/))
</li><li>`1213,1205` (&gt;= [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/))
</li></ul>
- <strong>Valid Values:</strong> `comma-separated list of error codes`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `slave_transaction_retry_interval`

- <strong>Description:</strong> Interval in seconds for the slave SQL thread to retry a failed transaction due to a deadlock, elapsed lock wait timeout or an error listed in [slave_transaction_retry_errors](#slave_transaction_retry_errors). The interval is calculated as `max(slave_transaction_retry_interval, min(retry_count, 5))`.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-transaction-retry-interval=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `3600`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `slave_type_conversions`

- <strong>Description:</strong> Determines the type conversion mode on the slave when using [row-based](/kb/en/binary-log-formats/#row-based) [replication](/replication/), including replications in MariaDB Galera cluster. Multiple options can be set, delimited by commas. If left empty, the default, type conversions are disallowed. The variable is dynamic and a change in its value takes effect immediately. This variable tells the server what to do if the table definition is different between the master and slave (for example a column is 'int' on the master and 'bigint' on the slave). 
<ul start="1"><li>`ALL_NON_LOSSY` means that all safe conversions (no data loss) are allowed. 
</li><li>`ALL_LOSSY` means that all lossy conversions are allowed (for example 'bigint' to 'int'). This, however, does not imply that safe conversions (non-lossy) are allowed as well. In order to allow all conversions, one needs to allow both lossy as well as non-lossy conversions by setting this variable to ALL_NON_LOSSY,ALL_LOSSY.
</li><li>Empty (default) means that the server should give an error and replication should stop if the table definition is different between the master and slave.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave-type-conversions=set</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `set`
- <strong>Default Value:</strong> `Empty variable`
- <strong>Valid Values:</strong> `ALL_LOSSY`, `ALL_NON_LOSSY`, empty

---

#### `sql_log_bin`

- <strong>Description:</strong> If set to 0 (1 is the default), no logging to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is done for the client. Only clients with the SUPER privilege can update this variable. Can have unintended consequences if set globally, see [SET SQL_LOG_BIN](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set-sql_log_bin/). From [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/), this variable does not affect the replication of events in a Galera cluster.
- <strong>Scope:</strong> Global (before [MariaDB 10.0.16](/kb/en/mariadb-10016-release-notes/) and [MariaDB 5.5.41](/kb/en/mariadb-5541-release-notes/) only), Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`

---

#### `sql_slave_skip_counter`

- <strong>Description:</strong> Number of events that a slave skips from the master. If this would cause the slave to begin in the middle of an event group, the slave will instead begin from the beginning of the next event group. See [SET GLOBAL sql_slave_skip_counter](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/set-global-sql_slave_skip_counter/).
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`

---

#### `sync_binlog`

- <strong>Description:</strong> MariaDB will synchronize its binary log file to disk after this many events. The default is 0, in which case the operating system handles flushing the file to disk. 1 is the safest, but slowest, choice, since the file is flushed after each write. If autocommit is enabled, there is one write per statement, otherwise there's one write per transaction. If the disk has cache backed by battery, synchronization will be fast and a more conservative number can be chosen.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sync-binlog=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `sync_master_info`

- <strong>Description:</strong> A [replication](/replication/) slave will synchronize its master.info file to disk after this many events. If set to 0, the operating system handles flushing the file to disk.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sync-master-info=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10000` (&gt;= [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)), `0` (&lt;= [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/))

---

#### `sync_relay_log`

- <strong>Description:</strong> The MariaDB server will synchronize its [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) to disk after this many writes to the log. The default until [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/) was 0, in which case the operating system handles flushing the file to disk. 1 is the safest, but slowest, choice, since the file is flushed after each write. If autocommit is enabled, there is one write per statement, otherwise there's one write per transaction. If the disk has cache backed by battery, synchronization will be fast and a more conservative number can be chosen.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sync-relay-log=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10000` (&gt;= [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)), `0` (&lt;= [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/))

---

#### `sync_relay_log_info`

- <strong>Description:</strong> A [replication](/replication/) slave will synchronize its relay-log.info file to disk after this many transactions. The default until [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/) was 0, in which case the operating system handles flushing the file to disk. 1 is the most secure choice, because at most one event could be lost in the event of a crash, but it's also the slowest.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sync-relay-log-info=#</code>
- <strong>Scope:</strong> Global,
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10000` (&gt;= [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)), `0` (&lt;= [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/))
- <strong>Range:</strong> `0` to `4294967295`

---