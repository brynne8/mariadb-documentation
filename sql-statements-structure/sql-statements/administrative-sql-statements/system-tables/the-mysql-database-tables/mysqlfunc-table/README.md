# mysql.func Table

The `mysql.func` table stores information about [user-defined functions](/programming-customizing-mariadb/user-defined-functions) (UDFs) created with the [CREATE FUNCTION UDF](/programming-customizing-mariadb/user-defined-functions/create-function-udf) statement.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine) storage engine.

The `mysql.func` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>name</code></td><td><code>char(64)</code></td><td>NO</td><td>PRI</td><td></td><td>UDF name</td></tr>
<tr><td><code>ret</code></td><td><code>tinyint(1)</code></td><td>NO</td><td></td><td>0</td><td></td></tr>
<tr><td><code>dl</code></td><td><code>char(128)</code></td><td>NO</td><td></td><td></td><td>Shared library name</td></tr>
<tr><td><code>type</code></td><td><code>enum('function','aggregate')</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Type, either <code>function</code> or <code>aggregate</code>. Aggregate functions are summary functions such as <a href="/kb/en/sum/">SUM()</a> and <a href="/kb/en/avg/">AVG()</a>.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM mysql.func;
+------------------------------+-----+--------------+-----------+
| name                         | ret | dl           | type      |
+------------------------------+-----+--------------+-----------+
| spider_direct_sql            |   2 | ha_spider.so | function  |
| spider_bg_direct_sql         |   2 | ha_spider.so | aggregate |
| spider_ping_table            |   2 | ha_spider.so | function  |
| spider_copy_tables           |   2 | ha_spider.so | function  |
| spider_flush_table_mon_cache |   2 | ha_spider.so | function  |
+------------------------------+-----+--------------+-----------+
```