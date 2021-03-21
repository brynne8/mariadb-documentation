# Performance Schema System Variables

The following variables are used with MariaDB's [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/). See [Performance Schema Options](/kb/en/mysqld-options/#performance-schema-options) for Performance Schema options that are not system variables. See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting them.

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `performance_schema`

- <strong>Description:</strong> If set to `1` (`0` is default), enables the Performance Schema
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `performance_schema_accounts_size`

- <strong>Description:</strong> Maximum number of rows in the [performance_schema.accounts](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-accounts-table/) table. If set to 0, the [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/) will not store statistics in the accounts table. Use `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-accounts-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`

---

#### `performance_schema_digests_size`

- <strong>Description:</strong> Maximum number of rows that can be stored in the [events_statements_summary_by_digest](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_statements_summary_by_digest-table/) table. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-digests-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `200`

---

#### `performance_schema_events_stages_history_long_size`

- <strong>Description:</strong> Number of rows in the [events_stages_history_long](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_stages_history_long-table/) table.  `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-events-stages-history-long-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`

---

#### `performance_schema_events_stages_history_size`

- <strong>Description:</strong> Number of rows per thread in the [events_stages_history](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_stages_history-table/) table. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-events-stages-history-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1024`

---

#### `performance_schema_events_statements_history_long_size`

- <strong>Description:</strong> Number of rows in the [events_statements_history_long](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_statements_history_long-table/) table. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-events-statements-history-long-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`

---

#### `performance_schema_events_statements_history_size`

- <strong>Description:</strong> Number of rows per thread in the [events_statements_history](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_statements_history-table/) table. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-events-statements-history-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1024`

---

#### `performance_schema_events_transactions_history_long_size`

- <strong>Description:</strong> Number of rows in [events_transactions_history_long](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_transactions_history_long-table/) table. Use `0` to disable, `-1` for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-events-transactions-history-long-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `performance_schema_events_transactions_history_size`

- <strong>Description:</strong>Number of rows per thread in [events_transactions_history](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_transactions_history-table/). Use 0 to disable, -1 for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-events-transactions-history-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1024`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `performance_schema_events_waits_history_long_size`

- <strong>Description:</strong> Number of rows contained in the [events_waits_history_long](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_waits_history_long-table/) table. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-events-waits-history-long-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`

---

#### `performance_schema_events_waits_history_size`

- <strong>Description:</strong> Number of rows per thread contained in the [events_waits_history](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_waits_history-table/) table. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-events-waits-history-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1024`

---

#### `performance_schema_hosts_size`

- <strong>Description:</strong> Number of rows stored in the [hosts](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-hosts-table/) table. If set to zero, no connection statistics are kept for the hosts table. `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-hosts-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`

---

#### `performance_schema_max_cond_classes`

- <strong>Description:</strong> Specifies the maximum number of condition instruments.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-cond-classes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `90` (&gt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)), `80` (&lt;= [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/))
- <strong>Range:</strong> `0` to `256`

---

#### `performance_schema_max_cond_instances`

- <strong>Description:</strong> Specifies the maximum number of instrumented condition objects. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-cond-instances=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`

---

#### `performance_schema_max_digest_length`

- <strong>Description:</strong> Maximum length considered for digest text, when stored in performance_schema tables.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-digest-length=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1024`
- <strong>Range:</strong> `0` to `1048576`

---

#### `performance_schema_max_file_classes`

- <strong>Description:</strong> Specifies the maximum number of file instruments.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-file-classes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>`80` (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)), `50` (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/))
- <strong>Range:</strong> `0` to `256`

---

#### `performance_schema_max_file_handles`

- <strong>Description:</strong> Specifies the maximum number of opened file objects. Should always be higher than [open_files_limit](/kb/en/server-system-variables/#open_files_limit).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-file-handles=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `32768`
- <strong>Range:</strong> `-1` to `32768`

---

#### `performance_schema_max_file_instances`

- <strong>Description:</strong> Specifies the maximum number of instrumented file objects. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-file-instances=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`

---

#### `performance_schema_max_index_stat`

- <strong>Description:</strong> Maximum number of index statistics for instrumented tables. Use 0 to disable, -1 for automated scaling.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-index-stat=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `performance_schema_max_memory_classes`

- <strong>Description:</strong> Maximum number of memory pool instruments.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-memory-classes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `320`
- <strong>Range:</strong> `0` to `1024`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `performance_schema_max_metadata_locks`

- <strong>Description:</strong> Maximum number of [Performance Schema metadata locks](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-metadata_locks-table/). Use 0 to disable, -1 for automated scaling.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-metadata-locks=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `104857600`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `performance_schema_max_mutex_classes`

- <strong>Description:</strong> Specifies the maximum number of mutex instruments.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-mutex-classes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `210` (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)), `200` (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/))
- <strong>Range:</strong> `0` to `256`

---

#### `performance_schema_max_mutex_instances`

- <strong>Description:</strong> Specifies the maximum number of instrumented mutex instances. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-mutex-instances=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>`-1`
- <strong>Range:</strong> `-1` to `104857600`

---

#### `performance_schema_max_prepared_statement_instances`

- <strong>Description:</strong> Maximum number of instrumented prepared statements. Use 0 to disable, -1 for automated scaling.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-prepared-statement-instances=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `performance_schema_max_program_instances`

- <strong>Description:</strong> Maximum number of instrumented programs. Use 0 to disable, -1 for automated scaling.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-program-instances=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `performance_schema_max_rwlock_classes`

- <strong>Description:</strong> Specifies the maximum number of rwlock instruments.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-rwlock-classes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `50` (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)), `40` (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/))
- <strong>Range:</strong> `0` to `256`

---

#### `performance_schema_max_rwlock_instances`

- <strong>Description:</strong> Specifies the maximum number of instrumented rwlock objects. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-rwlock-instances=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>`-1`
- <strong>Range:</strong> `-1` to `104857600`

---

#### `performance_schema_max_socket_classes`

- <strong>Description:</strong> Specifies the maximum number of socket instruments.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-socket-classes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `0` to `256`

---

#### `performance_schema_max_socket_instances`

- <strong>Description:</strong> Specifies the maximum number of instrumented socket objects. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-socket-instances=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>`-1`
- <strong>Range:</strong> `-1` to `1048576`

---

#### `performance_schema_max_sql_text_length`

- <strong>Description:</strong> Maximum length of displayed sql text.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-sql-text-length=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1024`
- <strong>Range:</strong> `0` to `1048576`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `performance_schema_max_stage_classes`

- <strong>Description:</strong> Specifies the maximum number of stage instruments.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-stage-classes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `160` (&gt;= [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)), `150` (&lt;= [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/))
- <strong>Range:</strong> `0` to `256`

---

#### `performance_schema_max_statement_classes`

- <strong>Description:</strong> Specifies the maximum number of statement instruments. Automatically calculated at server build based on the number of available statements. Should be left as either autosized or disabled, as changing to any positive value has no benefit and will most likely allocate unnecessary memory. Setting to zero disables all statement instrumentation, and no memory will be allocated for this purpose.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-statement-classes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> Autosized (see description)
- <strong>Range:</strong> `0` to `256`

---

#### `performance_schema_max_statement_stack`

- <strong>Description:</strong> Number of rows per thread in EVENTS_STATEMENTS_CURRENT.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-statement-stack=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `1` to `256`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `performance_schema_max_table_handles`

- <strong>Description:</strong> Specifies the maximum number of opened table objects. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-table-handles=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`

---

#### `performance_schema_max_table_instances`

- <strong>Description:</strong> Specifies the maximum number of instrumented table objects. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-table-instances=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>`-1`
- <strong>Range:</strong> `-1` to `1048576`

---

#### `performance_schema_max_table_lock_stat`

- <strong>Description:</strong> Maximum number of lock statistics for instrumented tables. Use 0 to disable, -1 for automated scaling.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-table-lock-stat=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `performance_schema_max_thread_classes`

- <strong>Description:</strong> Specifies the maximum number of thread instruments.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-thread-classes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `50`
- <strong>Range:</strong> `0` to `256`

---

#### `performance_schema_max_thread_instances`

- <strong>Description:</strong> Specifies how many of the running server threads (see [max_connections](/kb/en/server-system-variables/#max_connections) and [max_delayed_threads](/kb/en/server-system-variables/#max_delayed_threads)) can be instrumented. Should be greater than the sum of max_connections and max_delayed_threads. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-max-thread-instances=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>`-1`
- <strong>Range:</strong> `-1` to `1048576`

---

#### `performance_schema_session_connect_attrs_size`

- <strong>Description:</strong> Per thread preallocated memory for holding connection attribute strings. Incremented if the strings are larger than the reserved space. `0` for disabling, `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-session-connect-attrs-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`

---

#### `performance_schema_setup_actors_size`

- <strong>Description:</strong> The maximum number of rows to store in the performance schema [setup_actors](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-setup_actors-table/) table. `-1` (from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)) denotes automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-setup-actors-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1` (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)), `100` (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/))
- <strong>Range:</strong> `-1` to `1024` (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)), `0` to `1024` (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/))

---

#### `performance_schema_setup_objects_size`

- <strong>Description:</strong> The maximum number of rows that can be stored in the performance schema [setup_objects](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-setup_objects-table/) table. `-1` (from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)) denotes automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-setup-objects-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1` (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)), `100` (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/))
- <strong>Range:</strong> `-1` to `1048576` (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)), `0` to `1048576` (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/))

---

#### `performance_schema_users_size`

- <strong>Description:</strong> Number of rows in the [performance_schema.users](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-users-table/) table. If set to 0, the [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/) will not store connection statistics in the users table. `-1` (the default) for automated sizing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-users-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `-1`
- <strong>Range:</strong> `-1` to `1048576`

---