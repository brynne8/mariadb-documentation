# MariaDB Audit Plugin - Status Variables

There are a few status variables related to the [MariaDB Audit Plugin](/kb/en/server_audit-mariadb-audit-plugin/), once it has been [installed](/kb/en/mariadb-audit-plugin-entitymdashentity-installation/).  These variables can be displayed using the [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status) statement like so:

```sql
SHOW STATUS LIKE 'server_audit%';

+----------------------------+------------------+
| Variable_name              | Value            |
+----------------------------+------------------+
| Server_audit_active        | ON               |
| Server_audit_current_log   | server_audit.log |
| Server_audit_last_error    |                  |
| Server_audit_writes_failed | 0                |
+----------------------------+------------------+
```

### Status Variables

Below is a list of all status variables related to the Audit Plugin.  These cannot be set: These are not to be confused with system variables, which can be set.  See [Server Status Variables](/replication/optimization-and-tuning/system-variables/server-status-variables) for a complete list of status variables that can be viewed with the [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status) statement. See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables).

#### `Server_audit_active`

- <strong>Description:</strong> If the auditing is actually working. It gets the ON value when the logging is successfully started. Then it can get the OFF value if the logging was stopped or log records can't be properly stored due to file or syslog errors.
- <strong>Data Type:</strong> `boolean`

---

#### `Server_audit_current_log`

- <strong>Description:</strong> The name of the logfile or the SYSLOG parameters that are in current use.
- <strong>Data Type:</strong> `string`

---

#### `Server_audit_last_error`

- <strong>Description:</strong> If something went wrong with the logging here you can see the message.
- <strong>Data Type:</strong> `string`

---

#### `Server_audit_writes_failed`

- <strong>Description:</strong> The number of log records since last logging-start that weren't properly stored because of errors of any kind. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`

---