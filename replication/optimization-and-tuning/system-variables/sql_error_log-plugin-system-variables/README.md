# SQL_ERROR_LOG Plugin System Variables

This page documents system variables related to the [SQL_Error_Log Plugin](/kb/en/sql_error_log-plugin/). See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting them.

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `sql_error_log_filename`

- <strong>Description:</strong> The name (and optionally path) of the logfile containing the errors. Rotation will use a naming convention such as `sql_error_log_filename.001`. If no path is specified, the log file will be written to the [data directory](/kb/en/server-system-variables/#datadir).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-error-log-filename=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `sql_errors.log`

---

#### `sql_error_log_rate`

- <strong>Description:</strong> The logging sampling rate. Setting to `10`, for example, means that one in ten errors will be logged. If set to zero, logging is disabled. The default, `1`, logs every error.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-error-log-rate=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `1`

---

#### `sql_error_log_rotate`

- <strong>Description:</strong> Setting to #1` forces log rotation.`
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-error-log-rate[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `sql_error_log_rotations`

- <strong>Description:</strong> Number of rotations before the log is removed. When rotated, the current log file is stored and a new, empty, log is created. Any rotations older than this setting are removed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-error-log-rotations=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `9`
- <strong>Range:</strong> `1` to `999`

---

#### `sql_error_log_size_limit`

- <strong>Description:</strong> The log file size limit in bytes. After reaching this size, the log file is rotated.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-error-log-size-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1000000`
- <strong>Range:</strong> `100` to `9223372036854775807`

---