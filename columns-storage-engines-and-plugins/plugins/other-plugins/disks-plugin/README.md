# Disks Plugin

##### MariaDB starting with [10.1.32](/kb/en/mariadb-10132-release-notes/)

The `DISKS` plugin was first released in [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/), [MariaDB 10.2.14](/kb/en/mariadb-10214-release-notes/) and [MariaDB 10.1.32](/kb/en/mariadb-10132-release-notes/).

The `DISKS` plugin creates the <a undefined>DISKS</a> table in the [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database. This table shows metadata about disks on the system.

Before [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/) and [MariaDB 10.1.41](/kb/en/mariadb-10141-release-notes/), this plugin did <strong>not</strong> check [user privileges](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/). When it is enabled, <strong>any</strong> user can query the `INFORMATION_SCHEMA.DISKS` table and see all the information it provides.

Since [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/) and [MariaDB 10.1.41](/kb/en/mariadb-10141-release-notes/), it requires the [FILE privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/).

The plugin only works on Linux.

## Installing the Plugin

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). For example:

```sql
INSTALL SONAME 'disks';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = disks
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'disks';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Example

```sql
SELECT * FROM information_schema.DISKS;

+-----------+-------+----------+---------+-----------+
| Disk      | Path  | Total    | Used    | Available |
+-----------+-------+----------+---------+-----------+
| /dev/vda1 | /     | 26203116 | 2178424 |  24024692 |
| /dev/vda1 | /boot | 26203116 | 2178424 |  24024692 |
| /dev/vda1 | /etc  | 26203116 | 2178424 |  24024692 |
+-----------+-------+----------+---------+-----------+
```

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.1</td><td>Stable</td><td><a href="/kb/en/mariadb-1047-release-notes/">MariaDB 10.4.7</a>, <a href="/kb/en/mariadb-10317-release-notes/">MariaDB 10.3.17</a>, <a href="/kb/en/mariadb-10226-release-notes/">MariaDB 10.2.26</a>, <a href="/kb/en/mariadb-10141-release-notes/">MariaDB 10.1.41</a></td></tr>
<tr><td>1.0</td><td>Beta</td><td><a href="/kb/en/mariadb-1036-release-notes/">MariaDB 10.3.6</a>, <a href="/kb/en/mariadb-10214-release-notes/">MariaDB 10.2.14</a>, <a href="/kb/en/mariadb-10132-release-notes/">MariaDB 10.1.32</a></td></tr>
</tbody></table>

## Options

### `disks`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the <a undefined>mysql.plugins</a> table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--disks=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---