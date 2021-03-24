# TIME_MS column in INFORMATION_SCHEMA.PROCESSLIST

In MariaDB, an extra column <code class="highlight fixed" style="white-space:pre-wrap">TIME_MS</code> has been added to the
[INFORMATION_SCHEMA.PROCESSLIST](/kb/en/information-schema-processlist-table/) table. This column shows the same information as the column '<code class="highlight fixed" style="white-space:pre-wrap">TIME</code>', but in units of
milliseconds with microsecond precision (the unit and precision of the
<code class="highlight fixed" style="white-space:pre-wrap">TIME</code> column is one second).

For details about microseconds support in MariaDB, see [microseconds in MariaDB](/built-in-functions/date-time-functions/microseconds-in-mariadb/).

The value displayed in the <code class="highlight fixed" style="white-space:pre-wrap">TIME</code> and
<code class="highlight fixed" style="white-space:pre-wrap">TIME_MS</code> columns is the period of time that the given
thread has been in its current state. Thus it can be used to check for example
how long a thread has been executing the current query, or for how long it has
been idle.

```sql
select id, time, time_ms, command, state from
   information_schema.processlist, (select sleep(2)) t;
+----+------+----------+---------+-----------+
| id | time | time_ms  | command | state     |
+----+------+----------+---------+-----------+
| 37 |    2 | 2000.493 | Query   | executing |
+----+------+----------+---------+-----------+
```

Note that as a difference to MySQL, in MariaDB the <code class="highlight fixed" style="white-space:pre-wrap">TIME</code>
column (and also the <code class="highlight fixed" style="white-space:pre-wrap">TIME_MS</code> column) are not affected by
any setting of [@TIMESTAMP](/kb/en/server-system-variables/#timestamp). This means that it can be
reliably used also for threads that change <code class="highlight fixed" style="white-space:pre-wrap">@TIMESTAMP</code> (such
as the [replication](/replication/) SQL thread). See also [MySQL Bug #22047](http://bugs.mysql.com/bug.php?id=22047).

As a consequence of this, the <code class="highlight fixed" style="white-space:pre-wrap">TIME</code> column of 
<code class="highlight fixed" style="white-space:pre-wrap">SHOW FULL PROCESSLIST</code> and
<code class="highlight fixed" style="white-space:pre-wrap">INFORMATION_SCHEMA.PROCESSLIST</code> can not be used to determine
if a slave is lagging behind. For this, use instead the
<code class="highlight fixed" style="white-space:pre-wrap">Seconds_Behind_Master</code> column in the output of 
[SHOW SLAVE STATUS](/kb/en/show-slave-status/).

The addition of the TIME_MS column is based on the microsec_process patch,
developed by [Percona](http://www.percona.com/).