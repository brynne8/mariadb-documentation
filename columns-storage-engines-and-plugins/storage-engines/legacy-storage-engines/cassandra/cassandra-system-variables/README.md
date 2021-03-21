# Cassandra System Variables

CassandraSE is no longer actively being developed and has been removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/). See [MDEV-23024](https://jira.mariadb.org/browse/MDEV-23024).

This page documents system variables related to the [Cassandra storage engine](/columns-storage-engines-and-plugins/storage-engines/legacy-storage-engines/cassandra/). See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting them.

#### `cassandra_default_thrift_host`

- <strong>Description:</strong> Host to connect to, if not specified on per-table basis.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`

---

#### `cassandra_failure_retries`

- <strong>Description:</strong> Number of times to retry on timeout/unavailable failures.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `3`
- <strong>Valid Values:</strong> `1` to `1073741824`

---

#### `cassandra_insert_batch_size`

- <strong>Description:</strong> INSERT batch size.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100`
- <strong>Valid Values:</strong> `1` to `1073741824`

---

#### `cassandra_multiget_batch_size`

- <strong>Description:</strong> Batched Key Access batch size.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100`
- <strong>Valid Values:</strong> `1` to `1073741824`

---

#### `cassandra_read_consistency`

- <strong>Description:</strong> Consistency to use for reading. See [Datastax's documentation](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.
- <strong>Scope:</strong> Global, Session
- <strong>Default Value:</strong> `ONE`
- <strong>Valid Values:</strong> `ONE`, `TWO`, `THREE`, `ANY`, `ALL`, `QUORUM`, `EACH_QUORUM`, `LOCAL_QUORUM`, <code>
</code>

---

#### `cassandra_rnd_batch_size`

- <strong>Description:</strong> Full table scan batch size.
- <strong>Scope:</strong> Global, Session
- <strong>Default Value:</strong> `10000`
- <strong>Valid Values:</strong> `1` to `1073741824`

---

#### `cassandra_write_consistency`

- <strong>Description:</strong> Consistency to use for writing. See [Datastax's documentation](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.
- <strong>Scope:</strong> Global, Session
- <strong>Default Value:</strong> `ONE`
- <strong>Valid Values:</strong> `ONE`, `TWO`, `THREE`, `ANY`, `ALL`, `QUORUM`, `EACH_QUORUM`, `LOCAL_QUORUM`, <code>
</code>

---