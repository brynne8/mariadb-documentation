# Delayed Insert Connection Thread States

This article documents thread states that are related to the connection thread that processes [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/) statements.

These correspond to the `STATE` values listed by the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) statement or in the [Information Schema PROCESSLIST Table](/kb/en/information-schema-processlist-table/) as well as the `PROCESSLIST_STATE` value listed in the [Performance Schema threads Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table/).

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td>allocating local table</td><td>Preparing to allocate rows to the delayed-insert handler thread. Follows from the <code>got handler lock</code> state.</td></tr>
<tr><td>Creating delayed handler</td><td>Creating a handler for the delayed-inserts.</td></tr>
<tr><td>got handler lock</td><td>Lock to access the delayed-insert handler thread has been received. Follows from the <code>waiting for handler lock</code> state and before the <code>allocating local table</code> state.</td></tr>
<tr><td>got old table</td><td>The initialization phase is over. Follows from the <code>waiting for handler open</code> state.</td></tr>
<tr><td>storing row into queue</td><td>Adding new row to the list of rows to be inserted by the delayed-insert handler thread.</td></tr>
<tr><td>waiting for delay_list</td><td>Initializing (trying to find the delayed-insert handler thread).</td></tr>
<tr><td>waiting for handler insert</td><td>Waiting for new inserts, as all inserts have been processed.</td></tr>
<tr><td>waiting for handler lock</td><td>Waiting for delayed insert-handler lock to access the delayed-insert handler thread.</td></tr>
<tr><td>waiting for handler open</td><td>Waiting for the delayed-insert handler thread to initialize. Follows from the <code>Creating delayed handler</code> state and before the <code>got old table</code> state.</td></tr>
</tbody></table>