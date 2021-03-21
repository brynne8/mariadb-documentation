# xtstat

`xtstat` can be used to monitor all internal activity of [PBXT](/kb/en/pbxt/).

`xtstat` polls the `INFORMATION_SCHEMA.PBXT_STATISTICS` table. The poll interval can be set using the <code class="fixed" style="white-space:pre-wrap">--delay</code> option, and is 1 second by default.

For most statistics, `xtstat` will display the difference in values between the current and previous polls. For example, if bytes written current value is 1000, and on the previous call it was 800, then `xtstat` will display 200. This means that 200 bytes were written to disk in the intervening period.

## Using `xtstat`

Invoke xtstat as follows:

```sql
$ xtstat [ options ]
```

For example, to poll every 10 seconds:

```sql
xtstat -D10
```

Note that statistic counters are never reset, even if a rollback occurs. For example, if an <code class="fixed" style="white-space:pre-wrap"><span class="k">UPDATE</span>
</code> statement is rolled back, `xtstat` will still indicate that one write statement (see stat-write below) was executed.

If MariaDB shuts down or crashes, `xtstat` will attempt to reconnect. `xtstat` can be terminated any time using the `CTRL-C` key cimbination.

Before [PBXT](/kb/en/pbxt/) has recovered, not all statistics are available. In particular, the statistics relating to PBXT background threads are not available (including the `sweep` and `chkpnt` statistics).

### Command line options

`xtstat` options are as follows:

<table><tbody><tr><th>Option<code class="fixed" style="white-space:pre-wrap">                                               </code></th><th>Description</th></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">-?, --help</code></td><td>Prints help text.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">-h, --host=value</code></td><td>Connect to host.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">-u, --user=value</code></td><td>User for login if not current user.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">-p, --password[=value]</code></td><td>Password to use when connecting to server. If password is not given it's asked from the tty.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">-d, --database=value</code></td><td>Database to be used (<code class="fixed" style="white-space:pre-wrap">pbxt</code> or <code class="fixed" style="white-space:pre-wrap">information_schema</code> required), default is <code class="fixed" style="white-space:pre-wrap">information_schema</code></td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">-P, --port=value</code></td><td>Port number to use for connection.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">-S, --socket=value</code></td><td>Socket file to use for connection.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">-D, --delay=value</code></td><td>Delay in seconds between polls of the database.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">--protocol=value</code></td><td>Connection protocol to use: <code class="fixed" style="white-space:pre-wrap">default/tcp/socket/pipe/memory</code></td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">--display=value</code></td><td>Columns to display: use short names separated by <code>|</code> (the pipe character), partial match allowed. Use <code class="fixed" style="white-space:pre-wrap">--display=all</code> to display all columns available.</td></tr>
</tbody></table>

Connection options will also be taken from the MySQL config file if available.

#### Size indicators

Values displayed by `xtstat` are either a time in milliseconds, a value in bytes, or a counter. If these values are too large to be displayed then the value is rounded and a size indicator is added.

The following size indicators are used:

<table><tbody><tr><td><code class="fixed" style="white-space:pre-wrap">K</code></td><td>:</td><td>Kilobytes <em>(1,024 bytes)</em></td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">M</code></td><td>:</td><td>Megabytes <em>(1,048,576 bytes)</em></td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">G</code></td><td>:</td><td>Gigabytes <em>(1,073,741,024 bytes)</em></td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">T</code></td><td>:</td><td>Terabytes <em>(1,099,511,627,776 bytes)</em></td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">t</code></td><td>:</td><td>thousands <em>(1,000s)</em></td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">m</code></td><td>:</td><td>millions <em>(1,000,000s)</em></td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">b</code></td><td>:</td><td>billions <em>(1,000,000,000s)</em></td></tr>
</tbody></table>

### Statistics

The following is a list of the statistics displayed by `xtstat`. Each statistic as a two-part display name. The first part is the category and the second part is the type.

You can select categories and types for display, as you require. For example <code class="fixed" style="white-space:pre-wrap">--display=read</code> will display all read activity, <code class="fixed" style="white-space:pre-wrap">--display=xact|stat</code> will display transaction and statement activity.

Note, for diagnostics it is best to capture all statistics. The reason is because you never now where a problem might turn up, so without certain statistics you may not be able to identify the problem.

<table><tbody><tr><th>Display<span>&nbsp;</span>name</th><th>Name</th><th>Description</th></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">time-curr</code></td><td>Current Time</td><td>The current time in seconds</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">time-msec</code></td><td>Time<span>&nbsp;</span>Since<span>&nbsp;</span>Last<span>&nbsp;</span>Call</td><td>Time passed in milliseconds since last statistics call</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">xact-commt</code></td><td>Commit Count</td><td>Number of transactions committed</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">xact-rollb</code></td><td>Rollback Count</td><td>Number of transactions rolled back</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">xact-waits</code></td><td>Wait for Xact Count</td><td>Number of times waited for another transaction</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">xact-dirty</code></td><td>Dirty Xact Count</td><td>Number of transactions still to be cleaned up. This also includes all the currently running transactions. Cleanup means that the Sweeper thread must still scan the transcation and collect/mark any "garbage" left by the transaction. Garbage is, for example, versions of rows that are no longer visiable by any transaction.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">stat-read</code></td><td>Read Statements</td><td>Number of SELECT statements</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">stat-write</code></td><td>Write Statements</td><td>Number of UPDATE/INSERT/DELETE statements</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">rec-in</code></td><td>Record Bytes Read</td><td>Bytes read from the record/row files</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">rec-out</code></td><td>Record Bytes Written</td><td>Bytes written to the record/row files. This data is transfered from the transaction logs to the handle data (xtd) and the row index files (xtr).</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">rec-syncs/ms</code></td><td>Record File Flushes</td><td>2 values separated by a '/': the number of flushes to data handle (.xtd) and row index (.xtr) files and the time taken in milliseconds to perform the flush operations.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">rec-hits</code></td><td>Record Cache Hits</td><td>Hits when accessing the record cache. The record cache caches the data handle (.xtd) and row index (.xtr) files.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">rec-miss</code></td><td>Record Cache Misses</td><td>Misses when accessing the record cache</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">rec-frees</code></td><td>Record Cache Frees</td><td>Number of record cache pages freed</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">rec-%use</code></td><td>Record Cache Usage</td><td>Percentage of record cache in use. This value is displayed by xtstat as a percentage of the total cache available, but the value returned by PBXT_STATISTICS table is in bytes used.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">ind-in</code></td><td>Index Bytes Read</td><td>Bytes read from the index files</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">ind-out</code></td><td>Index<span>&nbsp;</span>Bytes<span>&nbsp;</span>Written</td><td>Bytes written to the index files. This data is transfered from the index log files (ilog) to the index files (xti), during a consistent flush of the index.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">ind-syncs/ms</code></td><td>Index File Flushes</td><td>2 values separated by a '/': the number of flushes to index files and the time taken for the flush operations in milliseconds.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">ind-hits</code></td><td>Index Cache Hits</td><td>Hits when accessing the index cache</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">ind-miss</code></td><td>Index Cache Misses</td><td>Misses when accessing the index cache</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">ind-%use</code></td><td>Index Cache Usage</td><td>Percentage of index cache used. This value is displayed by xtstat as a percentage of the total cache available, but the value returned by PBXT_STATISTICS table is in bytes used.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">ilog-in</code></td><td>Index Log Bytes In</td><td>Bytes read from the index log files</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">ilog-out</code></td><td>Index Log Bytes Out</td><td>Bytes written to the index log files. This data is transfered from the index cache in main memory to the index log files (ilog) during a consistent flush of the index.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">ilog-syncs/ms</code></td><td>Index Log File Syncs</td><td>2 values separated by a '/': the number of flushes to index log files and the time taken for the flush operations in milliseconds</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">xlog-in</code></td><td>Xact Log Bytes In</td><td>Bytes read from the transaction log files</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">xlog-out</code></td><td>Xact Log Bytes Out</td><td>Bytes written to the transaction log files. This is data transfered from the transaction log buffer (pbxt_transaction_buffer_size) to the transaction log files (.xlog). This transfer occurs on commit or when the transaction log buffer is full.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">xlog-syncs</code></td><td>Xact Log File Syncs</td><td>Number of flushes to transaction log files</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">xlog-msec</code></td><td>Xact Log Sync Time</td><td>The time in milliseconds to flush transaction log files</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">xlog-hits</code></td><td>Xact Log Cache Hits</td><td>Hits when accessing the transaction log cache</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">xlog-miss</code></td><td>Xact Log Cache Misses</td><td>Misses when accessing the transaction log cache</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">xlog-%use</code></td><td>Xact Log Cache Usage</td><td>Percentage of transaction log cache used. This value is displayed by xtstat as a percentage of the total cache available, but the value returned by PBXT_STATISTICS table is in bytes used.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">data-in</code></td><td>Data Log Bytes In</td><td>Bytes read from the data log files</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">data-out</code></td><td>Data Log Bytes Out</td><td>Bytes written to the data log files. This data is transfered from the data log buffer (pbxt_log_buffer_size) to the data log files (.dlog), when the buffer is full, or on commit.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">data-syncs</code></td><td>Data Log File Syncs</td><td>Number of flushes to data log files</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">data-msec</code></td><td>Data Log Sync Time</td><td>The time in milliseconds spent flushing data log files</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">to-chkpt</code></td><td>Bytes to Checkpoint</td><td>Bytes written to the transaction log since the last checkpoint</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">to-write</code></td><td>Log Bytes to Write</td><td>Bytes written to the transaction log, still to be written to the database</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">to-sweep</code></td><td>Log Bytes to Sweep</td><td>Bytes written to the transaction log, still to be read by the Sweeper thread</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">sweep-waits</code></td><td>Sweeper<span>&nbsp;</span>Wait<span>&nbsp;</span>on<span>&nbsp;</span>Xact<span>&nbsp;</span><span>&nbsp;</span></td><td>Attempts to cleanup a transaction</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">scan-index</code></td><td>Index Scan Count</td><td>Number of index scans</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">scan-table</code></td><td>Table Scan Count</td><td>Number of table scans</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">row-sel</code></td><td>Select Row Count</td><td>Number of rows selected</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">row-ins</code></td><td>Insert Row Count</td><td>Number of rows inserted</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">row-upd</code></td><td>Update Row Count</td><td>Number of rows updated</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">row-del</code></td><td>Delete Row Count</td><td>Number of rows deleted</td></tr>
</tbody></table>

## More Information

Documentation on this page is based on the [xtstat documentation](http://primebase.org/documentation/index.php#xtstat) on the PrimeBase website.

Paul McCullagh's presentation from the 2010 User's Conference has some usage examples: [http://www.primebase.org/download/pbxt-uc-2010.pdf](http://www.primebase.org/download/pbxt-uc-2010.pdf)