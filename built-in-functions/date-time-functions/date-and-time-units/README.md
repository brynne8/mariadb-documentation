# Date and Time Units

The `INTERVAL` keyword can be used to add or subtract a time interval of time to a [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime), [DATE](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/date) or [TIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/time) value.

The syntax is:

```sql
INTERVAL time_quantity time_unit
```

For example, the `SECOND` unit is used below by the [DATE_ADD()](/built-in-functions/date-time-functions/date_add) function:

```sql
SELECT '2008-12-31 23:59:59' + INTERVAL 1 SECOND;
+-------------------------------------------+
| '2008-12-31 23:59:59' + INTERVAL 1 SECOND |
+-------------------------------------------+
| 2009-01-01 00:00:00                       |
+-------------------------------------------+
```

The following units are valid:

<table><tbody><tr><th>Unit</th><th>Description</th></tr>
<tr><td><code>MICROSECOND</code></td><td>Microseconds</td></tr>
<tr><td><code>SECOND</code></td><td>Seconds</td></tr>
<tr><td><code>MINUTE</code></td><td>Minutes</td></tr>
<tr><td><code>HOUR</code></td><td>Hours</td></tr>
<tr><td><code>DAY</code></td><td>Days</td></tr>
<tr><td><code>WEEK</code></td><td>Weeks</td></tr>
<tr><td><code>MONTH</code></td><td>Months</td></tr>
<tr><td><code>QUARTER</code></td><td>Quarters</td></tr>
<tr><td><code>YEAR</code></td><td>Years</td></tr>
<tr><td><code>SECOND_MICROSECOND</code></td><td>Seconds.Microseconds</td></tr>
<tr><td><code>MINUTE_MICROSECOND</code></td><td>Minutes.Seconds.Microseconds</td></tr>
<tr><td><code>MINUTE_SECOND</code></td><td>Minutes.Seconds</td></tr>
<tr><td><code>HOUR_MICROSECOND</code></td><td>Hours.Minutes.Seconds.Microseconds</td></tr>
<tr><td><code>HOUR_SECOND</code></td><td>Hours.Minutes.Seconds</td></tr>
<tr><td><code>HOUR_MINUTE</code></td><td>Hours.Minutes</td></tr>
<tr><td><code>DAY_MICROSECOND</code></td><td>Days Hours.Minutes.Seconds.Microseconds</td></tr>
<tr><td><code>DAY_SECOND</code></td><td>Days Hours.Minutes.Seconds</td></tr>
<tr><td><code>DAY_MINUTE</code></td><td>Days Hours.Minutes</td></tr>
<tr><td><code>DAY_HOUR</code></td><td>Days Hours</td></tr>
<tr><td><code>YEAR_MONTH</code></td><td>Years-Months</td></tr>
</tbody></table>

The time units containing an underscore are composite; that is, they consist of multiple base time units. For base time units, `time_quantity` is an integer number. For composite units, the quantity must be expressed as a string with multiple integer numbers separated by any punctuation character.

Example of composite units:

```sql
INTERVAL '2:2' YEAR_MONTH
INTERVAL '1:30:30' HOUR_SECOND
INTERVAL '1!30!30' HOUR_SECOND -- same as above
```

Time units can be used in the following contexts:

- after a [+](/built-in-functions/numeric-functions/addition-operator) or a [-](/sql-statements-structure/operators/arithmetic-operators/subtraction-operator-) operator;
- with the following `DATE` or `TIME` functions: [ADDDATE()](/built-in-functions/date-time-functions/adddate), [SUBDATE()](/built-in-functions/date-time-functions/subdate), [DATE_ADD()](/built-in-functions/date-time-functions/date_add), [DATE_SUB()](/built-in-functions/date-time-functions/date_sub), [TIMESTAMPADD()](/built-in-functions/date-time-functions/timestampadd), [TIMESTAMPDIFF()](/built-in-functions/date-time-functions/timestampdiff), [EXTRACT()](/built-in-functions/date-time-functions/extract);
- in the `ON SCHEDULE` clause of [CREATE EVENT](/sql-statements-structure/sql-statements/data-definition/create/create-event) and [ALTER EVENT](/programming-customizing-mariadb/triggers-events/event-scheduler/alter-event).
- when defining a [partitioning](/kb/en/create-table/#partitions) `BY SYSTEM_TIME`

## See also

- [Date and time literals](/sql-statements-structure/sql-language-structure/date-and-time-literals)