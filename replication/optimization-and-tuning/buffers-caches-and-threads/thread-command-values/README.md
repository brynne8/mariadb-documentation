# Thread Command Values

A thread can have any of the following `COMMAND` values (displayed by the `COMMAND` field listed by the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) statement or in the [Information Schema PROCESSLIST Table](/kb/en/information-schema-processlist-table/), as well as the `PROCESSLIST_COMMAND` value listed in the [Performance Schema threads Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table/)). These indicate the nature of the thread's activity.

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td>Binlog Dump</td><td>Master thread for sending <a href="/kb/en/binary-log/">binary log</a> contents to a slave.</td></tr>
<tr><td>Change user</td><td>Executing a change user operation.</td></tr>
<tr><td>Close stmt</td><td>Closing a <a href="/kb/en/prepared-statements/">prepared statement</a>.</td></tr>
<tr><td>Connect</td><td><a href="/kb/en/replication/">Replication</a> slave is connected to its master.</td></tr>
<tr><td>Connect Out</td><td>Replication slave is in the process of connecting to its master.</td></tr>
<tr><td>Create DB</td><td>Executing an operation to create a database.</td></tr>
<tr><td>Daemon</td><td>Internal server thread rather than for servicing a client connection.</td></tr>
<tr><td>Debug</td><td>Generating debug information.</td></tr>
<tr><td>Delayed insert</td><td>A delayed-insert handler.</td></tr>
<tr><td>Drop DB</td><td>Executing an operation to drop a database.</td></tr>
<tr><td>Error</td><td>Error.</td></tr>
<tr><td>Execute</td><td>Executing a <a href="/kb/en/prepared-statements/">prepared statement</a>.</td></tr>
<tr><td>Fetch</td><td>Fetching the results of an executed <a href="/kb/en/prepared-statements/">prepared statement</a>.</td></tr>
<tr><td>Field List</td><td>Retrieving table column information.</td></tr>
<tr><td>Init DB</td><td>Selecting default database.</td></tr>
<tr><td>Kill</td><td>Killing another thread.</td></tr>
<tr><td>Long Data</td><td>Retrieving long data from the result of executing a <a href="/kb/en/prepared-statements/">prepared statement</a>.</td></tr>
<tr><td>Ping</td><td>Handling a server ping request.</td></tr>
<tr><td>Prepare</td><td>Preparing a <a href="/kb/en/prepared-statements/">prepared statement</a>.</td></tr>
<tr><td>Processlist</td><td>Preparing processlist information about server threads.</td></tr>
<tr><td>Query</td><td>Executing a statement.</td></tr>
<tr><td>Quit</td><td>In the process of terminating the thread.</td></tr>
<tr><td>Refresh</td><td><a href="/kb/en/flush/">Flushing</a> a table, logs or caches, or refreshing replication server or <a href="/kb/en/server-status-variables/">status variable</a> information.</td></tr>
<tr><td>Register Slave</td><td>Registering a slave server.</td></tr>
<tr><td>Reset stmt</td><td>Resetting a <a href="/kb/en/prepared-statements/">prepared statement</a>.</td></tr>
<tr><td>Set option</td><td>Setting or resetting a client statement execution option.</td></tr>
<tr><td>Sleep</td><td>Waiting for the client to send a new statement.</td></tr>
<tr><td>Shutdown</td><td>Shutting down the server.</td></tr>
<tr><td>Statistics</td><td>Preparing status information about the server.</td></tr>
<tr><td>Table Dump</td><td>Sending the contents of a table to a slave.</td></tr>
<tr><td>Time</td><td>Not used.</td></tr>
</tbody></table>