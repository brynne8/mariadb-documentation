# SHOW PROCESSLIST

## Syntax

```sql
SHOW [FULL] PROCESSLIST
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">SHOW PROCESSLIST</code> shows you which threads are running. You
can also get this information from the
[information_schema.PROCESSLIST](/kb/en/information-schema-processlist-table/) table or the [mysqladmin processlist](/clients-utilities/mysqladmin/) command. If you have the 
<code class="highlight fixed" style="white-space:pre-wrap">[PROCESS privilege](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-privileges/)</code>, you can see all threads.
Otherwise, you can see only your own threads (that is, threads associated with
the MariaDB account that you are using). If you do not use the
<code class="highlight fixed" style="white-space:pre-wrap">FULL</code> keyword, only the first 100 characters of each
statement are shown in the Info field.

The columns shown in `SHOW PROCESSLIST` are:

<table><tbody><tr><th>Name</th><th>Description</th></tr>
<tr><td><strong><code>ID</code></strong></td><td>The client's process ID.</td></tr>
<tr><td><strong><code>USER</code></strong></td><td>The username associated with the process.</td></tr>
<tr><td><strong><code>HOST</code></strong></td><td>The host the client is connected to.</td></tr>
<tr><td><strong><code>DB</code></strong></td><td>The default database of the process (NULL if no default).</td></tr>
<tr><td><strong><code>COMMAND</code></strong></td><td>The command type. See <a href="/kb/en/thread-command-values/">Thread Command Values</a>.</td></tr>
<tr><td><strong><code>TIME</code></strong></td><td>The amount of time, in seconds, the process has been in its current state. For a replica SQL thread before <a href="/kb/en/what-is-mariadb-101/">MariaDB 10.1</a>, this is the time in seconds between the last replicated event's timestamp and the replica machine's real time.</td></tr>
<tr><td><strong><code>STATE</code></strong></td><td>See <a href="/kb/en/thread-states/">Thread States</a>.</td></tr>
<tr><td><strong><code>INFO</code></strong></td><td>The statement being executed.</td></tr>
<tr><td><strong><code>PROGRESS</code></strong></td><td>The total progress of the process (0-100%) (see <a href="/kb/en/progress-reporting/">Progress Reporting</a>).</td></tr>
</tbody></table>

See `TIME_MS` column in [information_schema.PROCESSLIST](/kb/en/time_ms-column-in-information_schemaprocesslist/) for differences in the `TIME` column between MariaDB and MySQL.

The [information_schema.PROCESSLIST](/kb/en/information-schema-processlist-table/)  table contains the following additional columns:

<table><tbody><tr><th>Name</th><th>Description</th></tr>
<tr><td><strong><code>TIME_MS</code></strong></td><td>The amount of time, in milliseconds, the process has been in its current state.</td></tr>
<tr><td><strong><code>STAGE</code></strong></td><td>The stage the process is currently in.</td></tr>
<tr><td><strong><code>MAX_STAGE</code></strong></td><td>The maximum number of stages.</td></tr>
<tr><td><strong><code>PROGRESS</code></strong></td><td>The progress of the process within the current stage (0-100%).</td></tr>
<tr><td><strong><code>MEMORY_USED</code></strong></td><td>The amount of memory used by the process.</td></tr>
<tr><td><strong><code>EXAMINED_ROWS</code></strong></td><td>The number of rows the process has examined.</td></tr>
<tr><td><strong><code>QUERY_ID</code></strong></td><td>Query ID.</td></tr>
</tbody></table>

Note that the `PROGRESS` field from the information schema, and the `PROGRESS` field from `SHOW PROCESSLIST` display different results. `SHOW PROCESSLIST` shows the total progress, while the information schema shows the progress for the current stage only.

Threads can be killed using their thread_id, or, since [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/), their query_id, with the [KILL](/kb/en/data-manipulation-kill-connection-query/) statement.

Since queries on this table are locking, if the [performance_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/) is enabled, you may want to query the [THREADS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table/) table instead.

## Examples

```sql
SHOW PROCESSLIST;
+----+-----------------+-----------+------+---------+------+------------------------+------------------+----------+
| Id | User            | Host      | db   | Command | Time | State                  | Info             | Progress |
+----+-----------------+-----------+------+---------+------+------------------------+------------------+----------+
|  2 | event_scheduler | localhost | NULL | Daemon  | 2693 | Waiting on empty queue | NULL             |    0.000 |
|  4 | root            | localhost | NULL | Query   |    0 | Table lock             | SHOW PROCESSLIST |    0.000 |
+----+-----------------+-----------+------+---------+------+------------------------+------------------+----------+
```