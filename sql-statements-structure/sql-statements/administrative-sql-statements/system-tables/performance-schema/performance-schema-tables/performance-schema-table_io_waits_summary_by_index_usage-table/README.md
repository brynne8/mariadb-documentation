# Performance Schema table_io_waits_summary_by_index_usage Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `table_io_waits_summary_by_index_usage` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables/), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

The `table_io_waits_summary_by_index_usage` table records table I/O waits by index.

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>OBJECT_TYPE</code></td><td><code>TABLE</code> in the case of all indexes.</td></tr>
<tr><td><code>OBJECT_SCHEMA</code></td><td>Schema name.</td></tr>
<tr><td><code>OBJECT_NAME</code></td><td>Table name.</td></tr>
<tr><td><code>INDEX_NAME</code></td><td>Index name, or <code>PRIMARY</code> for the primary index, <code>NULL</code> for no index (inserts are counted in this case).</td></tr>
<tr><td><code>COUNT_STAR</code></td><td>Number of summarized events and the sum of the <code>x_READ</code> and <code>x_WRITE</code> columns.</td></tr>
<tr><td><code>SUM_TIMER_WAIT</code></td><td>Total wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MIN_TIMER_WAIT</code></td><td>Minimum wait time of the summarized events that are timed.</td></tr>
<tr><td><code>AVG_TIMER_WAIT</code></td><td>Average wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MAX_TIMER_WAIT</code></td><td>Maximum wait time of the summarized events that are timed.</td></tr>
<tr><td><code>COUNT_READ</code></td><td>Number of all read operations, and the sum of the equivalent <code>x_FETCH</code> columns.</td></tr>
<tr><td><code>SUM_TIMER_READ</code></td><td>Total wait time of all read operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_READ</code></td><td>Minimum wait time of all read operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_READ</code></td><td>Average wait time of all read operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_READ</code></td><td>Maximum wait time of all read operations that are timed.</td></tr>
<tr><td><code>COUNT_WRITE</code></td><td>Number of all write operations, and the sum of the equivalent <code>x_INSERT</code>, <code>x_UPDATE</code> and <code>x_DELETE</code> columns.</td></tr>
<tr><td><code>SUM_TIMER_WRITE</code></td><td>Total wait time of all write operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_WRITE</code></td><td>Minimum wait time of all write operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_WRITE</code></td><td>Average wait time of all write operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_WRITE</code></td><td>Maximum wait time of all write operations that are timed.</td></tr>
<tr><td><code>COUNT_FETCH</code></td><td>Number of all fetch operations.</td></tr>
<tr><td><code>SUM_TIMER_FETCH</code></td><td>Total wait time of all fetch operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_FETCH</code></td><td>Minimum wait time of all fetch operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_FETCH</code></td><td>Average wait time of all fetch operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_FETCH</code></td><td>Maximum wait time of all fetch operations that are timed.</td></tr>
<tr><td><code>COUNT_INSERT</code></td><td>Number of all insert operations.</td></tr>
<tr><td><code>SUM_TIMER_INSERT</code></td><td>Total wait time of all insert operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_INSERT</code></td><td>Minimum wait time of all insert operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_INSERT</code></td><td>Average wait time of all insert operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_INSERT</code></td><td>Maximum wait time of all insert operations that are timed.</td></tr>
<tr><td><code>COUNT_UPDATE</code></td><td>Number of all update operations.</td></tr>
<tr><td><code>SUM_TIMER_UPDATE</code></td><td>Total wait time of all update operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_UPDATE</code></td><td>Minimum wait time of all update operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_UPDATE</code></td><td>Average wait time of all update operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_UPDATE</code></td><td>Maximum wait time of all update operations that are timed.</td></tr>
<tr><td><code>COUNT_DELETE</code></td><td>Number of all delete operations.</td></tr>
<tr><td><code>SUM_TIMER_DELETE</code></td><td>Total wait time of all delete operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_DELETE</code></td><td>Minimum wait time of all delete operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_DELETE</code></td><td>Average wait time of all delete operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_DELETE</code></td><td>Maximum wait time of all delete operations that are timed.</td></tr>
</tbody></table>

You can [TRUNCATE](/sql-statements-structure/sql-statements/table-statements/truncate-table/) the table, which will reset all counters to zero. The table is also truncated if the [table_io_waits_summary_by_table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-table_io_waits_summary_by_table-table/) table is truncated.

If a table's index structure is changed, index statistics recorded in this table may also be reset.