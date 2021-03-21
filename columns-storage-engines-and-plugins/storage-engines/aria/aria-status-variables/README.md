# Aria Status Variables

This page documents status variables related to the [Aria storage engine](/columns-storage-engines-and-plugins/storage-engines/aria/). See [Server Status Variables](/replication/optimization-and-tuning/system-variables/server-status-variables/) for a complete list of status variables that can be viewed with [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status/).

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `Aria_pagecache_blocks_not_flushed`

- <strong>Description:</strong> The number of dirty blocks in the Aria page cache. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Aria_pagecache_blocks_unused`

- <strong>Description:</strong> Free blocks in the Aria page cache. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Aria_pagecache_blocks_used`

- <strong>Description:</strong> Blocks used in the Aria page cache. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Aria_pagecache_read_requests`

- <strong>Description:</strong> The number of requests to read something from the Aria page cache.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Aria_pagecache_reads`

- <strong>Description:</strong> The number of Aria page cache read requests that caused a block to be read from the disk.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Aria_pagecache_write_requests`

- <strong>Description:</strong> The number of requests to write a block to the Aria page cache.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Aria_pagecache_writes`

- <strong>Description:</strong> The number of blocks written to disk from the Aria page cache.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Aria_transaction_log_syncs`

- <strong>Description:</strong> The number of Aria log fsyncs.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---