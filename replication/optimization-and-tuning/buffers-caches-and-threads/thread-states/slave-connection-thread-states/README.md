# Slave Connection Thread States

This article documents thread states that are related to connection threads that occur on a [replication](/replication) slave. These correspond to the `STATE` values listed by the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist) statement or in the [Information Schema PROCESSLIST Table](/kb/en/information-schema-processlist-table/) as well as the `PROCESSLIST_STATE` value listed in the [Performance Schema threads Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table).

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td>Changing master</td><td>Processing a <a href="/kb/en/change-master-to/">CHANGE MASTER TO</a> statement.</td></tr>
<tr><td>Killing slave</td><td>Processing a <a href="/kb/en/stop-slave/">STOP SLAVE</a> statement.</td></tr>
<tr><td>Opening master dump table</td><td>A table has been created from a master dump and is now being opened.</td></tr>
<tr><td>Reading master dump table data</td><td>After the table created by a master dump (the <code>Opening master dump table</code> state) the table is now being read.</td></tr>
<tr><td>Rebuilding the index on master dump table</td><td>After the table created by a master dump has been opened and read (the <code>Reading master dump table data</code> state), the index is built.</td></tr>
</tbody></table>