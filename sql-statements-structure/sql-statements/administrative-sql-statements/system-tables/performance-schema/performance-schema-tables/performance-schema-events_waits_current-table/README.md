# Performance Schema events_waits_current Table

The `events_waits_current` table contains the status of a thread's most recently monitored wait event, listing one event per thread.

The table contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>THREAD_ID</code></td><td>Thread associated with the event. Together with <code>EVENT_ID</code> uniquely identifies the row.</td></tr>
<tr><td><code>EVENT_ID</code></td><td>Thread's current event number at the start of the event. Together with <code>THREAD_ID</code> uniquely identifies the row.</td></tr>
<tr><td><code>END_EVENT_ID</code></td><td><code>NULL</code> when the event starts, set to the thread's current event number at the end of the event.</td></tr>
<tr><td><code>EVENT_NAME</code></td><td>Event instrument name and a <code>NAME</code> from the <code>setup_instruments</code> table</td></tr>
<tr><td><code>SOURCE</code></td><td>Name and line number of the source file containing the instrumented code that produced the event.</td></tr>
<tr><td><code>TIMER_START</code></td><td>Value in picoseconds when the event timing started or <code>NULL</code> if timing is not collected.</td></tr>
<tr><td><code>TIMER_END</code></td><td>Value in picoseconds when the event timing ended, or <code>NULL</code> if the event has not ended or timing is not collected.</td></tr>
<tr><td><code>TIMER_WAIT</code></td><td>Value in picoseconds of the event's duration or <code>NULL</code> if the event has not ended or timing is not collected.</td></tr>
<tr><td><code>SPINS</code></td><td>Number of spin rounds for a mutex, or <code>NULL</code> if spin rounds are not used, or spinning is not instrumented.</td></tr>
<tr><td><code>OBJECT_SCHEMA</code></td><td>Name of the schema that contains the table for table I/O objects, otherwise <code>NULL</code> for file I/O and synchronization objects.</td></tr>
<tr><td><code>OBJECT_NAME</code></td><td>File name for file I/O objects, table name for table I/O objects, the socket's <code>IP:PORT</code> value for a socket object or <code>NULL</code> for a synchronization object.</td></tr>
<tr><td><code>OBJECT_TYPE</code></td><td>FILE for a file object, <code>TABLE</code> or <code>TEMPORARY TABLE</code> for a table object, or <code>NULL</code> for a synchronization object.</td></tr>
<tr><td><code>OBJECT_INSTANCE_BEGIN</code></td><td>Address in memory of the object.</td></tr>
<tr><td><code>INDEX</code> NAME</td><td>Name of the index, <code>PRIMARY</code> for the primary key, or <code>NULL</code> for no index used.</td></tr>
<tr><td><code>NESTING_EVENT_ID</code></td><td><code>EVENT_ID</code> of event within which this event nests.</td></tr>
<tr><td><code>NESTING_EVENT_TYPE</code></td><td>Nesting event type. Either <code>statement</code>, <code>stage</code> or <code>wait</code>.</td></tr>
<tr><td><code>OPERATION</code></td><td>Operation type, for example read, write or lock</td></tr>
<tr><td><code>NUMBER_OF_BYTES</code></td><td>Number of bytes that the operation read or wrote, or <code>NULL</code> for table I/O waits.</td></tr>
<tr><td><code>FLAGS</code></td><td>Reserved for use in the future.</td></tr>
</tbody></table>

It is possible to empty this table with a `TRUNCATE TABLE` statement.

The related tables, [events_waits_history](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_waits_history-table/) and [events_waits_history_long](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_waits_history_long-table/) derive their values from the current events.