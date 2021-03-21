# Performance Schema events_waits_summary_global_by_event_name Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `events_waits_summary_global_by_event_name` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

The [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema) `events_waits_summary_global_by_event_name` table contains wait events summarized by event name. It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>EVENT_NAME</code></td><td>Event name.</td></tr>
<tr><td><code>COUNT_STAR</code></td><td>Number of summarized events</td></tr>
<tr><td><code>SUM_TIMER_WAIT</code></td><td>Total wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MIN_TIMER_WAIT</code></td><td>Minimum wait time of the summarized events that are timed.</td></tr>
<tr><td><code>AVG_TIMER_WAIT</code></td><td>Average wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MAX_TIMER_WAIT</code></td><td>Maximum wait time of the summarized events that are timed.</td></tr>
</tbody></table>

The `*_TIMER_WAIT` columns only calculate results for timed events, as non-timed events have a `NULL` wait time.

## Example

```sql
SELECT * FROM events_waits_summary_global_by_event_name\G
...
*************************** 303. row ***************************
    EVENT_NAME: wait/io/socket/sql/server_tcpip_socket
    COUNT_STAR: 0
SUM_TIMER_WAIT: 0
MIN_TIMER_WAIT: 0
AVG_TIMER_WAIT: 0
MAX_TIMER_WAIT: 0
*************************** 304. row ***************************
    EVENT_NAME: wait/io/socket/sql/server_unix_socket
    COUNT_STAR: 0
SUM_TIMER_WAIT: 0
MIN_TIMER_WAIT: 0
AVG_TIMER_WAIT: 0
MAX_TIMER_WAIT: 0
*************************** 305. row ***************************
    EVENT_NAME: wait/io/socket/sql/client_connection
    COUNT_STAR: 0
SUM_TIMER_WAIT: 0
MIN_TIMER_WAIT: 0
AVG_TIMER_WAIT: 0
MAX_TIMER_WAIT: 0
*************************** 306. row ***************************
    EVENT_NAME: idle
    COUNT_STAR: 265
SUM_TIMER_WAIT: 46861125181000000
MIN_TIMER_WAIT: 1000000
AVG_TIMER_WAIT: 176834434000000
MAX_TIMER_WAIT: 4912417573000000
```