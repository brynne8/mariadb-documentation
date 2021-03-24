# INSTALL PLUGIN

## Syntax

```sql
INSTALL PLUGIN [IF NOT EXISTS] plugin_name SONAME 'plugin_library'
```

## Description

This statement installs an individual [plugin](/kb/en/mariadb-plugins/) from the specified library. To install the whole library (which could be required), use [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/). See also [Installing a Plugin](/kb/en/plugin-overview/#installing-a-plugin).

`plugin_name` is the name of the plugin as defined in the
plugin declaration structure contained in the library file. Plugin names are
not case sensitive. For maximal compatibility, plugin names should be limited
to ASCII letters, digits, and underscore, because they are used in C source
files, shell command lines, M4 and Bourne shell scripts, and SQL environments.

`plugin_library` is the name of the shared library that
contains the plugin code. The file name extension can be omitted (which makes the statement look the same on all architectures).

The shared library must be located in the plugin directory (that is,
the directory named by the [plugin_dir](/kb/en/server-system-variables/#plugin_dir) system variable). The library must be in the plugin directory itself, not in a subdirectory. By
default, `plugin_dir` is plugin directory under the directory named by
the `pkglibdir` configuration variable, but it can be changed by setting
the value of <code class="highlight fixed" style="white-space:pre-wrap">plugin_dir</code> at server startup. For example, set
its value in a `my.cnf` file:

```sql
[mysqld]
plugin_dir=/path/to/plugin/directory
```

If the value of [plugin_dir](/kb/en/server-system-variables/#plugin_dir) is a relative path name, it is
taken to be relative to the MySQL base directory (the value of the basedir
system variable).

`INSTALL PLUGIN` adds a line to the `mysql.plugin` table that
describes the plugin. This table contains the plugin name and library file
name.

`INSTALL PLUGIN` causes the server to read
option (`my.cnf`) files just as during server startup. This enables the plugin to
pick up any relevant options from those files. It is possible to add plugin
options to an option file even before loading a plugin (if the loose prefix is
used). It is also possible to uninstall a plugin, edit `my.cnf`, and install the
plugin again. Restarting the plugin this way enables it to the new option
values without a server restart.

`INSTALL PLUGIN` also loads and initializes the plugin code to
make the plugin available for use. A plugin is initialized by executing its
initialization function, which handles any setup that the plugin must perform
before it can be used.

To use `INSTALL PLUGIN`, you must have the
[INSERT privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for the `mysql.plugin` table.

At server startup, the server loads and initializes any plugin that is
listed in the `mysql.plugin` table. This means that a plugin is installed
with `INSTALL PLUGIN` only once, not every time the server
starts. Plugin loading at startup does not occur if the server is started with
the <code class="fixed" style="white-space:pre-wrap">--skip-grant-tables</code> option.

When the server shuts down, it executes the de-initialization function
for each plugin that is loaded so that the plugin has a chance to
perform any final cleanup.

If you need to load plugins for a single server startup when the
<code class="highlight fixed" style="white-space:pre-wrap">--skip-grant-tables</code> option is given (which tells the server
not to read system tables), use the 
<code class="highlight fixed" style="white-space:pre-wrap">--plugin-load</code> [mysqld option](/kb/en/mysqld-options-full-list/).

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

#### IF NOT EXISTS

When the `IF NOT EXISTS` clause is used, MariaDB will return a note instead of an error if the specified plugin already exists. See [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/).

## Examples

```sql
INSTALL PLUGIN sphinx SONAME 'ha_sphinx.so';
```

The extension can also be omitted:

```sql
INSTALL PLUGIN innodb SONAME 'ha_xtradb';
```

From [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/):

```sql
INSTALL PLUGIN IF NOT EXISTS example SONAME 'ha_example';
Query OK, 0 rows affected (0.104 sec)

INSTALL PLUGIN IF NOT EXISTS example SONAME 'ha_example';
Query OK, 0 rows affected, 1 warning (0.000 sec)

SHOW WARNINGS;
+-------+------+------------------------------------+
| Level | Code | Message                            |
+-------+------+------------------------------------+
| Note  | 1968 | Plugin 'example' already installed |
+-------+------+------------------------------------+
```

## See Also

- [List of Plugins](/columns-storage-engines-and-plugins/plugins/information-on-plugins/list-of-plugins/)
- [Plugin Overview](/columns-storage-engines-and-plugins/plugins/plugin-overview/)
- [INFORMATION_SCHEMA.PLUGINS Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/plugins-table-information-schema/)
- [mysql_plugin](/clients-utilities/mysql_plugin/)
- [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins/)
- [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/)
- [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/)
- [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/)