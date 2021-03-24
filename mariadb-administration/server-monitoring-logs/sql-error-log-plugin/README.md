# SQL Error Log Plugin

##### MariaDB starting with [5.5.22](/kb/en/mariadb-5522-release-notes/)

The `SQL_ERROR_LOG` plugin was first released in [MariaDB 5.5.22](/kb/en/mariadb-5522-release-notes/).

The `SQL_ERROR_LOG` plugin collects errors sent to clients in a log file defined by <a undefined>sql_error_log_filename</a>, so that they can later be analyzed. The log file can be rotated if <a undefined>sql_error_log_rotate</a> is set.

Comments are also logged, which can make the log easier to search. But this is only possible if the client does not strip the comments away. For example, [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) command-line client only leaves comments when started with the <a undefined>--comments</a> option.

It is implemented as a <a undefined>MYSQL_AUDIT_PLUGIN</a>.

## Installing the Plugin

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). For example:

```sql
INSTALL SONAME 'sql_errlog';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = sql_errlog
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'sql_errlog';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Example

```sql
install plugin SQL_ERROR_LOG soname 'sql_errlog';
Query OK, 0 rows affected (0.00 sec)

use test;
Database changed

set sql_mode='STRICT_ALL_TABLES,NO_ENGINE_SUBSTITUTION';
Query OK, 0 rows affected (0.00 sec)

CREATE TABLE foo2 (id int) ENGINE=WHOOPSIE;
ERROR 1286 (42000): Unknown storage engine 'WHOOPSIE'
\! cat data/sql_errors.log
2013-03-19  9:38:40 msandbox[msandbox] @ localhost [] ERROR 1286: Unknown storage engine 'WHOOPSIE' : CREATE TABLE foo2 (id int) ENGINE=WHOOPSIE
```

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a></td></tr>
<tr><td>1.0</td><td>Gamma</td><td><a href="/kb/en/mariadb-10010-release-notes/">MariaDB 10.0.10</a></td></tr>
<tr><td>1.0</td><td>Alpha</td><td><a href="/kb/en/mariadb-5522-release-notes/">MariaDB 5.5.22</a></td></tr>
</tbody></table>

## System Variables

### `sql_error_log_filename`

- <strong>Description:</strong> The name of the logfile. Rotation of it will be named like `<em>sql_error_log_filename</em>.001`
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-error-log-filename=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `sql_errors.log`

---

### `sql_error_log_rate`

- <strong>Description:</strong> The rate of logging. `SET sql_error_log_rate=300;` means that one of 300 errors will be written to the log.<br>If `sql_error_log_rate` is `0` the logging is disabled.<br>The default rate is `1` (every error is logged).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-error-log-rate=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`

---

### `sql_error_log_rotate`

- <strong>Description:</strong> This is the 'write-only' variable. Assigning TRUE to this variable forces the log rotation.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-error-log-rotate={0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

### `sql_error_log_rotations`

- <strong>Description:</strong> The number of rotations. When rotated, the current log file is stored and the new empty one created.<br>The sql_error_log_rotations logs are stored, older are removed.<br>The default number of  rotations is `9`.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-error-log-rotations</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `9`
- <strong>Range:</strong> `1` to `999`

---

### `sql_error_log_size_limit`

- <strong>Description:</strong> The limitation for the size of the log file. After reaching the specified limit, the log file is rotated.<br>1M limit set by default.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-error-log-size-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1000000`
- <strong>Range:</strong> `100` to `9223372036854775807`

---

## Options

### `sql_error_log`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the [mysql.plugins](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlplugin-table/) table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-error-log=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---