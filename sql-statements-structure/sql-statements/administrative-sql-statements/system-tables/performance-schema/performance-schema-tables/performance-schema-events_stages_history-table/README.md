# Performance Schema events_stages_history Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `events_stages_history` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables/), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

The `events_stages_history` table by default contains the ten most recent completed stage events per thread. This number can be adjusted by setting the [performance_schema_events_stages_history_size](/kb/en/performance-schema-system-variables/#performance_schema_events_stages_history_size) system variable when the server starts up.

The table structure is identical to the [events_stage_current](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_stages_current-table/) table structure, and contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>THREAD_ID</code></td><td>Thread associated with the event. Together with <code>EVENT_ID</code> uniquely identifies the row.</td></tr>
<tr><td><code>EVENT_ID</code></td><td>Thread's current event number at the start of the event. Together with <code>THREAD_ID</code> uniquely identifies the row.</td></tr>
<tr><td><code>END_EVENT_ID</code></td><td><code>NULL</code> when the event starts, set to the thread's current event number at the end of the event.</td></tr>
<tr><td><code>EVENT_NAME</code></td><td>Event instrument name and a <code>NAME</code> from the <code>setup_instruments</code> table</td></tr>
<tr><td><code>SOURCE</code></td><td>Name and line number of the source file containing the instrumented code that produced the event.</td></tr>
<tr><td><code>TIMER_START</code></td><td>Value in picoseconds when the event timing started or <code>NULL</code> if timing is not collected.</td></tr>
<tr><td><code>TIMER_END</code></td><td>Value in picoseconds when the event timing ended, or <code>NULL</code> if timing is not collected.</td></tr>
<tr><td><code>TIMER_WAIT</code></td><td>Value in picoseconds of the event's duration or <code>NULL</code> if timing is not collected.</td></tr>
<tr><td><code>NESTING_EVENT_ID</code></td><td><code>EVENT_ID</code> of event within which this event nests.</td></tr>
<tr><td><code>NESTING_EVENT_TYPE</code></td><td>Nesting event type. Either <code>statement</code>, <code>stage</code> or <code>wait</code>.</td></tr>
</tbody></table>

It is possible to empty this table with a `TRUNCATE TABLE` statement.

[events_stages_current](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_stages_current-table/) and [events_stages_history_long](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_stages_history_long-table/) are related tables.