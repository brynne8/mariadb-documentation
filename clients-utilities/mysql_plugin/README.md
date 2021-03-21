# mysql_plugin

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-plugin` is a symlink to `mysql_plugin`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mysql_plugin` is the symlink, and `mariadb-plugin` the binary name.

`mysql_plugin` is a tool for enabling or disabling [plugins](/kb/en/mariadb-plugins/).

It is a commandline alternative to the [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin) and [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin) statements, and the `--plugin-load option` to [mysqld](/kb/en/mysqld-options-full-list/).

`mysql_plugin` must be run while the server is offline, and works by adding or removing rows from the [mysql.plugin](/kb/en/mysqlplugin-table/) table.

## Usage

```sql
mysql_plugin [options] <plugin> ENABLE|DISABLE
```

`mysql_plugin` expects to find a configuration file that indicates how to configure the plugins. The configuration file is by default the same name as the plugin, with a `.ini` extension. For example:

```sql
mysql_plugin crazyplugins ENABLE
```

Here, `mysql_plugin` will look for a file called `crazyplugins.ini`

```sql
crazyplugins
crazyplugin1
crazyplugin2
crazyplugin3
```

The first line should contain the name of the library object file, with no extension. The other lines list the names of the components. Each value should be on a separate line, and the `#` character at the start of the line indicates a comment.

## Options

The following options can be specified on the command line, while some can be specified in the `[mysqld]` group of any option file. For options specified in a `[mysqld]` group, only the `--basedir`, `--datadir`, and `--plugin-dir` options can be used - the rest are ignored.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-b</code>, <code>--basedir=name</code></td><td>The base directory for the server.</td></tr>
<tr><td><code>-d</code>, <code>--datadir=name</code></td><td>The data directory for the server.</td></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Display help and exit.</td></tr>
<tr><td><code>-f</code>, <code>--my-print-defaults=name</code></td><td>Path to <code>my_print_defaults</code> executable. Example: <code>/source/temp11/extra</code></td></tr>
<tr><td><code>-m</code>, <code>--mysqld=name</code></td><td>Path to <code>mysqld</code> executable. Example: <code>/sbin/temp1/mysql/bin</code></td></tr>
<tr><td><code>-n</code>, <code>--no-defaults</code></td><td>Do not read values from configuration file.</td></tr>
<tr><td><code>-p</code>, <code>--plugin-dir=name</code></td><td>The plugin directory for the server.</td></tr>
<tr><td><code>-i</code>, <code>--plugin-ini=name</code></td><td>Read plugin information from configuration file specified instead of from <code>&lt;plugin-dir&gt;/&lt;plugin_name&gt;.ini</code>.</td></tr>
<tr><td><code>-P</code>, <code>--print-defaults</code></td><td>Show default values from configuration file.</td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>More verbose output; you can use this multiple times to get even more verbose output.</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Output version information and exit.</td></tr>
</tbody></table>

## See Also

- [List of Plugins](/columns-storage-engines-and-plugins/plugins/information-on-plugins/list-of-plugins)
- [Plugin Overview](/columns-storage-engines-and-plugins/plugins/plugin-overview)
- [INFORMATION_SCHEMA.PLUGINS Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/plugins-table-information-schema)
- [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin)
- [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname)
- [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin)
- [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname)