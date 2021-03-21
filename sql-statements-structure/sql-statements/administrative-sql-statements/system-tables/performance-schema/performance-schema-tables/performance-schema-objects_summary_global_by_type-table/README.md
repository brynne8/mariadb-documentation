# Performance Schema objects_summary_global_by_type Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `objects_summary_global_by_type` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

It aggregates object wait events, and contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>OBJECT_TYPE</code></td><td>Groups records together with <code>OBJECT_SCHEMA</code> and <code>OBJECT_NAME</code>.</td></tr>
<tr><td><code>OBJECT_SCHEMA</code></td><td>Groups records together with <code>OBJECT_TYPE</code> and <code>OBJECT_NAME</code>.</td></tr>
<tr><td><code>OBJECT_NAME</code></td><td>Groups records together with <code>OBJECT_SCHEMA</code> and <code>OBJECT_TYPE</code>.</td></tr>
<tr><td><code>COUNT_STAR</code></td><td>Number of summarized events</td></tr>
<tr><td><code>SUM_TIMER_WAIT</code></td><td>Total wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MIN_TIMER_WAIT</code></td><td>Minimum wait time of the summarized events that are timed.</td></tr>
<tr><td><code>AVG_TIMER_WAIT</code></td><td>Average wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MAX_TIMER_WAIT</code></td><td>Maximum wait time of the summarized events that are timed.</td></tr>
</tbody></table>

You can [TRUNCATE](/sql-statements-structure/sql-statements/table-statements/truncate-table) the table, which will reset all counters to zero.

## Example

```sql
SELECT * FROM objects_summary_global_by_type\G
...
*************************** 101. row ***************************
   OBJECT_TYPE: TABLE
 OBJECT_SCHEMA: test
   OBJECT_NAME: v
    COUNT_STAR: 0
SUM_TIMER_WAIT: 0
MIN_TIMER_WAIT: 0
AVG_TIMER_WAIT: 0
MAX_TIMER_WAIT: 0
*************************** 102. row ***************************
   OBJECT_TYPE: TABLE
 OBJECT_SCHEMA: test
   OBJECT_NAME: xx2
    COUNT_STAR: 2
SUM_TIMER_WAIT: 1621920
MIN_TIMER_WAIT: 481344
AVG_TIMER_WAIT: 810960
MAX_TIMER_WAIT: 1140576
```