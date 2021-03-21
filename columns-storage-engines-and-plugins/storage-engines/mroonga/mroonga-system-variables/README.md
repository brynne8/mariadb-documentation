# Mroonga System Variables

This page documents system variables related to the [Mroonga storage engine](/columns-storage-engines-and-plugins/storage-engines/mroonga/). See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting them.

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `mroonga_action_on_fulltext_query_error`

- <strong>Description:</strong> Action to take when encountering a Mroonga fulltext error.
<ul><li>`ERROR`: Report an error without logging.
</li><li>`ERROR_AND_LOG`: Report an error with logging (the default)
</li><li>`IGNORE`: No logging or reporting - the error is ignored.
</li><li>`IGNORE_AND_LOG`: Log the error without reporting it.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-action-on-fulltext-query-error=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `ERROR_AND_LOG`

---

#### `mroonga_boolean_mode_syntax_flags`

- <strong>Description:</strong> Flags to customize syntax in BOOLEAN MODE searches. Available flags: 
<ul><li>`DEFAULT`: (=SYNTAX_QUERY,ALLOW_LEADING_NOT)
</li><li>`ALLOW_COLUMN`: Allows `COLUMN:...` syntax in query syntax, an incompatible change to the regular BOOLEAN MODE syntax. Permits multiple indexes in one `MATCH () AGAINST ()`. Can be used in other operations besides full-text search, such as equal, and prefix search. See [Groonga query syntax](http://groonga.org/docs/reference/grn_expr/query_syntax.html) for more details.
</li><li>`ALLOW_LEADING_NOT` Permits using the `NOT_INCLUDED_KEYWORD` syntax in the query syntax.
</li><li>`ALLOW_UPDATE`: Permits updating values with the `COLUMN:=NEW_VALUE` syntax in the query syntax.
</li><li>`SYNTAX_QUERY`: Mroonga will use Groonga's query syntax, compatible with MariaDB's BOOLEAN MODE syntax. Unless  `SYNTAX_SCRIPT` is specified, this mode is always in use.
</li><li>`SYNTAX_SCRIPT`: Mroonga will use Groonga's script syntax, a JavaScript-like syntax. If both `SYNTAX_QUERY` and `SYNTAX_SCRIPT` are specified, `SYNTAX_SCRIPT` will take precedence..
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-boolean-mode-syntax-flags=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `DEFAULT`

---

#### `mroonga_database_path_prefix`

- <strong>Description:</strong> The database path prefix.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-database-path-prefix=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty)

---

#### `mroonga_default_parser`

- <strong>Description:</strong> The fulltext default parser, for example `TokenBigramSplitSymbolAlphaDigit` or `TokenBigram` (the default). See the list of options at [Mroonga Overview:Parser](/kb/en/mroonga-overview/#parser). Deprecated since Mroonga 5.04, use [mroonga_default_tokenizer](#mroonga_default_tokenizer) instead.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-default-parser=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `TokenBigram`
- <strong>Deprecated:</strong> [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/), Mroonga 5.0.4

---

#### `mroonga_default_tokenizer`

- <strong>Description:</strong> The fulltext default parser, for example `TokenBigramSplitSymbolAlphaDigit` or `TokenBigram` (the default). See the list of options at [Mroonga Overview:Parser](/kb/en/mroonga-overview/#parser).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-default-tokenizer=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `TokenBigram`
- <strong>Introduced:</strong> [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/), Mroonga 5.0.4

---

#### `mroonga_default_wrapper_engine`

- <strong>Description:</strong> The default engine for wrapper mode.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-default-wrapper-engine=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty)

---

#### `mroonga_dry_write`

- <strong>Description:</strong> If set to `on`, (`off` is default), data is not actually written to the Groonga database. Only really useful to change for benchmarking.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-dry-write[={0|1}]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `off`

---

#### `mroonga_enable_operations_recording`

- <strong>Description:</strong> Whether recording operations for recovery to the Groonga database is enabled (default) or not. Requires reopening the database with [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) after changing the variable.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-enable-operations-recording={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.2.11](/kb/en/mariadb-10211-release-notes/), [MariaDB 10.1.29](/kb/en/mariadb-10129-release-notes/)

---

#### `mroonga_enable_optimization`

- <strong>Description:</strong> If set to `on` (the default), optimization is enabled. Only really useful to change for benchmarking.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-enable-optimization={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `on`

---

#### `mroonga_libgroonga_embedded`

- <strong>Description:</strong> Whether libgroonga is embedded or not.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)

---

#### `mroonga_libgroonga_support_lz4`

- <strong>Description:</strong> Whether libgroonga supports lz4 or not.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0.17](/kb/en/mariadb-10017-release-notes/)

---

#### `mroonga_libgroonga_support_zlib`

- <strong>Description:</strong> Whether libgroonga supports zlib or not.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `mroonga_libgroonga_support_zstd`

- <strong>Description:</strong> Whether libgroonga supports Zstandard or not.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.2.11](/kb/en/mariadb-10211-release-notes/), [MariaDB 10.1.29](/kb/en/mariadb-10129-release-notes/)

---

#### `mroonga_libgroonga_version`

- <strong>Description:</strong> Groonga library version.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---

#### `mroonga_lock_timeout`

- <strong>Description:</strong> Lock timeout used in Groonga.
- <strong>Commandline:</strong> <code class="unknown_macro">&lt;&lt;<span class="macro_name">code</span><span class="macro_arg_string"></span>&gt;&gt;</code>--mroonga-lock-timeout=#&lt;/code&gt;&gt;
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `900000`
- <strong>Range:</strong> `-1` to `2147483647`

---

#### `mroonga_log_file`

- <strong>Description:</strong> Name and path of the Mroonga log file.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-log-file=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `groonga.log`

---

#### `mroonga_log_level`

- <strong>Description:</strong> Mroonga log file output level, which determines what is logged. Valid levels include:
<ul><li>`NONE` 	No output.
</li><li>`EMERG`: Only emergency error messages, such as database corruption.
</li><li>`ALERT`: Alert messages, such as internal errors.
</li><li>`CRIT `: Critical error messages, such as deadlocks.
</li><li>`ERROR `: Errors, such as API errors.
</li><li>`WARNING`: Warnings, such as invalid arguments.
</li><li>`NOTICE`: Notices, such as a change in configuration or a status change.
</li><li>`INFO`: Information messages, such as file system operations.
</li><li>`DEBUG`: Debug messages, suggested for developers or testers.
</li><li>`DUMP`: Dump messages.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-log-level=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `NOTICE`

---

#### `mroonga_match_escalation_threshold`

- <strong>Description:</strong> The threshold to determine whether the match method is escalated. `-1` means never escalate.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-match-escalation-threshold=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `-1` to `9223372036854775807`

---

#### `mroonga_max_n_records_for_estimate`

- <strong>Description:</strong> The max number of records to estimate the number of matched records
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-max-n-records-for-estimate=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1000`
- <strong>Range:</strong> `-1` to `2147483647`
- <strong>Introduced:</strong> [MariaDB 10.0.18](/kb/en/mariadb-10018-release-notes/), Mroonga 5.0.2

---

#### `mroonga_query_log_file`

- <strong>Description:</strong> Query log file for Mroonga.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-query-log-file=filename</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (Empty string)
- <strong>Introduced:</strong> [MariaDB 10.2.11](/kb/en/mariadb-10211-release-notes/), [MariaDB 10.1.29](/kb/en/mariadb-10129-release-notes/)

---

#### `mroonga_vector_column_delimiter`

- <strong>Description:</strong> Delimiter to use when outputting a vector column. The default is a white space.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mroonga-vector-column-delimiter=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> ` ` (white space)

---

#### `mroonga_version`

- <strong>Description:</strong> Mroonga version
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---