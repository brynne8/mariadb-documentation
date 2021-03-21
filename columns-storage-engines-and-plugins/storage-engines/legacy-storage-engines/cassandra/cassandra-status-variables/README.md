# Cassandra Status Variables

CassandraSE is no longer actively being developed and has been removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/). See [MDEV-23024](https://jira.mariadb.org/browse/MDEV-23024).

This page documents status variables related to the [Cassandra storage engine](/columns-storage-engines-and-plugins/storage-engines/legacy-storage-engines/cassandra/). See [Server Status Variables](/replication/optimization-and-tuning/system-variables/server-status-variables/) for a complete list of status variables that can be viewed with [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status/).

#### `Cassandra_multiget_keys_scanned`

- <strong>Description:</strong> Number of keys we've made lookups for.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Cassandra_multiget_reads`

- <strong>Description:</strong> Number of read operations.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Cassandra_multiget_rows_read`

- <strong>Description:</strong> Number of rows actually read.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Cassandra_network_exceptions`

- <strong>Description:</strong> Number of network exceptions.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> <a undefined>MariaDB 10.0.3</a>

---

#### `Cassandra_row_insert_batches`

- <strong>Description:</strong> Number of insert batches performed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Cassandra_row_inserts`

- <strong>Description:</strong> Number of rows inserted.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Cassandra_timeout_exceptions`

- <strong>Description:</strong> Number of Timeout exceptions we got from Cassandra.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Cassandra_unavailable_exceptions`

- <strong>Description:</strong> Number of Unavailable exceptions we got from Cassandra.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---