# UNINSTALL SONAME

## Syntax

```sql
UNINSTALL SONAME  [IF EXISTS] 'plugin_library'```

## Description

This statement is a variant of [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin) statement, that removes all [plugins](/kb/en/mariadb-plugins/) belonging to a specified `plugin_library`. See [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin) for details.

<code class="highlight fixed" style="white-space:pre-wrap">plugin_library</code> is the name of the shared library that
contains the plugin code. The file name extension (for
example, `libmyplugin.so` or `libmyplugin.dll`) can be omitted (which makes the statement look the same on all architectures).

To use <code class="highlight fixed" style="white-space:pre-wrap">UNINSTALL SONAME</code>, you must have the
[DELETE privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) for the `mysql.plugin` table.

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

#### IF EXISTS

If the `IF EXISTS` clause is used, MariaDB will return a note instead of an error if the plugin library does not exist. See [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings).

## Examples

To uninstall the XtraDB plugin and all of its `information_schema` tables with one statement, use

```sql
UNINSTALL SONAME 'ha_xtradb';
```

From [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/):

```sql
UNINSTALL SONAME IF EXISTS 'ha_example';
Query OK, 0 rows affected (0.099 sec)

UNINSTALL SONAME IF EXISTS 'ha_example';
Query OK, 0 rows affected, 1 warning (0.000 sec)

SHOW WARNINGS;
+-------+------+-------------------------------------+
| Level | Code | Message                             |
+-------+------+-------------------------------------+
| Note  | 1305 | SONAME ha_example.so does not exist |
+-------+------+-------------------------------------+
```

## See Also

- [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname)
- [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins)
- [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin)
- [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin)
- [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins)
- [INFORMATION_SCHEMA.PLUGINS Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/plugins-table-information-schema)
- [mysql_plugin](/clients-utilities/mysql_plugin)
- [List of Plugins](/columns-storage-engines-and-plugins/plugins/information-on-plugins/list-of-plugins)