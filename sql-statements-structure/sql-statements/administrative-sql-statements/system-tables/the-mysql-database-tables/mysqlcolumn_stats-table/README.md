# mysql.column_stats Table

##### MariaDB starting with [10.0.1](/kb/en/mariadb-1001-release-notes/)

The `mysql.column_stats` table was introduced as part of the [Engine-independent table statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics/) feature implemented in [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/).

The `mysql.column_stats` table is one of three tables storing data used for [Engine-independent table statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics/) added in [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/). The others are [mysql.table_stats](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqltable_stats-table/) and [mysql.index_stats](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlindex_stats-table/).The histogram fields were added in [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/) as part of the [Histogram-based Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/histogram-based-statistics/) feature.

Note that statistics for blob and text columns are not collected. If explicitly specified, a warning is returned.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.column_stats` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>db_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Database the table is in.</td></tr>
<tr><td><code>table_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Table name.</td></tr>
<tr><td><code>column_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Name of the column.</td></tr>
<tr><td><code>min_value</code></td><td><code>varchar(255)</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>Minimum value in the table (in text form).</td></tr>
<tr><td><code>max_value</code></td><td><code>varchar(255)</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>Maximum value in the table (in text form).</td></tr>
<tr><td><code>nulls_ratio</code></td><td><code>decimal(12,4)</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>Fraction of <code>NULL</code> values (0- no <code>NULL</code>s, 0.5 - half values are <code>NULL</code>s, 1 - all values are <code>NULL</code>s).</td></tr>
<tr><td><code>avg_length</code></td><td><code>decimal(12,4)</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>Average length of column value, in bytes. Counted as if one ran <code>SELECT AVG(LENGTH(col))</code>. This doesn't count <code>NULL</code> bytes, assumes endspace removal for <code>CHAR(n)</code>, etc.</td></tr>
<tr><td><code>avg_frequency</code></td><td><code>decimal(12,4)</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>Average number of records with the same value</td></tr>
<tr><td><code>hist_size</code></td><td><code>tinyint(3) unsigned</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>Histogram size in bytes, from 0-255.</td></tr>
<tr><td><code>hist_type</code></td><td><code>enum('SINGLE_PREC_HB', 'DOUBLE_PREC_HB')</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>Histogram type. See the <a href="/kb/en/server-system-variables/#histogram_type">histogram_type</a> system variable.</td></tr>
<tr><td><code>histogram</code></td><td><code>varbinary(255)</code></td><td>YES</td><td></td><td><code>NULL</code></td><td></td></tr>
</tbody></table>

It is possible to manually update the table. See [Manual updates to statistics tables](/kb/en/engine-independent-table-statistics/#manual-updates-to-statistics-tables) for details.