# Performance Schema events_statements_current Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The events_statements_current table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

The `events_statements_current` table contains current statement events, with each row being a record of a thread and its most recent statement event.

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
<tr><td><code>LOCK_TIME</code></td><td>Time in picoseconds spent waiting for locks. The time is calculated in microseconds but stored in picoseconds for compatibility with other timings.</td></tr>
<tr><td><code>SQL_TEXT</code></td><td>The SQL statement, or <code>NULL</code> if the command is not associated with an SQL statement.</td></tr>
<tr><td><code>DIGEST</code></td><td><a href="/kb/en/performance-schema-digests/">Statement digest</a>.</td></tr>
<tr><td><code>DIGEST_TEXT</code></td><td><a href="/kb/en/performance-schema-digests/">Statement digest</a> text.</td></tr>
<tr><td><code>CURRENT_SCHEMA</code></td><td>Statement's default database for the statement, or <code>NULL</code> if there was none.</td></tr>
<tr><td><code>OBJECT_SCHEMA</code></td><td>Reserved, currently <code>NULL</code></td></tr>
<tr><td><code>OBJECT_NAME</code></td><td>Reserved, currently <code>NULL</code></td></tr>
<tr><td><code>OBJECT_TYPE</code></td><td>Reserved, currently <code>NULL</code></td></tr>
<tr><td><code>OBJECT_INSTANCE_BEGIN</code></td><td>Address in memory of the statement object.</td></tr>
<tr><td><code>MYSQL_ERRNO</code></td><td>Error code. See <a href="/kb/en/mariadb-error-codes/">MariaDB Error Codes</a> for a full list.</td></tr>
<tr><td><code>RETURNED_SQLSTATE</code></td><td>The <a href="/kb/en/sqlstate/">SQLSTATE</a> value.</td></tr>
<tr><td><code>MESSAGE_TEXT</code></td><td>Statement error message. See <a href="/kb/en/mariadb-error-codes/">MariaDB Error Codes</a>.</td></tr>
<tr><td><code>ERRORS</code></td><td><code>0</code> if <code>SQLSTATE</code> signifies completion (starting with 00) or warning (01), otherwise <code>1</code>.</td></tr>
<tr><td><code>WARNINGS</code></td><td>Number of warnings from the diagnostics area.</td></tr>
<tr><td><code>ROWS_AFFECTED</code></td><td>Number of rows affected the statement affected.</td></tr>
<tr><td><code>ROWS_SENT</code></td><td>Number of rows returned.</td></tr>
<tr><td><code>ROWS_EXAMINED</code></td><td>Number of rows read during the statement's execution.</td></tr>
<tr><td><code>CREATED_TMP_DISK_TABLES</code></td><td>Number of on-disk temp tables created by the statement.</td></tr>
<tr><td><code>CREATED_TMP_TABLES</code></td><td>Number of temp tables created by the statement.</td></tr>
<tr><td><code>SELECT_FULL_JOIN</code></td><td>Number of joins performed by the statement which did not use an index.</td></tr>
<tr><td><code>SELECT_FULL_RANGE_JOIN</code></td><td>Number of joins performed by the statement which used a range search of the first table.</td></tr>
<tr><td><code>SELECT_RANGE</code></td><td>Number of joins performed by the statement which used a range of the first table.</td></tr>
<tr><td><code>SELECT_RANGE_CHECK</code></td><td>Number of joins without keys performed by the statement that check for key usage after each row.</td></tr>
<tr><td><code>SELECT_SCAN</code></td><td>Number of joins performed by the statement which used a full scan of the first table.</td></tr>
<tr><td><code>SORT_MERGE_PASSES</code></td><td>Number of merge passes by the sort algorithm performed by the statement. If too high, you may need to increase the <a href="/kb/en/server-system-variables/#sort_buffer_size">sort_buffer_size</a>.</td></tr>
<tr><td><code>SORT_RANGE</code></td><td>Number of sorts performed by the statement which used a range.</td></tr>
<tr><td><code>SORT_ROWS</code></td><td>Number of rows sorted by the statement.</td></tr>
<tr><td><code>SORT_SCAN</code></td><td>Number of sorts performed by the statement which used a full table scan.</td></tr>
<tr><td><code>NO_INDEX_USED</code></td><td><code>0</code> if the statement performed a table scan with an index, <code>1</code> if without an index.</td></tr>
<tr><td><code>NO_GOOD_INDEX_USED</code></td><td><code>0</code> if a good index was found for the statement, <code>1</code> if no good index was found. See the <code>Range checked for each record description</code> in the <a href="/kb/en/explain/">EXPLAIN</a> article.</td></tr>
<tr><td><code>NESTING_EVENT_ID</code></td><td>Reserved, currently <code>NULL</code>.</td></tr>
<tr><td><code>NESTING_EVENT_TYPE</code></td><td>Reserved, currently <code>NULL</code>.</td></tr>
</tbody></table>

It is possible to empty this table with a `TRUNCATE TABLE` statement.

The related tables, [events_statements_history](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_statements_history-table) and [events_statements_history_long](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_statements_history_long-table) derive their values from the current events table.