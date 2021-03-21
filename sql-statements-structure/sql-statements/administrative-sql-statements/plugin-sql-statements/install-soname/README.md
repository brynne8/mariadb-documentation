# INSTALL SONAME

## Syntax

```sql
INSTALL SONAME 'plugin_library'```

## Description

This statement is a variant of [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). It installs <strong>all</strong> [plugins](/kb/en/mariadb-plugins/) from a given `plugin_library`. See [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/) for details.

<code class="highlight fixed" style="white-space:pre-wrap">plugin_library</code> is the name of the shared library that
contains the plugin code. The file name extension (for
example, `libmyplugin.so` or `libmyplugin.dll`) can be omitted (which makes the statement look the same on all architectures).

The shared library must be located in the plugin directory (that is,
the directory named by the <a undefined>plugin_dir</a> system variable). The library must be in the plugin directory itself, not in a subdirectory. By
default, `plugin_dir` is plugin directory under the directory named by
the `pkglibdir` configuration variable, but it can be changed by setting
the value of <code class="highlight fixed" style="white-space:pre-wrap">plugin_dir</code> at server startup. For example, set
its value in a `my.cnf` file:

```sql
[mysqld]
plugin_dir=/path/to/plugin/directory```

If the value of [plugin_dir](/kb/en/server-system-variables/#plugin_dir) is a relative path name, it is
taken to be relative to the MySQL base directory (the value of the `basedir`
system variable).

<code class="highlight fixed" style="white-space:pre-wrap">INSTALL SONAME</code> adds one or more lines to the `mysql.plugin` table that
describes the plugin. This table contains the plugin name and library file
name.

<code class="highlight fixed" style="white-space:pre-wrap">INSTALL SONAME</code> causes the server to read
option (`my.cnf`) files just as during server startup. This enables the plugin to
pick up any relevant options from those files. It is possible to add plugin
options to an option file even before loading a plugin (if the loose prefix is
used). It is also possible to uninstall a plugin, edit `my.cnf`, and install the
plugin again. Restarting the plugin this way enables it to the new option
values without a server restart.

<code class="highlight fixed" style="white-space:pre-wrap">INSTALL SONAME</code> also loads and initializes the plugin code to
make the plugin available for use. A plugin is initialized by executing its
initialization function, which handles any setup that the plugin must perform
before it can be used.

To use <code class="highlight fixed" style="white-space:pre-wrap">INSTALL SONAME</code>, you must have the
[INSERT privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for the `mysql.plugin` table.

At server startup, the server loads and initializes any plugin that is
listed in the `mysql.plugin` table. This means that a plugin is installed
with <code class="fixed" style="white-space:pre-wrap">INSTALL SONAME</code> only once, not every time the server
starts. Plugin loading at startup does not occur if the server is started with
the <code class="fixed" style="white-space:pre-wrap">--skip-grant-tables</code> option.

When the server shuts down, it executes the de-initialization function
for each plugin that is loaded so that the plugin has a chance to
perform any final cleanup.

If you need to load plugins for a single server startup when the
<code class="highlight fixed" style="white-space:pre-wrap">--skip-grant-tables</code> option is given (which tells the server
not to read system tables), use the 
<code class="highlight fixed" style="white-space:pre-wrap">--plugin-load</code> [mysqld option](/kb/en/mysqld-options-full-list/).

If you need to install only one plugin from a library, use the [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/) statement.

## Examples

To load the XtraDB storage engine and all of its `information_schema` tables with one statement, use

```sql
INSTALL SONAME 'ha_xtradb';
```

This statement can be used instead of `INSTALL PLUGIN` even when the library contains only one plugin:

```sql
INSTALL SONAME 'ha_sequence';
```

## See Also

- [List of Plugins](/columns-storage-engines-and-plugins/plugins/information-on-plugins/list-of-plugins/)
- [Plugin Overview](/columns-storage-engines-and-plugins/plugins/plugin-overview/)
- [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins/)
- [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/)
- [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/)
- [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/)
- [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins/)
- [INFORMATION_SCHEMA.PLUGINS Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/plugins-table-information-schema/)
- [mysql_plugin](/clients-utilities/mysql_plugin/)