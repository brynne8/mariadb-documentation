# Performance Schema events_statements_summary_by_account_by_event_name Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `events_statements_summary_by_account_by_event_name` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

The [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema) events_statements_summary_by_account_by_event_name table contains statement events summarized by account and event name. It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>USER</code></td><td>User. Used together with <code>HOST</code> and <code>EVENT_NAME</code> for grouping events.</td></tr>
<tr><td><code>HOST</code></td><td>Host. Used together with <code>USER</code> and <code>EVENT_NAME</code> for grouping events.</td></tr>
<tr><td><code>EVENT_NAME</code></td><td>Event name. Used together with <code>USER</code> and <code>HOST</code> for grouping events.</td></tr>
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
<tr><td><code>SUM_SELECT_SCAN</code></td><td>Sum of the <code>SELECT_SCANx</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SORT_MERGE_PASSES</code></td><td>Sum of the <code>SORT_MERGE_PASSES</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SORT_RANGE</code></td><td>Sum of the <code>SORT_RANGE</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SORT_ROWS</code></td><td>Sum of the <code>SORT_ROWS</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_SORT_SCAN</code></td><td>Sum of the <code>SORT_SCAN</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_NO_INDEX_USED</code></td><td>Sum of the <code>NO_INDEX_USED</code> column in the <code>events_statements_current</code> table.</td></tr>
<tr><td><code>SUM_NO_GOOD_INDEX_USED</code></td><td>Sum of the <code>NO_GOOD_INDEX_USED</code> column in the <code>events_statements_current</code> table.</td></tr>
</tbody></table>

The `*_TIMER_WAIT` columns only calculate results for timed events, as non-timed events have a `NULL` wait time.

## Example

```sql
SELECT * FROM events_statements_summary_by_account_by_event_name\G
...
*************************** 521. row ***************************
                       USER: NULL
                       HOST: NULL
                 EVENT_NAME: statement/com/Error
                 COUNT_STAR: 0
             SUM_TIMER_WAIT: 0
             MIN_TIMER_WAIT: 0
             AVG_TIMER_WAIT: 0
             MAX_TIMER_WAIT: 0
              SUM_LOCK_TIME: 0
                 SUM_ERRORS: 0
               SUM_WARNINGS: 0
          SUM_ROWS_AFFECTED: 0
              SUM_ROWS_SENT: 0
          SUM_ROWS_EXAMINED: 0
SUM_CREATED_TMP_DISK_TABLES: 0
     SUM_CREATED_TMP_TABLES: 0
       SUM_SELECT_FULL_JOIN: 0
 SUM_SELECT_FULL_RANGE_JOIN: 0
           SUM_SELECT_RANGE: 0
     SUM_SELECT_RANGE_CHECK: 0
            SUM_SELECT_SCAN: 0
      SUM_SORT_MERGE_PASSES: 0
             SUM_SORT_RANGE: 0
              SUM_SORT_ROWS: 0
              SUM_SORT_SCAN: 0
          SUM_NO_INDEX_USED: 0
     SUM_NO_GOOD_INDEX_USED: 0
*************************** 522. row ***************************
                       USER: NULL
                       HOST: NULL
                 EVENT_NAME: statement/com/
                 COUNT_STAR: 0
             SUM_TIMER_WAIT: 0
             MIN_TIMER_WAIT: 0
             AVG_TIMER_WAIT: 0
             MAX_TIMER_WAIT: 0
              SUM_LOCK_TIME: 0
                 SUM_ERRORS: 0
               SUM_WARNINGS: 0
          SUM_ROWS_AFFECTED: 0
              SUM_ROWS_SENT: 0
          SUM_ROWS_EXAMINED: 0
SUM_CREATED_TMP_DISK_TABLES: 0
     SUM_CREATED_TMP_TABLES: 0
       SUM_SELECT_FULL_JOIN: 0
 SUM_SELECT_FULL_RANGE_JOIN: 0
           SUM_SELECT_RANGE: 0
     SUM_SELECT_RANGE_CHECK: 0
            SUM_SELECT_SCAN: 0
      SUM_SORT_MERGE_PASSES: 0
             SUM_SORT_RANGE: 0
              SUM_SORT_ROWS: 0
              SUM_SORT_SCAN: 0
          SUM_NO_INDEX_USED: 0
     SUM_NO_GOOD_INDEX_USED: 0
```