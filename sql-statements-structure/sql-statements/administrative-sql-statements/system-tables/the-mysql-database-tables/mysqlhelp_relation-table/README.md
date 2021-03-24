# mysql.help_relation Table

`mysql.help_relation` is one of the four tables used by the [HELP command](/sql-statements-structure/sql-statements/administrative-sql-statements/help-command/). It is populated when the server is installed by the `fill_help_table.sql` script. The other help tables are [help_topic](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlhelp_topic-table/), [help_category](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlhelp_category-table/) and [help_keyword](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlhelp_keyword-table/).

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.help_relation` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>help_topic_id</code></td><td><code>int(10) unsigned</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td></td></tr>
<tr><td><code>help_keyword_id</code></td><td><code>int(10) unsigned</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td></td></tr>
</tbody></table>

## Example

```sql
...
|           106 |             456 |
|           463 |             456 |
|           468 |             456 |
|           463 |             457 |
|           194 |             458 |
|           478 |             458 |
|           374 |             459 |
|           459 |             459 |
|            39 |             460 |
|            58 |             460 |
|           185 |             460 |
|           264 |             460 |
|           269 |             460 |
|           209 |             461 |
|           468 |             461 |
|           201 |             462 |
|           468 |             463 |
+---------------+-----------------+
```