# UNINSTALL PLUGIN

## Syntax

```sql
UNINSTALL PLUGIN [IF EXISTS] plugin_name```

## Description

This statement removes a single installed [plugin](/kb/en/mariadb-plugins/). To uninstall the whole library which contains the plugin, use [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/). You cannot uninstall a plugin if any table that uses it is open.

<code class="highlight fixed" style="white-space:pre-wrap">plugin_name</code> must be the name of some plugin that is listed
in the [mysql.plugin](/kb/en/mysqlplugin-table/) table. The server executes the plugin's deinitialization
function and removes the row for the plugin from the `mysql.plugin`
table, so that subsequent server restarts will not load and initialize
the plugin. <code class="highlight fixed" style="white-space:pre-wrap">UNINSTALL PLUGIN</code> does not remove the plugin's
shared library file.

To use <code class="highlight fixed" style="white-space:pre-wrap">UNINSTALL PLUGIN</code>, you must have the
[DELETE](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) privilege for the [mysql.plugin](/kb/en/mysqlplugin-table/) table.

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

#### IF EXISTS

If the `IF EXISTS` clause is used, MariaDB will return a note instead of an error if the plugin does not exist. See [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/).

## Examples

```sql
UNINSTALL PLUGIN example;
```

From [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/):

```sql
UNINSTALL PLUGIN IF EXISTS example;
Query OK, 0 rows affected (0.099 sec)

UNINSTALL PLUGIN IF EXISTS example;
Query OK, 0 rows affected, 1 warning (0.000 sec)

SHOW WARNINGS;
+-------+------+-------------------------------+
| Level | Code | Message                       |
+-------+------+-------------------------------+
| Note  | 1305 | PLUGIN example does not exist |
+-------+------+-------------------------------+
```

## See Also

- [Plugin Overview](/columns-storage-engines-and-plugins/plugins/plugin-overview/)
- [mysql_plugin](/clients-utilities/mysql_plugin/)
- [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/)
- [List of Plugins](/columns-storage-engines-and-plugins/plugins/information-on-plugins/list-of-plugins/)