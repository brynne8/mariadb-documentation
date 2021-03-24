# mysql.table_stats Table

##### MariaDB starting with [10.0.1](/kb/en/mariadb-1001-release-notes/)

The `mysql.table_stats` table was introduced as part of the [Engine-independent table statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics/) feature implemented in [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/).

The `mysql.table_stats` table is one of three tables storing data used for [Engine-independent table statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics/) added in [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/). The others are [mysql.column_stats](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlcolumn_stats-table/) and [mysql.index_stats](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlindex_stats-table/).

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.table_stats` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>db_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Database the table is in .</td></tr>
<tr><td><code>table_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Table name.</td></tr>
<tr><td><code>cardinality</code></td><td><code>bigint(21) unsigned</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>Number of records in the table.</td></tr>
</tbody></table>

It is possible to manually update the table. See [Manual updates to statistics tables](/kb/en/engine-independent-table-statistics/#manual-updates-to-statistics-tables) for details.