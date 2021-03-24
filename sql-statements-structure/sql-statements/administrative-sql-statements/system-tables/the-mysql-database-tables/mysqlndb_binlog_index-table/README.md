# mysql.ndb_binlog_index Table

##### MariaDB until [10.0.3](/kb/en/mariadb-1003-release-notes/)

The `mysql.ndb_binlog_index` table is not used by MariaDB. It was kept for MySQL compatibility reasons, and is used there for MySQL Cluster. It was removed in [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/).

For MariaDB clustering, see [Galera](/kb/en/galera/).

The table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th></tr>
<tr><td><code>Position</code></td><td><code>bigint(20) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td></tr>
<tr><td><code>File</code></td><td><code>varchar(255)</code></td><td>NO</td><td></td><td><code>NULL</code></td></tr>
<tr><td><code>epoch</code></td><td><code>bigint(20) unsigned</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td></tr>
<tr><td><code>inserts</code></td><td><code>bigint(20) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td></tr>
<tr><td><code>updates</code></td><td><code>bigint(20) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td></tr>
<tr><td><code>deletes</code></td><td><code>bigint(20) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td></tr>
<tr><td><code>schemaops</code></td><td><code>bigint(20) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td></tr>
</tbody></table>