# Master Thread States

This article documents thread states that are related to [replication](/replication) master threads. These correspond to the `STATE` values listed by the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist) statement or in the [Information Schema PROCESSLIST Table](/kb/en/information-schema-processlist-table/) as well as the `PROCESSLIST_STATE` value listed in the [Performance Schema threads Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table).

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td>Finished reading one binlog; switching to next binlog</td><td>After completing one <a href="/kb/en/binary-log/">binary log</a>, the next is being opened for sending to the slave.</td></tr>
<tr><td>Master has sent all binlog to slave; waiting for binlog to be updated</td><td>All events have been read from the binary logs and sent to the slave. Now waiting for the binary log to be updated with new events.</td></tr>
<tr><td>Sending binlog event to slave</td><td>An event has been read from the <a href="/kb/en/binary-log/">binary log</a>, and is now being sent to the slave.</td></tr>
<tr><td>Waiting to finalize termination</td><td>State that only occurs very briefly while the thread is terminating.</td></tr>
</tbody></table>