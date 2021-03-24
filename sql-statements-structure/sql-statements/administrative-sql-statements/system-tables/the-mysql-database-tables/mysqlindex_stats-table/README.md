# mysql.index_stats Table

##### MariaDB starting with [10.0.1](/kb/en/mariadb-1001-release-notes/)

The `mysql.index_stats` table was introduced as part of the [Engine-independent table statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics/) feature implemented in [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/).

The `mysql.index_stats` table is one of three tables storing data used for [Engine-independent table statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics/) added in [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/). The others are [mysql.column_stats](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlcolumn_stats-table/) and [mysql.table_stats](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqltable_stats-table/).

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.index_stats` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>db_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Database the table is in.</td></tr>
<tr><td><code>table_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Table name</td></tr>
<tr><td><code>index_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Name of the index</td></tr>
<tr><td><code>prefix_arity</code></td><td><code>int(11) unsigned</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Index prefix length. 1 for the first keypart, 2 for the first two, and so on. InnoDB's extended keys are supported.</td></tr>
<tr><td><code>avg_frequency</code></td><td><code>decimal(12,4)</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>Average number of records one will find for given values of (keypart1, keypart2, ..), provided the values will be found in the table.</td></tr>
</tbody></table>

It is possible to manually update the table. See [Manual updates to statistics tables](/kb/en/engine-independent-table-statistics/#manual-updates-to-statistics-tables) for details.