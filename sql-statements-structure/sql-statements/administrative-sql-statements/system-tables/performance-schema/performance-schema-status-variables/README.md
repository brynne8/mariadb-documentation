# Performance Schema Status Variables

This page documents status variables related to the [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/). See [Server Status Variables](/replication/optimization-and-tuning/system-variables/server-status-variables/) for a complete list of status variables that can be viewed with [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status/).

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `Performance_schema_accounts_lost`

- <strong>Description:</strong> Number of times a row could not be added to the performance schema accounts table due to it being full. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_cond_classes_lost`

- <strong>Description:</strong> Number of condition instruments that could not be loaded.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_cond_instances_lost`

- <strong>Description:</strong> Number of instances a condition object could not be created. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_digest_lost`

- <strong>Description:</strong> The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_file_classes_lost`

- <strong>Description:</strong> Number of file instruments that could not be loaded.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_file_handles_lost`

- <strong>Description:</strong> Number of instances a file object could not be opened. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_file_instances_lost`

- <strong>Description:</strong> Number of instances a file object could not be created. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_hosts_lost`

- <strong>Description:</strong> Number of times a row could not be added to the performance schema hosts table due to it being full. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_index_stat_lost`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Performance_schema_locker_lost`

- <strong>Description:</strong>  Number of events not recorded, due to either being recursive, or having a deeper nested events stack than the implementation limit. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_memory_classes_lost`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Performance_schema_metadata_lock_lost`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Performance_schema_mutex_classes_lost`

- <strong>Description:</strong> Number of mutual exclusion instruments that could not be loaded.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_mutex_instances_lost`

- <strong>Description:</strong> Number of instances a mutual exclusion object could not be created. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_nested_statement_lost`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Performance_schema_prepared_statements_lost`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Performance_schema_program_lost`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Performance_schema_rwlock_classes_lost`

- <strong>Description:</strong> Number of read/write lock instruments that could not be loaded.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_rwlock_instances_lost`

- <strong>Description:</strong> Number of instances a read/write lock object could not be created. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_session_connect_attrs_lost`

- <strong>Description:</strong> Number of connections for which connection attribute truncation has occurred. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_socket_classes_lost`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_socket_instances_lost`

- <strong>Description:</strong> Number of instances a socket object could not be created. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_stage_classes_lost`

- <strong>Description:</strong> Number of stage event instruments that could not be loaded. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_statement_classes_lost`

- <strong>Description:</strong> Number of statement instruments that could not be loaded. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_table_handles_lost`

- <strong>Description:</strong> Number of instances a table object could not be opened. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_table_instances_lost`

- <strong>Description:</strong> Number of instances a table object could not be created. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_table_lock_stat_lost`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `Performance_schema_thread_classes_lost`

- <strong>Description:</strong> Number of thread instruments that could not be loaded.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_thread_instances_lost`

- <strong>Description:</strong> Number of instances thread object could not be created. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Performance_schema_users_lost`

- <strong>Description:</strong> Number of times a row could not be added to the performance schema users table due to it being full. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---