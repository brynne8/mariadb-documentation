# MariaDB Audit Plugin Options and System Variables

There are a several options and system variables related to the [MariaDB Audit Plugin](/kb/en/server_audit-mariadb-audit-plugin/), once it has been [installed](/kb/en/mariadb-audit-plugin-entitymdashentity-installation/).  System variables can be displayed using the [SHOW VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables/) statement like so:

```sql
SHOW GLOBAL VARIABLES LIKE '%server_audit%';

+-------------------------------+-----------------------+
| Variable_name                 | Value                 |
+-------------------------------+-----------------------+
| server_audit_events           | CONNECT,QUERY,TABLE   |
| server_audit_excl_users       |                       |
| server_audit_file_path        | server_audit.log      |
| server_audit_file_rotate_now  | OFF                   |
| server_audit_file_rotate_size | 1000000               |
| server_audit_file_rotations   | 9                     |
| server_audit_incl_users       |                       |
| server_audit_logging          | ON                    |
| server_audit_mode             | 0                     |
| server_audit_output_type      | file                  |
| server_audit_query_log_limit  | 1024                  |
| server_audit_syslog_facility  | LOG_USER              |
| server_audit_syslog_ident     | mysql-server_auditing |
| server_audit_syslog_info      |                       |
| server_audit_syslog_priority  | LOG_INFO              |
+-------------------------------+-----------------------+
```

To change the value of one of these variables, you can use the `SET` statement, or set them at the command-line when starting MariaDB.  It's recommended that you set them in the MariaDB configuration for the server like so:

```sql
[mariadb]
...
server_audit_excl_users='bob,ted'
...
```

### System Variables

Below is a list of all system variables related to the Audit Plugin.  See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting them. See also the [full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

#### `server_audit_events`

- <strong>Description:</strong> If set, then this restricts audit logging to certain event types. If not set, then every event type is logged to the audit log. For example: <em>SET GLOBAL server_audit_events='connect, query'</em>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-events=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> Empty string
- <strong>Valid Values:</strong>
<ul start="1"><li>`CONNECT`, `QUERY`, `TABLE` (MariaDB Audit Plugin &lt; 1.2.0)
</li><li>`CONNECT`, `QUERY`, `TABLE`, `QUERY_DDL`, `QUERY_DML` (MariaDB Audit Plugin &gt;= 1.2.0)
</li><li>`CONNECT`, `QUERY`, `TABLE`, `QUERY_DDL`, `QUERY_DML`, `QUERY_DCL` (MariaDB Audit Plugin &gt;=1.3.0)
</li><li>`CONNECT`, `QUERY`, `TABLE`, `QUERY_DDL`, `QUERY_DML`, `QUERY_DCL`, `QUERY_DML_NO_SELECT` (MariaDB Audit Plugin &gt;= 1.4.4)
</li><li>See [MariaDB Audit Plugin - Versions](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-versions/) to determine which MariaDB releases contain each MariaDB Audit Plugin versions.
</li></ul>

---

#### `server_audit_excl_users`

- <strong>Description:</strong> If not empty, it contains the list of users whose activity will NOT be logged.	For example: `SET GLOBAL server_audit_excl_users='user_foo, user_bar'`. CONNECT records aren't affected by this variable - they are always logged. The user is still logged if it's specified in [server_audit_incl_users](#server_audit_incl_users).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-excl-users=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> Empty string
- <strong>Size limit:</strong> 1024 characters

---

#### `server_audit_file_path`

- <strong>Description:</strong> When [server_audit_output_type=file](#server_audit_output_type), sets the path and the filename to the log file. If the specified path exists as a directory, then the log will be created inside that directory with the name 'server_audit.log'. Otherwise the value is treated as a filename. The default value is 'server_audit.log', which means this file will be created in the database directory.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-file-path=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `server_audit.log`

---

#### `server_audit_file_rotate_now`

- <strong>Description:</strong> When [server_audit_output_type=file](#server_audit_output_type), the user can force the log file rotation by setting this variable to ON or 1.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-rotate-now[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `server_audit_file_rotate_size`

- <strong>Description:</strong> When [server_audit_output_type=file](#server_audit_output_type), it limits the size of the log file to the given amount of bytes. Reaching that limit turns on the rotation - the current log file is renamed as 'file_path.1'. The empty log file is created as 'file_path' to log into it. The default value is 1000000.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-rotate-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1000000`

---

#### `server_audit_file_rotations`

- <strong>Description:</strong> When [server_audit_output_type=file](#server_audit_output_type)', this specifies the number of rotations to save. If set to 0 then the log never rotates. The default value is 9.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-rotations=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `9`
- <strong>Range:</strong> `0` to `999`

---

#### `server_audit_incl_users`

- <strong>Description:</strong> If not empty, it contains a comma-delimited list of users whose activity will be logged. For example: `SET GLOBAL server_audit_incl_users='user_foo, user_bar'`. CONNECT records aren't affected by this variable - they are always logged. This setting has higher priority than [server_audit_excl_users](#server_audit_excl_users). So if the same user is specified both in incl_ and excl_ lists, they will still be logged.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-incl-users=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> Empty string
- <strong>Size limit:</strong> 1024 characters

---

#### `server_audit_loc_info`

- <strong>Description:</strong> Used by plugin internals. It has no useful meaning to users.
<ul start="1"><li>In earlier versions, users see it as a read-only variable.
</li><li>In later versions, it is hidden from the user.
</li></ul>
- <strong>Commandline:</strong> N/A
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> Empty string
- <strong>Introduced:</strong> [MariaDB 10.1.12](/kb/en/mariadb-10112-release-notes/), [MariaDB 10.0.24](/kb/en/mariadb-10024-release-notes/), [MariaDB 5.5.48](/kb/en/mariadb-5548-release-notes/)
- <strong>Hidden:</strong> [MariaDB 10.1.18](/kb/en/mariadb-10118-release-notes/), [MariaDB 10.0.28](/kb/en/mariadb-10028-release-notes/), [MariaDB 5.5.53](/kb/en/mariadb-5553-release-notes/)

---

#### `server_audit_logging`

- <strong>Description:</strong> Enables/disables the logging. Expected values are ON/OFF. For example: `SET GLOBAL server_audit_logging=on` If the server_audit_output_type is FILE, this will actually create/open the logfile so the [server_audit_file_path](#server_audit_file_path) should be properly specified beforehand. Same about the SYSLOG-related parameters. The logging is turned off by default.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-logging[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `server_audit_mode`

- <strong>Description:</strong> This variable doesn't have any distinctive meaning for a user. Its value mostly reflects the server version with which the plugin was started and is intended to be used by developers for testing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-mode[=#]</code>

---

#### `server_audit_output_type`

- <strong>Description:</strong> Specifies the desired output type. Can be SYSLOG or FILE. For example: `SET GLOBAL server_audit_output_type=file` file: log records will be saved into the rotating log file. The name of the file set by [server_audit_file_path](#server_audit_file_path) variable. syslog: log records will be sent to the local syslogd daemon with the standard &lt;syslog.h&gt; API. The default value is 'file'.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-output-type=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `file`
- <strong>Valid Values:</strong> `SYSLOG` or `FILE`

---

#### `server_audit_query_log_limit`

- <strong>Description:</strong> Limit on the length of the query string in a record.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-query-log-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1024`
- <strong>Range:</strong> `0` to `2147483647`
- <strong>Introduced:</strong> [MariaDB 5.5.43](/kb/en/mariadb-5543-release-notes/), [MariaDB 10.0.18](/kb/en/mariadb-10018-release-notes/), [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/)

---

#### `server_audit_syslog_facility`

- <strong>Description:</strong> SYSLOG-mode variable. It defines the 'facility' of the records that will be sent to the syslog. Later the log can be filtered by this parameter.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-syslog-facility=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `LOG_USER`
- <strong>Valid Values:</strong> `LOG_USER`, `LOG_MAIL`, `LOG_DAEMON`, `LOG_AUTH`, `LOG_SYSLOG`, `LOG_LPR`, `LOG_NEWS`, `LOG_UUCP`, `LOG_CRON`, `LOG_AUTHPRIV`, `LOG_FTP`, and `LOG_LOCAL0`–`LOG_LOCAL7`.

---

#### `server_audit_syslog_ident`

- <strong>Description:</strong> SYSLOG-mode variable. String value for the 'ident' part of each syslog record. Default value is 'mysql-server_auditing'. New value becomes effective only after restarting the logging.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-syslog-ident=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `mysql-server_auditing`

---

#### `server_audit_syslog_info`

- <strong>Description:</strong> SYSLOG-mode variable. The 'info' string to be added to the syslog records. Can be changed any time.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-syslog-info=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> Empty string

---

#### `server_audit_syslog_priority`

- <strong>Description:</strong> SYSLOG-mode variable. Defines the priority of the log records for the syslogd.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit-syslog-priority=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `LOG_INFO`
- <strong>Valid Values:</strong>`LOG_EMERG`, `LOG_ALERT`, `LOG_CRIT`, `LOG_ERR`, `LOG_WARNING`, `LOG_NOTICE`, `LOG_INFO`, `LOG_DEBUG`

---

### Options

#### `server_audit`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the [mysql.plugins](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlplugin-table/) table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [MariaDB Audit Plugin - Installation: Prohibiting Uninstallation](/kb/en/mariadb-audit-plugin-installation/#prohibiting-uninstallation) for more information on one use case.
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--server-audit=val</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---