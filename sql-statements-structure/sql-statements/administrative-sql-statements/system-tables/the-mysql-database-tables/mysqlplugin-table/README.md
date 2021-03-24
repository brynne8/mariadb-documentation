# mysql.plugin Table

The `mysql.plugin` table can be queried to get information about installed [plugins](/columns-storage-engines-and-plugins/plugins/).

This table only contains information about [plugins](/columns-storage-engines-and-plugins/plugins/) that have been installed via the following methods:

- The [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) statement.
- The [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/) statement.
- The [mysql_plugin](/clients-utilities/mysql_plugin/) utility.

This table does not contain information about:

- Built-in plugins.
- Plugins loaded with the <a undefined>--plugin-load-add</a> option.
- Plugins loaded with the <a undefined>--plugin-load</a> option.

This table only contains enough information to reload the plugin when the server is restarted, which means it only contains the plugin name and the plugin library.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.plugin` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td></td><td>Plugin name.</td></tr>
<tr><td><code>dl</code></td><td><code>varchar(128)</code></td><td>NO</td><td></td><td></td><td>Name of the plugin library.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM mysql.plugin;
+---------------------------+------------------------+
| name                      | dl                     |
+---------------------------+------------------------+
| spider                    | ha_spider.so           |
| spider_alloc_mem          | ha_spider.so           |
| METADATA_LOCK_INFO        | metadata_lock_info.so  |
| OQGRAPH                   | ha_oqgraph.so          |
| cassandra                 | ha_cassandra.so        |
| QUERY_RESPONSE_TIME       | query_response_time.so |
| QUERY_RESPONSE_TIME_AUDIT | query_response_time.so |
| LOCALES                   | locales.so             |
| sequence                  | ha_sequence.so         |
+---------------------------+------------------------+
```