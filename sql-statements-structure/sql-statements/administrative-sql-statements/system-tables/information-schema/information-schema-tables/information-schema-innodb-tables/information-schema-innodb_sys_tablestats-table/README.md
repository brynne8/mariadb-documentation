# Information Schema INNODB_SYS_TABLESTATS Table

The [Information Schema](/kb/en/information_schema/) `INNODB_SYS_TABLESTATS` table contains InnoDB status information.  It can be used for developing new performance-related extensions, or high-level performance monitoring.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

Note that the MySQL InnoDB and Percona XtraDB versions of the tables differ (see [XtraDB and InnoDB](/kb/en/xtradb-and-innodb/)).

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>TABLE_ID</code></td><td>Table ID, matching the <a href="/kb/en/information-schema-innodb_sys_tables-table/">INNODB_SYS_TABLES.TABLE_ID</a> value.</td></tr>
<tr><td><code>SCHEMA</code></td><td>Database name (XtraDB only).</td></tr>
<tr><td><code>NAME</code></td><td>Table name, matching the <a href="/kb/en/information-schema-innodb_sys_tables-table/">INNODB_SYS_TABLES.NAME</a> value.</td></tr>
<tr><td><code>STATS_INITIALIZED</code></td><td><code>Initialized</code> if statistics have already been collected, otherwise <code>Uninitialized</code>.</td></tr>
<tr><td><code>NUM_ROWS</code></td><td>Estimated number of rows currently in the table. Updated after each statement modifying the data, but uncommited transactions mean it may not be accurate.</td></tr>
<tr><td><code>CLUST_INDEX_SIZE</code></td><td>Number of pages on disk storing the clustered index, holding InnoDB table data in primary key order, or <code>NULL</code> if not statistics yet collected.</td></tr>
<tr><td><code>OTHER_INDEX_SIZE</code></td><td>Number of pages on disk storing secondary indexes for the table, or <code>NULL</code> if not statistics yet collected.</td></tr>
<tr><td><code>MODIFIED_COUNTER</code></td><td>Number of rows modified by statements modifying data.</td></tr>
<tr><td><code>AUTOINC</code></td><td><a href="/kb/en/auto_increment/">Auto_increment</a> value.</td></tr>
<tr><td><code>REF_COUNT</code></td><td>Countdown to zero, when table metadata can be removed from the table cache. (InnoDB only)</td></tr>
<tr><td><code>MYSQL_HANDLES_OPENED</code></td><td>(XtraDB only).</td></tr>
</tbody></table>