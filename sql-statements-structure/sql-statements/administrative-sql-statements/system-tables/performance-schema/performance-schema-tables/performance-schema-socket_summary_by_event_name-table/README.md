# Performance Schema socket_summary_by_event_name Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `socket_summary_by_event_name` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables/), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

It aggregates timer and byte count statistics for all socket I/O operations by socket instrument.

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>EVENT_NAME</code></td><td>Socket instrument.</td></tr>
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
<tr><td><code>COUNT_MISC</code></td><td>Number of all miscellaneous operations not counted above, including <code>CONNECT</code>, <code>LISTENw</code>, <code>ACCEPT</code>, <code>CLOSE</code>, and <code>SHUTDOWN</code>.</td></tr>
<tr><td><code>SUM_TIMER_MISC</code></td><td>Total wait time of all miscellaneous operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_MISC</code></td><td>Minimum wait time of all miscellaneous operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_MISC</code></td><td>Average wait time of all miscellaneous operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_MISC</code></td><td>Maximum wait time of all miscellaneous operations that are timed.</td></tr>
</tbody></table>

You can [TRUNCATE](/sql-statements-structure/sql-statements/table-statements/truncate-table/) the table, which will reset all counters to zero.

## Example

```sql
SELECT * FROM socket_summary_by_event_name\G
*************************** 1. row ***************************
               EVENT_NAME: wait/io/socket/sql/server_tcpip_socket
               COUNT_STAR: 0
           SUM_TIMER_WAIT: 0
           MIN_TIMER_WAIT: 0
           AVG_TIMER_WAIT: 0
           MAX_TIMER_WAIT: 0
               COUNT_READ: 0
           SUM_TIMER_READ: 0
           MIN_TIMER_READ: 0
           AVG_TIMER_READ: 0
           MAX_TIMER_READ: 0
 SUM_NUMBER_OF_BYTES_READ: 0
              COUNT_WRITE: 0
          SUM_TIMER_WRITE: 0
          MIN_TIMER_WRITE: 0
          AVG_TIMER_WRITE: 0
          MAX_TIMER_WRITE: 0
SUM_NUMBER_OF_BYTES_WRITE: 0
               COUNT_MISC: 0
           SUM_TIMER_MISC: 0
           MIN_TIMER_MISC: 0
           AVG_TIMER_MISC: 0
           MAX_TIMER_MISC: 0
*************************** 2. row ***************************
               EVENT_NAME: wait/io/socket/sql/server_unix_socket
               COUNT_STAR: 0
           SUM_TIMER_WAIT: 0
           MIN_TIMER_WAIT: 0
           AVG_TIMER_WAIT: 0
           MAX_TIMER_WAIT: 0
               COUNT_READ: 0
           SUM_TIMER_READ: 0
           MIN_TIMER_READ: 0
           AVG_TIMER_READ: 0
           MAX_TIMER_READ: 0
 SUM_NUMBER_OF_BYTES_READ: 0
              COUNT_WRITE: 0
          SUM_TIMER_WRITE: 0
          MIN_TIMER_WRITE: 0
          AVG_TIMER_WRITE: 0
          MAX_TIMER_WRITE: 0
SUM_NUMBER_OF_BYTES_WRITE: 0
               COUNT_MISC: 0
           SUM_TIMER_MISC: 0
           MIN_TIMER_MISC: 0
           AVG_TIMER_MISC: 0
           MAX_TIMER_MISC: 0
...
```