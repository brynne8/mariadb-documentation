# Plugin Overview

Plugins are server components that enhance MariaDB in some way. These can be anything from new storage engines, plugins for enhancing full-text parsing, or even small enhancements, such as a plugin to get a timestamp as an integer.

## Querying Plugin Information

There are a number of ways to see which plugins are currently active.

A server almost always has a large number of active plugins, because the server contains a large number of built-in plugins, which are active by default and cannot be uninstalled.

### Querying Plugin Information with `SHOW PLUGINS`

The [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins/) statement can be used to query information about all active plugins.

For example:

```sql
SHOW PLUGINS\G;
********************** 1. row **********************
   Name: binlog
 Status: ACTIVE
   Type: STORAGE ENGINE
Library: NULL
License: GPL
********************** 2. row **********************
   Name: mysql_native_password
 Status: ACTIVE
   Type: AUTHENTICATION
Library: NULL
License: GPL
********************** 3. row **********************
   Name: mysql_old_password
 Status: ACTIVE
   Type: AUTHENTICATION
Library: NULL
License: GPL
...
```

If a plugin's `Library` column has a `NULL` value, then the plugin is built-in, and it cannot be uninstalled.

### Querying Plugin Information with information_schema.PLUGINS

The [information_schema.PLUGINS](/kb/en/information_schemaplugins-table/) table can be queried to get more detailed information about plugins.

For example:

```sql
SELECT * FROM information_schema.PLUGINS\G
...
*************************** 6. row ***************************
           PLUGIN_NAME: CSV
        PLUGIN_VERSION: 1.0
         PLUGIN_STATUS: ACTIVE
           PLUGIN_TYPE: STORAGE ENGINE
   PLUGIN_TYPE_VERSION: 100003.0
        PLUGIN_LIBRARY: NULL
PLUGIN_LIBRARY_VERSION: NULL
         PLUGIN_AUTHOR: Brian Aker, MySQL AB
    PLUGIN_DESCRIPTION: CSV storage engine
        PLUGIN_LICENSE: GPL
           LOAD_OPTION: FORCE
       PLUGIN_MATURITY: Stable
   PLUGIN_AUTH_VERSION: 1.0
*************************** 7. row ***************************
           PLUGIN_NAME: MEMORY
        PLUGIN_VERSION: 1.0
         PLUGIN_STATUS: ACTIVE
           PLUGIN_TYPE: STORAGE ENGINE
   PLUGIN_TYPE_VERSION: 100003.0
        PLUGIN_LIBRARY: NULL
PLUGIN_LIBRARY_VERSION: NULL
         PLUGIN_AUTHOR: MySQL AB
    PLUGIN_DESCRIPTION: Hash based, stored in memory, useful for temporary tables
        PLUGIN_LICENSE: GPL
           LOAD_OPTION: FORCE
       PLUGIN_MATURITY: Stable
   PLUGIN_AUTH_VERSION: 1.0
...
```

If a plugin's `PLUGIN_LIBRARY` column has the `NULL` value, then the plugin is built-in, and it cannot be uninstalled.

### Querying Plugin Information with `mysql.plugin`

The [mysql.plugin](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlplugin-table/) table can be queried to get information about installed plugins.

This table only contains information about plugins that have been installed via the following methods:

- The [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) statement.
- The [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/) statement.
- The [mysql_plugin](/clients-utilities/mysql_plugin/) utility.

This table does not contain information about:

- Built-in plugins.
- Plugins loaded with the [--plugin-load-add](/kb/en/mysqld-options/#-plugin-load-add) option.
- Plugins loaded with the [--plugin-load](/kb/en/mysqld-options/#-plugin-load) option.

This table only contains enough information to reload the plugin when the server is restarted, which means it only contains the plugin name and the plugin library.

For example:

```sql
SELECT * FROM mysql.plugin;

+------+------------+
| name | dl         |
+------+------------+
| PBXT | libpbxt.so |
+------+------------+
```

## Installing a Plugin

There are three primary ways to install a plugin:

- A plugin can be installed dynamically with an SQL statement.
- A plugin can be installed with a [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) option, but it requires a server restart.
- A plugin can be installed with the [mysql_plugin](/clients-utilities/mysql_plugin/) utility, while the server is completely offline.

When you are installing a plugin, you also have to ensure that:

- The server's plugin directory is properly configured, and the plugin's library is in the plugin directory.
- The server's minimum plugin maturity is properly configured, and the plugin is mature enough to be installed.

### Installing a Plugin Dynamically

A plugin can be installed dynamically by executing either the [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or the [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/) statement.

If a plugin is installed with one of these statements, then a record will be added to the [mysql.plugins](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlplugin-table/) table for the plugin. This means that the plugin will automatically be loaded every time the server restarts, unless specifically uninstalled or deactivated.

#### Installing a Plugin with `INSTALL SONAME`

You can install a plugin dynamically by executing the [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) statement. [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) installs all plugins from the given plugin library. This could be required for some plugin libraries.

For example, to install all plugins in the `server_audit` plugin library (which is currently only the [server_audit](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/) audit plugin), you could execute the following:

```sql
INSTALL SONAME 'server_audit';
```

#### Installing a Plugin with `INSTALL PLUGIN`

You can install a plugin dynamically by executing the [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/) statement. [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/) installs a single plugin from the given plugin library.

For example, to install the [server_audit](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/) audit plugin from the `server_audit` plugin library, you could execute the following:

```sql
INSTALL PLUGIN server_audit SONAME 'server_audit';
```

### Installing a Plugin with Plugin Load Options

A plugin can be installed with a [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) option by providing either the [--plugin-load-add](/kb/en/mysqld-options/#-plugin-load-add) or the [--plugin-load](/kb/en/mysqld-options/#-plugin-load) option.

If a plugin is installed with one of these options, then a record will <strong>not</strong> be added to the [mysql.plugins](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlplugin-table/) table for the plugin. This means that if the server is restarted without the same option set, then the plugin will <strong>not</strong> automatically be loaded.

#### Installing a Plugin with `--plugin-load-add`

You can install a plugin with the <a undefined>--plugin-load-add</a> option by specifying the option as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or by specifying the option in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/).

The <a undefined>--plugin-load-add</a> option uses the following format:

- Plugins can be specified in the format `name=library`, where `name` is the plugin name and `library` is the plugin library. This format installs a single plugin from the given plugin library.
- Plugins can also be specified in the format `library`, where `library` is the plugin library. This format installs all plugins from the given plugin library.
- Multiple plugins can be specified by separating them with semicolons.

For example, to install all plugins in the `server_audit` plugin library (which is currently only the [server_audit](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/) audit plugin) and also the [ed25519](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-ed25519/) authentication plugin from the `auth_ed25519` plugin library, you could set the option to the following values on the command-line:

```sql
$ mysqld --user=mysql --plugin-load-add='server_audit' --plugin-load-add='ed25519=auth_ed25519'
```

You could also set the option to the same values in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

```sql
[mariadb]
...
plugin_load_add = server_audit
plugin_load_add = ed25519=auth_ed25519
```

Special care must be taken when specifying both the <a undefined>--plugin-load</a> option and the <a undefined>--plugin-load-add</a> option together. The <a undefined>--plugin-load</a> option resets the plugin load list, and this can cause unexpected problems if you are not aware. The <a undefined>--plugin-load-add</a> option does <strong>not</strong> reset the plugin load list, so it is much safer to use. See [Specifying Multiple Plugin Load Options](#specifying-multiple-plugin-load-options) for more information.

#### Installing a Plugin with `--plugin-load`

You can install a plugin with the <a undefined>--plugin-load</a> option by specifying the option as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or by specifying the option in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/).

The <a undefined>--plugin-load</a> option uses the following format:

- Plugins can be specified in the format `name=library`, where `name` is the plugin name and `library` is the plugin library. This format installs a single plugin from the given plugin library.
- Plugins can also be specified in the format `library`, where `library` is the plugin library. This format installs all plugins from the given plugin library.
- Multiple plugins can be specified by separating them with semicolons.

For example, to install all plugins in the `server_audit` plugin library (which is currently only the [server_audit](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/) audit plugin) and also the [ed25519](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-ed25519/) authentication plugin from the `auth_ed25519` plugin library, you could set the option to the following values on the command-line:

```sql
$ mysqld --user=mysql --plugin-load='server_audit;ed25519=auth_ed25519'
```

You could also set the option to the same values in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

```sql
[mariadb]
...
plugin_load = server_audit;ed25519=auth_ed25519
```

Special care must be taken when specifying the <a undefined>--plugin-load</a> option multiple times, or when specifying both the <a undefined>--plugin-load</a> option and the <a undefined>--plugin-load-add</a> option together. The <a undefined>--plugin-load</a> option resets the plugin load list, and this can cause unexpected problems if you are not aware. The <a undefined>--plugin-load-add</a> option does <strong>not</strong> reset the plugin load list, so it is much safer to use. See [Specifying Multiple Plugin Load Options](#specifying-multiple-plugin-load-options) for more information.

#### Specifying Multiple Plugin Load Options

Special care must be taken when specifying the <a undefined>--plugin-load</a> option multiple times, or when specifying both the <a undefined>--plugin-load</a> option and the <a undefined>--plugin-load-add</a> option. The <a undefined>--plugin-load</a> option resets the plugin load list, and this can cause unexpected problems if you are not aware. The <a undefined>--plugin-load-add</a> option does <strong>not</strong> reset the plugin load list, so it is much safer to use.

This can have the following consequences:

- If the <a undefined>--plugin-load</a> option is specified <strong>multiple times</strong>, then only the last instance will have any effect. For example, in the following case, the first instance of the option is reset:

```sql
[mariadb]
...
plugin_load = server_audit
plugin_load = ed25519=auth_ed25519
```

- If the <a undefined>--plugin-load</a> option is specified <strong>after</strong> the <a undefined>--plugin-load-add</a> option, then it will also reset the changes made by that option. For example, in the following case, the <a undefined>--plugin-load-add</a> option does not do anything, because the subsequent <a undefined>--plugin-load</a> option resets the plugin load list:

```sql
[mariadb]
...
plugin_load_add = server_audit
plugin_load = ed25519=auth_ed25519
```

- In contrast, if the <a undefined>--plugin-load</a> option is specified <strong>before</strong> the <a undefined>--plugin-load-add</a> option, then it will work fine, because the <a undefined>--plugin-load-add</a> option does not reset the plugin load list. For example, in the following case, both plugins are properly loaded:

```sql
[mariadb]
...
plugin_load = server_audit
plugin_load_add = ed25519=auth_ed25519
```

### Installing a Plugin with `mysql_plugin`

A plugin can be installed with the [mysql_plugin](/clients-utilities/mysql_plugin/) utility if the server is completely offline.

The syntax is:

```sql
mysql_plugin [options] <plugin> ENABLE|DISABLE
```

For example, to install the [server_audit](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/) audit plugin, you could execute the following:

```sql
mysql_plugin server_audit ENABLE
```

If a plugin is installed with this utility, then a record will be added to the [mysql.plugins](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlplugin-table/) table for the plugin. This means that the plugin will automatically be loaded every time the server restarts, unless specifically uninstalled or deactivated.

### Configuring the Plugin Directory

When a plugin is being installed, the server looks for the plugin's library in the server's plugin directory. This directory is configured by the <a undefined>plugin_dir</a> system variable. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_dir = /usr/lib64/mysql/plugin
```

### Configuring the Minimum Plugin Maturity

When a plugin is being installed, the server compares the plugin's maturity level against the server's minimum allowed plugin maturity. This can help prevent users from using unstable plugins on production servers. This minimum plugin maturity is configured by the <a undefined>plugin_maturity</a> system variable. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_maturity = stable
```

### Configuring Plugin Activation at Server Startup

A plugin will be loaded by default when the server starts if:

- The plugin was installed with the [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) statement.
- The plugin was installed with the [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/) statement.
- The plugin was installed with the [mysql_plugin](/clients-utilities/mysql_plugin/) utility.
- The server is configured to load the plugin with the <a undefined>--plugin-load-add</a> option.
- The server is configured to load the plugin with the <a undefined>--plugin-load</a> option.

This behavior can be changed with special options that take the form `--plugin-name`. For example, for the [server_audit](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/) audit plugin, the special option is called <a undefined>--server-audit</a>.

The possible values for these special options are:

<table><tbody><tr><th>Option Value</th><th>Description</th></tr>
<tr><td><code>OFF</code></td><td>Disables the plugin without removing it from the <code><a href="/kb/en/mysqlplugin-table/">mysql.plugins</a></code> table.</td></tr>
<tr><td><code>ON</code></td><td>Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.</td></tr>
<tr><td><code>FORCE</code></td><td>Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.</td></tr>
<tr><td><code>FORCE_PLUS_PERMANENT</code></td><td>Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with <code><a href="/kb/en/uninstall-soname/">UNINSTALL SONAME</a></code> or <code><a href="/kb/en/uninstall-plugin/">UNINSTALL PLUGIN</a></code> while the server is running.</td></tr>
</tbody></table>

A plugin's status can be found by looking at the `PLUGIN_STATUS` column of the <a undefined>information_schema.PLUGINS</a> table.

## Uninstalling Plugins

Plugins that are found in the mysql.plugin table, that is those that were installed with [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/), [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/) or [mysql_plugin](/clients-utilities/mysql_plugin/) can be uninstalled in one of two ways:

- The [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or the [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) statement while the server is running
- With [mysql_plugin](/clients-utilities/mysql_plugin/) while the server is offline.

Plugins that were enabled as a `--plugin-load` option do not need to be uninstalled. If `--plugin-load` is omitted the next time the server starts, or the plugin is not listed as one of the `--plugin-load` entries, the plugin will not be loaded.

[UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) uninstalls a single installed plugin, while [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) uninstalls all plugins belonging to a given library.

## See Also

- [List of Plugins](/columns-storage-engines-and-plugins/plugins/information-on-plugins/list-of-plugins/)
- [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/)
- [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/)
- [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/)
- [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/)
- [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins/)
- [INFORMATION_SCHEMA.PLUGINS Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/plugins-table-information-schema/)
- [mysql_plugin](/clients-utilities/mysql_plugin/)