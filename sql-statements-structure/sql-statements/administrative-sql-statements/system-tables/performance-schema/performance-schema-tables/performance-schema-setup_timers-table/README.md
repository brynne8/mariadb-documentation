# Performance Schema setup_timers Table

## Description

The `setup_timers` table shows the currently selected event timers.

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>NAME</code></td><td>Type of instrument the timer is used for.</td></tr>
<tr><td><code>TIMER_NAME</code></td><td>Timer applying to the instrument type. Can be modified.</td></tr>
</tbody></table>

The `TIMER_NAME` value can be changed to choose a different timer, and can be any non-NULL value in the [performance_timers.TIMER_NAME](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-performance_timers-table) column.

If you modify the table, monitoring is immediately affected, and currently monitored events would use a combination of old and new timers, which is probably undesirable. It is best to reset the Performance Schema statistics if you make changes to this table.

## Example

```sql
SELECT * FROM setup_timers;
+-----------+-------------+
| NAME      | TIMER_NAME  |
+-----------+-------------+
| idle      | MICROSECOND |
| wait      | CYCLE       |
| stage     | NANOSECOND  |
| statement | NANOSECOND  |
+-----------+-------------+
```