# Performance Schema socket_summary_by_instance Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `socket_summary_by_instance` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

It aggregates timer and byte count statistics for all socket I/O operations by socket instance.

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>EVENT_NAME</code></td><td>Socket instrument.</td></tr>
<tr><td><code>OBJECT_INSTANCE_BEGIN</code></td><td>Address in memory.</td></tr>
<tr><td><code>COUNT_STAR</code></td><td>Number of summarized events</td></tr>
<tr><td><code>SUM_TIMER_WAIT</code></td><td>Total wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MIN_TIMER_WAIT</code></td><td>Minimum wait time of the summarized events that are timed.</td></tr>
<tr><td><code>AVG_TIMER_WAIT</code></td><td>Average wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MAX_TIMER_WAIT</code></td><td>Maximum wait time of the summarized events that are timed.</td></tr>
<tr><td><code>COUNT_READ</code></td><td>Number of all read operations, including <code>RECV</code>, <code>RECVFROM</code>, and <code>RECVMSG</code>.</td></tr>
<tr><td><code>SUM_TIMER_READ</code></td><td>Total wait time of all read operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_READ</code></td><td>Minimum wait time of all read operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_READ</code></td><td>Average wait time of all read operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_READ</code></td><td>Maximum wait time of all read operations that are timed.</td></tr>
<tr><td><code>SUM_NUMBER_OF_BYTES_READ</code></td><td>Bytes read by read operations.</td></tr>
<tr><td><code>COUNT_WRITE</code></td><td>Number of all write operations, including <code>SEND</code>, <code>SENDTO</code>, and <code>SENDMSG</code>.</td></tr>
<tr><td><code>SUM_TIMER_WRITE</code></td><td>Total wait time of all write operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_WRITE</code></td><td>Minimum wait time of all write operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_WRITE</code></td><td>Average wait time of all write operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_WRITE</code></td><td>Maximum wait time of all write operations that are timed.</td></tr>
<tr><td><code>SUM_NUMBER_OF_BYTES_WRITE</code></td><td>Bytes written by write operations.</td></tr>
<tr><td><code>COUNT_MISC</code></td><td>Number of all miscellaneous operations not counted above, including <code>CONNECT</code>, <code>LISTEN</code>, <code>ACCEPT</code>, <code>CLOSE</code>, and <code>SHUTDOWN</code>.</td></tr>
<tr><td><code>SUM_TIMER_MISC</code></td><td>Total wait time of all miscellaneous operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_MISC</code></td><td>Minimum wait time of all miscellaneous operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_MISC</code></td><td>Average wait time of all miscellaneous operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_MISC</code></td><td>Maximum wait time of all miscellaneous operations that are timed.</td></tr>
</tbody></table>

The corresponding row in the table is deleted when a connection terminates.

You can [TRUNCATE](/sql-statements-structure/sql-statements/table-statements/truncate-table) the table, which will reset all counters to zero.