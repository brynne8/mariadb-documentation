# Performance Schema performance_timers Table

## Description

The `performance_timers` table lists available event timers.

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>TIMER_NAME</code></td><td>Time name, used in the <a href="/kb/en/performance-schema-setup_timers-table/">setup_timers</a> table.</td></tr>
<tr><td><code>TIMER_FREQUENCY</code></td><td>Number of timer units per second. Dependent on the processor speed.</td></tr>
<tr><td><code>TIMER_RESOLUTION</code></td><td>Number of timer units by which timed values increase each time.</td></tr>
<tr><td><code>TIMER_OVERHEAD</code></td><td>Minimum timer overhead, determined during initialization by calling the timer 20 times and selecting the smallest value. Total overhead will be at least double this, as the timer is called at the beginning and end of each timed event.</td></tr>
</tbody></table>

Any `NULL` values indicate that that particular timer is not available on your platform, Any timer names with a non-NULL value can be used in the [setup_timers](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-setup_timers-table) table.

## Example

```sql
SELECT * FROM performance_timers;
+-------------+-----------------+------------------+---------------------+
| TIMER_NAME  | TIMER_FREQUENCY | TIMER_RESOLUTION | TIMER_OVERHEAD      |
+-------------+-----------------+------------------+---------------------+
| CYCLE       |      2293651741 |                1 |                  28 |
| NANOSECOND  |      1000000000 |                1 |                  48 |
| MICROSECOND |         1000000 |                1 |                  52 |
| MILLISECOND |            1000 |             1000 | 9223372036854775807 |
| TICK        |             106 |                1 |                 496 |
+-------------+-----------------+------------------+---------------------+
```