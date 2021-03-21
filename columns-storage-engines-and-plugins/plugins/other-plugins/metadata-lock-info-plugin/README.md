# METADATA_LOCK_INFO Plugin

##### MariaDB starting with [10.0.7](/kb/en/mariadb-1007-release-notes/)

The `METADATA_LOCK_INFO` plugin was first released in [MariaDB 10.0.7](/kb/en/mariadb-1007-release-notes/).

The `METADATA_LOCK_INFO` plugin creates the <a undefined>METADATA_LOCK_INFO</a> table in the [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database. This table shows active [metadata locks](/sql-statements-structure/sql-statements/transactions/metadata-locking/). The table will be empty if there are no active metadata locks.

## Installing the Plugin

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). For example:

```sql
INSTALL SONAME 'metadata_lock_info';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = metadata_lock_info
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'metadata_lock_info';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Examples

### Viewing all Metadata Locks

```sql
SELECT * FROM information_schema.metadata_lock_info;  
+-----------+--------------------------+---------------+----------------------+-----------------+-------------+
| THREAD_ID | LOCK_MODE                | LOCK_DURATION | LOCK_TYPE            | TABLE_SCHEMA    | TABLE_NAME  |  
+-----------+--------------------------+---------------+----------------------+-----------------+-------------+
|        31 | MDL_INTENTION_EXCLUSIVE  | MDL_EXPLICIT  | Global read lock     |                 |             |  
|        31 | MDL_INTENTION_EXCLUSIVE  | MDL_EXPLICIT  | Commit lock          |                 |             |
|        31 | MDL_INTENTION_EXCLUSIVE  | MDL_EXPLICIT  | Schema metadata lock | dbname          |             |
|        31 | MDL_SHARED_NO_READ_WRITE | MDL_EXPLICIT  | Table metadata lock  | dbname          | exotics     |
+-----------+--------------------------+---------------+----------------------+-----------------+-------------+
4 rows in set (0.00 sec)
```

### Matching Metadata Locks with Threads and Queries

```sql
SELECT 
CONCAT('Thread ',P.ID,' executing "',P.INFO,'" IS LOCKED BY Thread ',
M.THREAD_ID) WhoLocksWho 
FROM INFORMATION_SCHEMA.PROCESSLIST P,
INFORMATION_SCHEMA.METADATA_LOCK_INFO M 
WHERE LOCATE(lcase(LOCK_TYPE), lcase(STATE))>0;
+-----------------------------------------------------------------------------------+
| WhoLocksWho                                                                       |
+-----------------------------------------------------------------------------------+
| Thread 3 executing "INSERT INTO foo ( b ) VALUES ( 'FOO' )" IS LOCKED BY Thread 2 |
+-----------------------------------------------------------------------------------+
1 row in set (0.00 sec)

SHOW PROCESSLIST;
+----+------+-----------+------+---------+------+------------------------------+----------------------------------------+----------+
| Id | User | Host      | db   | Command | Time | State                        | Info                                   | Progress |
+----+------+-----------+------+---------+------+------------------------------+----------------------------------------+----------+
|  2 | root | localhost | test | Sleep   |  123 |                              | NULL                                   |    0.000 |
|  3 | root | localhost | test | Query   |  103 | Waiting for global read lock | INSERT INTO foo ( b ) VALUES ( 'FOO' ) |    0.000 |
|  4 | root | localhost | test | Query   |    0 | init                         | SHOW PROCESSLIST                       |    0.000 |
+----+------+-----------+------+---------+------+------------------------------+----------------------------------------+----------+
3 rows in set (0.00 sec)
```

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>0.1</td><td>Stable</td><td><a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a></td></tr>
<tr><td>0.1</td><td>Beta</td><td><a href="/kb/en/mariadb-10010-release-notes/">MariaDB 10.0.10</a></td></tr>
<tr><td>0.1</td><td>Alpha</td><td><a href="/kb/en/mariadb-1007-release-notes/">MariaDB 10.0.7</a></td></tr>
</tbody></table>

## Options

### `metadata_lock_info`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the <a undefined>mysql.plugins</a> table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--metadata-lock-info=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---