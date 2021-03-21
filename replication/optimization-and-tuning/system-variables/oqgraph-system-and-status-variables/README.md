# OQGRAPH System and Status Variables

This page documents system and status variables related to the [OQGRAPH storage engine](/kb/en/oqgraph/). See [Server Status Variables](/replication/optimization-and-tuning/system-variables/server-status-variables/) and [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for complete list of all system and status variables.

## System Variables

#### `oqgraph_allow_create_integer_latch`

- <strong>Description:</strong> Created when the [OQGRAPH](/kb/en/oqgraph/) storage engine is installed, if set to `1` (`0` is default), permits the `latch` field to be an integer (see [OQGRAPH Overview](/kb/en/oqgraph-overview/#creating-a-table)).
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`
- <strong>Introduced:</strong> [MariaDB 10.0.7](/kb/en/mariadb-1007-release-notes/)

---

## Status Variables

#### `Oqgraph_boost_version`

- <strong>Description:</strong> [OQGRAPH](/kb/en/oqgraph/) boost version.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `string`
- <strong>Introduced:</strong> [MariaDB 10.0.7](/kb/en/mariadb-1007-release-notes/) with the [OQGRAPH](/kb/en/oqgraph/) storage engine.

---

#### `Oqgraph_compat_mode`

- <strong>Description:</strong> Whether or not legacy tables with integer latches are supported.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `string`
- <strong>Introduced:</strong> [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/)

---

#### `Oqgraph_verbose_debug`

- <strong>Description:</strong> Whether or not verbose debugging is enabled. If it is, performance may be adversely impacted
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `string`
- <strong>Introduced:</strong> [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/)

---