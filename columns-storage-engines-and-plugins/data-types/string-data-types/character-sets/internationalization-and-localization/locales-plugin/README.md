# Locales Plugin

##### MariaDB starting with [10.0.4](/kb/en/mariadb-1004-release-notes/)

The `LOCALES` plugin was first released in [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/).

The `LOCALES` plugin creates the <a undefined>LOCALES</a> table in the [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database. The plugin also adds the [SHOW LOCALES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-locales/) statement.The table and statement can be queried to see all [locales](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale/) that are compiled into the server.

## Installing the Plugin

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). For example:

```sql
INSTALL SONAME 'locales';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = locales
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'locales';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Example

```sql
SELECT * FROM INFORMATION_SCHEMA.LOCALES;
+-----+-------+-------------------------------------+-----------------------+---------------------+---------------+--------------+------------------------+
| ID  | NAME  | DESCRIPTION                         | MAX_MONTH_NAME_LENGTH | MAX_DAY_NAME_LENGTH | DECIMAL_POINT | THOUSAND_SEP | ERROR_MESSAGE_LANGUAGE |
+-----+-------+-------------------------------------+-----------------------+---------------------+---------------+--------------+------------------------+
|   0 | en_US | English - United States             |                     9 |                   9 | .             | ,            | english                |
|   1 | en_GB | English - United Kingdom            |                     9 |                   9 | .             | ,            | english                |
|   2 | ja_JP | Japanese - Japan                    |                     3 |                   3 | .             | ,            | japanese               |
|   3 | sv_SE | Swedish - Sweden                    |                     9 |                   7 | ,             |              | swedish                |
|   4 | de_DE | German - Germany                    |                     9 |                  10 | ,             | .            | german                 |
|   5 | fr_FR | French - France                     |                     9 |                   8 | ,             |              | french                 |
|   6 | ar_AE | Arabic - United Arab Emirates       |                     6 |                   8 | .             | ,            | english                |
|   7 | ar_BH | Arabic - Bahrain                    |                     6 |                   8 | .             | ,            | english                |
|   8 | ar_JO | Arabic - Jordan                     |                    12 |                   8 | .             | ,            | english                |
...
| 106 | no_NO | Norwegian - Norway                  |                     9 |                   7 | ,             | .            | norwegian              |
| 107 | sv_FI | Swedish - Finland                   |                     9 |                   7 | ,             |              | swedish                |
| 108 | zh_HK | Chinese - Hong Kong SAR             |                     3 |                   3 | .             | ,            | english                |
| 109 | el_GR | Greek - Greece                      |                    11 |                   9 | ,             | .            | greek                  |
| 110 | rm_CH | Romansh - Switzerland               |                     9 |                   9 | ,             | .            | english                |
+-----+-------+-------------------------------------+-----------------------+---------------------+---------------+--------------+------------------------+
```

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a></td></tr>
<tr><td>1.0</td><td>Gamma</td><td><a href="/kb/en/mariadb-10010-release-notes/">MariaDB 10.0.10</a></td></tr>
<tr><td>1.0</td><td>Alpha</td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
</tbody></table>

## Options

### `locales`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the [mysql.plugins](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlplugin-table/) table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--locales=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---