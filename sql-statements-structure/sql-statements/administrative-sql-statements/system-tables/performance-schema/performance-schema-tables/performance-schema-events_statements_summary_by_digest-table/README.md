# Performance Schema events_statements_summary_by_digest Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `events_statements_summary_by_digest` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables/), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

The [Performance Schema digest](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-digests/) is a hashed, normalized form of a statement with the specific data values removed. It allows statistics to be gathered for similar kinds of statements.

The [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/) `events_statements_summary_by_digest` table records statement events summarized by schema and digest. It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>SCHEMA NAME</code></td><td>Database name. Records are summarised together with <code>DIGEST</code>.</td></tr>
<tr><td><code>DIGEST</code></td><td><a href="/kb/en/performance-schema-digests/">Performance Schema digest</a>. Records are summarised together with <code>SCHEMA NAME</code>.</td></tr>
<tr><td><code>DIGEST TEXT</code></td><td>The unhashed form of the digest.</td></tr>
<tr><td><code>COUNT_STAR</code></td><td>Number of summarized events</td></tr>
<tr><td><code>SUM_TIMER_WAIT</code></td><td>Total wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MIN_TIMER_WAIT</code></td><td>Minimum wait time of the summarized events that are timed.</td></tr>
<tr><td><code>AVG_TIMER_WAIT</code></td><td>Average wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MAX_TIMER_WAIT</code></td><td>Maximum wait time of the summarized events that are timed.</td></tr>
<tr><td><code>SUM_LOCK_TIME</code></td><td>Sum of the <code>LOCK_TIME</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_ERRORS</code></td><td>Sum of the <code>ERRORS</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_WARNINGS</code></td><td>Sum of the <code>WARNINGS</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_ROWS_AFFECTED</code></td><td>Sum of the <code>ROWS_AFFECTED</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_ROWS_SENT</code></td><td>Sum of the <code>ROWS_SENT</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_ROWS_EXAMINED</code></td><td>Sum of the <code>ROWS_EXAMINED</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_CREATED_TMP_DISK_TABLES</code></td><td>Sum of the <code>CREATED_TMP_DISK_TABLES</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_CREATED_TMP_TABLES</code></td><td>Sum of the <code>CREATED_TMP_TABLES</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SELECT_FULL_JOIN</code></td><td>Sum of the <code>SELECT_FULL_JOIN</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SELECT_FULL_RANGE_JOIN</code></td><td>Sum of the <code>SELECT_FULL_RANGE_JOIN</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SELECT_RANGE</code></td><td>Sum of the <code>SELECT_RANGE</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SELECT_RANGE_CHECK</code></td><td>Sum of the <code>SELECT_RANGE_CHECK</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SELECT_SCAN</code></td><td>Sum of the <code>SELECT_SCAN</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SORT_MERGE_PASSES</code></td><td>Sum of the <code>SORT_MERGE_PASSES</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SORT_RANGE</code></td><td>Sum of the <code>SORT_RANGE</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SORT_ROWS</code></td><td>Sum of the <code>SORT_ROWS</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SORT_SCAN</code></td><td>Sum of the <code>SORT_SCAN</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_NO_INDEX_USED</code></td><td>Sum of the <code>NO_INDEX_USED</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_NO_GOOD_INDEX_USED</code></td><td>Sum of the <code>NO_GOOD_INDEX_USED</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>FIRST_SEEN</code></td><td>Time at which the digest was first seen.</td></tr>
<tr><td><code>LAST_SEEN</code></td><td>Time at which the digest was most recently seen.</td></tr>
</tbody></table>

The `*_TIMER_WAIT` columns only calculate results for timed events, as non-timed events have a `NULL` wait time.

The `events_statements_summary_by_digest` table is limited in size by the [performance_schema_digests_size](/kb/en/performance-schema-system-variables/#performance_schema_digests_size) system variable. Once the limit has been reached and the table is full, all entries are aggregated in a row with a `NULL` digest. The `COUNT_STAR` value of this `NULL` row indicates how many digests are recorded in the row and therefore gives an indication of whether `performance_schema_digests_size` should be increased to provide more accurate statistics.