# Slave SQL Thread States

This article documents thread states that are related to [replication](/replication) slave SQL threads. These correspond to the `Slave_SQL_State` shown by [SHOW SLAVE STATUS](/kb/en/show-slave-status/) as well as the `STATE` values listed by the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist) statement and the [Information Schema PROCESSLIST](/kb/en/information-schema-processlist-table/) as well as the `PROCESSLIST_STATE` value listed in the [Performance Schema threads Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table).

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td>Apply log event</td><td>Log event is being applied.</td></tr>
<tr><td>Making temp file</td><td>Creating a temporary file containing the row data as part of a <a href="/kb/en/load-data-infile/">LOAD DATA INFILE</a> statement.</td></tr>
<tr><td>Reading event from the relay log</td><td>Reading an event from the <a href="/kb/en/relay-log/">relay log</a> in order to process the event.</td></tr>
<tr><td>Slave has read all relay log; waiting for the slave I/O thread to update it</td><td>All <a href="/kb/en/relay-log/">relay log</a> events have been processed, now waiting for the I/O thread to write new events to the relay log.</td></tr>
<tr><td>Waiting for work from SQL thread</td><td>&nbsp;In parallel replication the worker thread is waiting for more things from the SQL thread.</td></tr>
<tr><td>Waiting for prior transaction to start commit before starting next transaction</td><td>In parallel replication the worker thread is waiting for conflicting things to end before starting executing.</td></tr>
<tr><td>Waiting for worker threads to be idle</td><td>&nbsp;Happens in parallel replication when moving to a new binary log after a master restart. All slave temporary files are deleted and worker threads are restarted.</td></tr>
<tr><td>Waiting due to global read lock</td><td>In parallel replication when worker threads are waiting for a global read lock to be released.</td></tr>
<tr><td>Waiting for worker threads to pause for global read lock</td><td><a href="/kb/en/flush/">FLUSH TABLES WITH READ LOCK</a> is waiting for worker threads to finish what they are doing.</td></tr>
<tr><td>Waiting while replication worker thread pool is busy</td><td>Happens in parallel replication during a <a href="/kb/en/flush/">FLUSH TABLES WITH READ LOCK</a> or when changing number of parallel workers.</td></tr>
<tr><td>Waiting for other master connection to process GTID received on multiple master connections</td><td>&nbsp;A worker thread noticed that there is already another thread executing the same GTID from another connection and it's waiting for the other to complete.</td></tr>
<tr><td>Waiting for slave mutex on exit</td><td>Thread is stopping. Only occurs very briefly.</td></tr>
<tr><td>Waiting for the next event in relay log</td><td>State before reading next event from the <a href="/kb/en/relay-log/">relay log</a>.</td></tr>
</tbody></table>