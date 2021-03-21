# Mroonga Status Variables

This page documents status variables related to the [Mroonga storage engine](/columns-storage-engines-and-plugins/storage-engines/mroonga). See [Server Status Variables](/replication/optimization-and-tuning/system-variables/server-status-variables) for a complete list of status variables that can be viewed with [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status).

#### `Mroonga_count_skip`

- <strong>Description:</strong> Incremented each time the 'fast line count feature' is used. Can be used to check if the feature is working after enabling it.
- <strong>Data Type:</strong> `numeric`

---

#### `Mroonga_fast_order_limit`

- <strong>Description:</strong> Incremented each time the 'fast ORDER BY LIMIT feature' is used. Can be used to check if the feature is working after enabling it.
- <strong>Data Type:</strong> `numeric`

---