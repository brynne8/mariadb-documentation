# Performance Schema events_waits_summary_by_instance Table

The [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema) `events_waits_summary_by_instance` table contains wait events summarized by instance. It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>EVENT_NAME</code></td><td>Event name. Used together with <code>OBJECT_INSTANCE_BEGIN</code> for grouping events.</td></tr>
<tr><td><code>OBJECT_INSTANCE_BEGIN</code></td><td>If an instrument creates multiple instances, each instance has a unique <code>OBJECT_INSTANCE_BEGIN</code> value to allow for grouping by instance.</td></tr>
<tr><td><code>COUNT_STAR</code></td><td>Number of summarized events</td></tr>
<tr><td><code>SUM_TIMER_WAIT</code></td><td>Total wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MIN_TIMER_WAIT</code></td><td>Minimum wait time of the summarized events that are timed.</td></tr>
<tr><td><code>AVG_TIMER_WAIT</code></td><td>Average wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MAX_TIMER_WAIT</code></td><td>Maximum wait time of the summarized events that are timed.</td></tr>
</tbody></table>

The `*_TIMER_WAIT` columns only calculate results for timed events, as non-timed events have a `NULL` wait time.

## Example

```sql
SELECT * FROM events_waits_summary_by_instance\G
...
*************************** 202. row ***************************
           EVENT_NAME: wait/io/file/sql/binlog
OBJECT_INSTANCE_BEGIN: 140578961969856
           COUNT_STAR: 6
       SUM_TIMER_WAIT: 90478331960
       MIN_TIMER_WAIT: 263344
       AVG_TIMER_WAIT: 15079721848
       MAX_TIMER_WAIT: 67760576376
*************************** 203. row ***************************
           EVENT_NAME: wait/io/file/sql/dbopt
OBJECT_INSTANCE_BEGIN: 140578961970560
           COUNT_STAR: 6
       SUM_TIMER_WAIT: 39891428472
       MIN_TIMER_WAIT: 387168
       AVG_TIMER_WAIT: 6648571412
       MAX_TIMER_WAIT: 24503293304
*************************** 204. row ***************************
           EVENT_NAME: wait/io/file/sql/dbopt
OBJECT_INSTANCE_BEGIN: 140578961971264
           COUNT_STAR: 6
       SUM_TIMER_WAIT: 39902495024
       MIN_TIMER_WAIT: 177888
       AVG_TIMER_WAIT: 6650415692
       MAX_TIMER_WAIT: 21026400404
```