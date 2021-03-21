# Spider Table System Variables

The following variables are only available in the `COMMENT` clause of the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) statement when the [Spider](/columns-storage-engines-and-plugins/storage-engines/spider) storage engine has been installed. Global variables can be overloaded at the table level using the DSN parameter. These variables are listed on the [Spider System Variables](/columns-storage-engines-and-plugins/storage-engines/spider/spider-server-system-variables) page.

#### `access_balances`

- <strong>Description:</strong> Connection load balancing integer weight.
- <strong>Default Table Value:</strong> `0`
- <strong>DSN Parameter Name:</strong> `abl`

#### `active_link_count`

- <strong>Description:</strong> Number of active remote servers, for use in load balancing read connections
- <strong>Default Table Value:</strong> `all backends`
- <strong>DSN Parameter Name:</strong> `alc`

#### `casual_read`

- <strong>Description:</strong>
- <strong>Default Table Value:</strong>
- <strong>DSN Parameter Name:</strong>
- <strong>Introduced:</strong> Spider 3.2

#### `database`

- <strong>Description:</strong> Database name for reference table that exists on remote backend server.
- <strong>Default Table Value:</strong> `local table database`
- <strong>DSN Parameter Name:</strong> `database`

#### `default_file`

- <strong>Description:</strong> Configuration file used when connecting to remote servers.  When the <a undefined>default_group</a> table variable is set, this variable defaults to the values of the `--defaults-extra-file` or `--defaults-file` options.  When the <a undefined>default_group</a> table variable is not set, it defaults to `none`.
- <strong>Default Table Value:</strong> `none`
- <strong>DSN Parameter Name:</strong> `dff`

#### `default_group`

- <strong>Description:</strong> Group name in configuration file used when connecting to remote servers.
- <strong>Default Table Value:</strong> `none`
- <strong>DSN Parameter Name:</strong> `dfg`

#### `delete_all_rows_type`

- <strong>Description:</strong>
- <strong>Default Table Value:</strong>
- <strong>DSN Parameter Name:</strong>
- <strong>Introduced:</strong> Spider 3.2

#### `host`

- <strong>Description:</strong> Host name of remote server.
- <strong>Default Table Value:</strong> `localhost`
- <strong>DSN Parameter Name:</strong> `host`

#### `idx000`

- <strong>Description:</strong> When using an index on Spider tables for searching, Spider uses this hint to search the remote table.  The remote table index is related to the Spider table index by this hint.  The number represented by `000` is the index ID, which is the number of the index shown by the [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table) statement.  `000` is the Primary Key.  For instance, `idx000 "force index(PRIMARY)"` (in abbreviated format `idx000 "f PRIMARY"`). 
<ul start="1"><li>`f` force index
</li><li>`u` use index 
</li><li>`ig` ignore index 
</li></ul>
- <strong>Default Table Value:</strong> `none`

#### `internal_delayed`

- <strong>Description:</strong> Whether to transmit existence of delay to remote servers when executing an [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed) statement on local server.
<ul start="1"><li>`0` Doesn't transmit.
</li><li>`1` Transmits.
</li></ul>
- <strong>Default Table Value:</strong> `0`
- <strong>DSN Parameter Name:</strong> `idl`

#### `link_status`

- <strong>Description:</strong> Change status of the remote backend server link.
<ul start="1"><li>`0` Doesn't change status. 
</li><li>`1` Changes status to `OK`.
</li><li>`2` Changes status to `RECOVERY`.
</li><li>`3` Changes status to no more in group communication.
</li></ul>
- <strong>Default Table Value:</strong> `0`
- <strong>DSN Parameter Name:</strong> `lst`

#### `monitoring_bg_interval`

- <strong>Description:</strong> Interval of background monitoring in microseconds.
- <strong>Default Table Value:</strong> `10000000`
- <strong>DSN Parameter Name:</strong> `mbi`

#### ` monitoring_bg_kind`

- <strong>Description:</strong> Kind of background monitoring to use.
<ul start="1"><li>`0` Disables background monitoring. 
</li><li>`1` Monitors connection state.
</li><li>`2` Monitors state of table without `WHERE` clause.
</li><li>`3` Monitors state of table with `WHERE` clause (currently unsupported).
</li></ul>
- <strong>Default Table Value:</strong> `0`
- <strong>DSN Parameter Name:</strong> `mbk`

#### `monitoring_kind`

- <strong>Description:</strong> Kind of monitoring.
<ul start="1"><li>`0` Disables monitoring
</li><li>`1` Monitors connection state. 
</li><li>`2` Monitors state of table without `WHERE` clause. 
</li><li>`3` Monitors state of table with `WHERE` clause (currently unsupported).
</li></ul>
- <strong>Default Table Value:</strong> `0`
- <strong>DSN Parameter Name:</strong> `mkd`

#### `monitoring_limit`

- <strong>Description:</strong> Limits the number of records in the monitoring table.  This is only effective when Spider monitors the state of a table, which occurs when the <a undefined>monitoring_kind</a> table variable is set to a value greater than `1`.
- <strong>Default Table Value:</strong> `1`
- <strong>Range: </strong> `0` upwards
- <strong>DSN Parameter Name:</strong> `mlt`

#### `monitoring_server_id`

- <strong>Description:</strong> Preferred monitoring `@@server_id` for each backend failure.  You can use this to geo-localize backend servers and set the first Spider monitoring node to contact for failover.  In the event that this monitor fails, other monitoring nodes are contacted.  For multiple copy backends, you can set a lazy configuration with a single MSI instead of one per backend.
- <strong>Default Table Value:</strong> `server_id`
- <strong>DSN Parameter Name:</strong> `msi`

#### `password`

- <strong>Description:</strong> Remote server password.
- <strong>Default Table Value:</strong> `none`
- <strong>DSN Parameter Name:</strong> `password`

#### `port`

- <strong>Description:</strong> Remote server port.
- <strong>Default Table Value:</strong> `3306`
- <strong>DSN Parameter Name:</strong> `port`

#### `priority`

- <strong>Description:</strong> Priority. Used to define the order of execution.  For instance, Spider uses priority when deciding the order in which to lock tables on a remote server.
- <strong>Default Table Value:</strong> `1000000`
- <strong>DSN Parameter Name:</strong> `prt`

#### `query_cache`

- <strong>Description:</strong> Passes the option for the [Query Cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache) when issuing [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) statements to the remote server.
<ul start="1"><li>`0` No option passed.
</li><li>`1` Passes the <a undefined>SQL_CACHE</a> option. 
</li><li>`2` Passes the <a undefined>SQL_NO_CACHE</a> option. 
</li></ul>
- <strong>Default Table Value:</strong> `0`
- <strong>DSN Parameter Name:</strong> `qch`

#### `read_rate`

- <strong>Description:</strong> Rate used to calculate the amount of time Spider requires when executing index scans.
- <strong>Default Table Value:</strong> `0.0002`
- <strong>DSN Parameter Name:</strong> `rrt`

#### `scan_rate`

- <strong>Description:</strong> Rate used to calculate the amount of time Spider requires when scanning tables.
- <strong>Default Table Value:</strong> ` 0.0001`
- <strong>DSN Parameter Name:</strong> `srt`

#### `server`

- <strong>Description:</strong> Server name.  Used when generating connection information with [CREATE SERVER](/sql-statements-structure/sql-statements/data-definition/create/create-server) statements.
- <strong>Default Table Value:</strong> `none`
- <strong>DSN Parameter Name:</strong> `srv`

#### `socket`

- <strong>Description:</strong> Remote server socket.
- <strong>Default Table Value:</strong> `none`
- <strong>DSN Parameter Name:</strong> `socket`

#### `ssl_ca`

- <strong>Description:</strong> Path to the Certificate Authority file.
- <strong>Default Table Value:</strong> `none`
- <strong>DSN Parameter Name:</strong> `sca`

#### `ssl_capath`

- <strong>Description:</strong> Path to directory containing trusted TLS CA certificates in PEM format.
- <strong>Default Table Value:</strong> `none`
- <strong>DSN Parameter Name:</strong> `scp`

#### `ssl_cert`

- <strong>Description:</strong> Path to the certificate file.
- <strong>Default Table Value:</strong> `none`
- <strong>DSN Parameter Name:</strong> `scr`

#### `ssl_cipher`

- <strong>Description:</strong> List of allowed ciphers to use with [TLS encryption](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview).
- <strong>Default Table Value:</strong> `none`
- <strong>DSN Parameter Name:</strong> `sch`

#### `ssl_key`

- <strong>Description:</strong> Path to the key file.
- <strong>Default Table Value:</strong> `none`
- <strong>DSN Parameter Name:</strong> `sky`

#### `ssl_verify_server_cert`

- <strong>Description:</strong> Enables verification of the server's Common Name value in the certificate against the host name used when connecting to the server.
<ul start="1"><li>`0` Disables verification.
</li><li>`1` Enables verification.
</li></ul>
- <strong>Default Table Value:</strong> `0`
- <strong>DSN Parameter Name:</strong> `svc`

#### `table`

- <strong>Description:</strong> Destination table name.
- <strong>Default Table Value:</strong> `Same table name`
- <strong>DSN Parameter Name:</strong> `tbl`