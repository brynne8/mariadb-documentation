# Performance Schema file_summary_by_instance Table

The [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/) `file_summary_by_instance` table contains file events summarized by instance. As of [MariaDB 10.0](/kb/en/what-is-mariadb-100/), it contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>FILE_NAME</code></td><td>File name.</td></tr>
<tr><td><code>EVENT_NAME</code></td><td>Event name.</td></tr>
<tr><td><code>OBJECT_INSTANCE_BEGIN</code></td><td>Address in memory. Together with <code>FILE_NAME</code> and <code>EVENT_NAME</code> uniquely identifies a row.</td></tr>
<tr><td><code>COUNT_STAR</code></td><td>Number of summarized events</td></tr>
<tr><td><code>SUM_TIMER_WAIT</code></td><td>Total wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MIN_TIMER_WAIT</code></td><td>Minimum wait time of the summarized events that are timed.</td></tr>
<tr><td><code>AVG_TIMER_WAIT</code></td><td>Average wait time of the summarized events that are timed.</td></tr>
<tr><td><code>MAX_TIMER_WAIT</code></td><td>Maximum wait time of the summarized events that are timed.</td></tr>
<tr><td><code>COUNT_READ</code></td><td>Number of all read operations, including <code>FGETS</code>, <code>FGETC</code>, <code>FREAD</code>, and <code>READ</code>.</td></tr>
<tr><td><code>SUM_TIMER_READ</code></td><td>Total wait time of all read operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_READ</code></td><td>Minimum wait time of all read operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_READ</code></td><td>Average wait time of all read operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_READ</code></td><td>Maximum wait time of all read operations that are timed.</td></tr>
<tr><td><code>SUM_NUMBER_OF_BYTES_READ</code></td><td>Bytes read by read operations.</td></tr>
<tr><td><code>COUNT_WRITE</code></td><td>Number of all write operations, including <code>FPUTS</code>, <code>FPUTC</code>, <code>FPRINTF</code>, <code>VFPRINTF</code>, <code>FWRITE</code>, and <code>PWRITE</code>.</td></tr>
<tr><td><code>SUM_TIMER_WRITE</code></td><td>Total wait time of all write operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_WRITE</code></td><td>Minimum wait time of all write operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_WRITE</code></td><td>Average wait time of all write operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_WRITE</code></td><td>Maximum wait time of all write operations that are timed.</td></tr>
<tr><td><code>SUM_NUMBER_OF_BYTES_WRITE</code></td><td>Bytes written by write operations.</td></tr>
<tr><td><code>COUNT_MISC</code></td><td>Number of all miscellaneous operations not counted above, including <code>CREATE</code>, <code>DELETE</code>, <code>OPEN</code>, <code>CLOSE</code>, <code>STREAM_OPEN</code>, <code>STREAM_CLOSE</code>, <code>SEEK</code>, <code>TELL</code>, <code>FLUSH</code>, <code>STAT</code>, <code>FSTAT</code>, <code>CHSIZE</code>, <code>RENAME</code>, and <code>SYNC</code>.</td></tr>
<tr><td><code>SUM_TIMER_MISC</code></td><td>Total wait time of all miscellaneous operations that are timed.</td></tr>
<tr><td><code>MIN_TIMER_MISC</code></td><td>Minimum wait time of all miscellaneous operations that are timed.</td></tr>
<tr><td><code>AVG_TIMER_MISC</code></td><td>Average wait time of all miscellaneous operations that are timed.</td></tr>
<tr><td><code>MAX_TIMER_MISC</code></td><td>Maximum wait time of all miscellaneous operations that are timed.</td></tr>
</tbody></table>

Before MariaDB 10, the table contained only the `FILE_NAME`, `EVENT_NAME`, `COUNT_READ`, `COUNT_WRITE`, `SUM_NUMBER_OF_BYTES_READ` and `SUM_NUMBER_OF_BYTES_WRITE` columns.

I/O operations can be avoided by caching, in which case they will not be recorded in this table.

You can [TRUNCATE](/sql-statements-structure/sql-statements/table-statements/truncate-table/) the table, which will reset all counters to zero.

## Example

```sql
SELECT * FROM file_summary_by_instance\G
...
*************************** 204. row ***************************
                FILE_NAME: /var/lib/mysql/test/db.opt
               EVENT_NAME: wait/io/file/sql/dbopt
    OBJECT_INSTANCE_BEGIN: 140578961971264
               COUNT_STAR: 6
           SUM_TIMER_WAIT: 39902495024
           MIN_TIMER_WAIT: 177888
           AVG_TIMER_WAIT: 6650415692
           MAX_TIMER_WAIT: 21026400404
               COUNT_READ: 1
           SUM_TIMER_READ: 21026400404
           MIN_TIMER_READ: 21026400404
           AVG_TIMER_READ: 21026400404
           MAX_TIMER_READ: 21026400404
 SUM_NUMBER_OF_BYTES_READ: 65
              COUNT_WRITE: 0
          SUM_TIMER_WRITE: 0
          MIN_TIMER_WRITE: 0
          AVG_TIMER_WRITE: 0
          MAX_TIMER_WRITE: 0
SUM_NUMBER_OF_BYTES_WRITE: 0
               COUNT_MISC: 5
           SUM_TIMER_MISC: 18876094620
           MIN_TIMER_MISC: 177888
           AVG_TIMER_MISC: 3775218924
           MAX_TIMER_MISC: 18864558060
*************************** 205. row ***************************
                FILE_NAME: /var/log/mysql/mariadb-bin.000157
               EVENT_NAME: wait/io/file/sql/binlog
    OBJECT_INSTANCE_BEGIN: 140578961971968
               COUNT_STAR: 6
           SUM_TIMER_WAIT: 73985877680
           MIN_TIMER_WAIT: 251136
           AVG_TIMER_WAIT: 12330979468
           MAX_TIMER_WAIT: 73846656340
               COUNT_READ: 0
           SUM_TIMER_READ: 0
           MIN_TIMER_READ: 0
           AVG_TIMER_READ: 0
           MAX_TIMER_READ: 0
 SUM_NUMBER_OF_BYTES_READ: 0
              COUNT_WRITE: 2
          SUM_TIMER_WRITE: 62583004
          MIN_TIMER_WRITE: 27630192
          AVG_TIMER_WRITE: 31291284
          MAX_TIMER_WRITE: 34952812
SUM_NUMBER_OF_BYTES_WRITE: 369
               COUNT_MISC: 4
           SUM_TIMER_MISC: 73923294676
           MIN_TIMER_MISC: 251136
           AVG_TIMER_MISC: 18480823560
           MAX_TIMER_MISC: 73846656340
```