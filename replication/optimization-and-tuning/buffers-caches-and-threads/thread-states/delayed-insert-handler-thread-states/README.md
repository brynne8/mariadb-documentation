# Delayed Insert Handler Thread States

This article documents thread states that are related to the handler thread that inserts the results of [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed) statements.

These correspond to the `STATE` values listed by the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist) statement or in the [Information Schema PROCESSLIST Table](/kb/en/information-schema-processlist-table/) as well as the `PROCESSLIST_STATE` value listed in the [Performance Schema threads Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table).

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td>insert</td><td>About to insert rows into the table.</td></tr>
<tr><td>reschedule</td><td>Sleeping in order to let other threads function, after inserting a number of rows into the table.</td></tr>
<tr><td>upgrading lock</td><td>Attempting to get lock on the table in order to insert rows.</td></tr>
<tr><td>Waiting for INSERT</td><td>Waiting for the delayed-insert connection thread to add rows to the queue.</td></tr>
</tbody></table>