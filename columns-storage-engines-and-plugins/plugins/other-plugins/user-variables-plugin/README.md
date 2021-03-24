# User Variables Plugin

##### MariaDB starting with [10.2.0](/kb/en/mariadb-1020-release-notes/)

The `user_variables` plugin was first released in [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/)

The `user_variables` plugin creates the <a undefined>USER_VARIABLES</a> table in the [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database. This table contains information about [user-defined variables](/sql-statements-structure/sql-language-structure/user-defined-variables/).

## Installing the Plugin

##### MariaDB starting with [10.2.6](/kb/en/mariadb-1026-release-notes/)

In [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) and later, the `user_variables` plugin is statically linked into the server by default, so it does not need to be installed.

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default prior to [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/). There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). For example:

```sql
INSTALL SONAME 'user_variables';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = user_variables
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'user_variables';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Example

```sql
SELECT * FROM information_schema.USER_VARIABLES ORDER BY VARIABLE_NAME;
+---------------+----------------+---------------+--------------------+
| VARIABLE_NAME | VARIABLE_VALUE | VARIABLE_TYPE | CHARACTER_SET_NAME |
+---------------+----------------+---------------+--------------------+
| var           | 0              | INT           | utf8               |
| var2          | abc            | VARCHAR       | utf8               |
+---------------+----------------+---------------+--------------------+
```

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a></td></tr>
<tr><td>1.0</td><td>Gamma</td><td><a href="/kb/en/mariadb-1026-release-notes/">MariaDB 10.2.6</a></td></tr>
<tr><td>1.0</td><td>Alpha</td><td><a href="/kb/en/mariadb-1020-release-notes/">MariaDB 10.2.0</a></td></tr>
</tbody></table>

## Options

### `user_variables`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the [mysql.plugins](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlplugin-table/) table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--user-variables=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---