# Slow Query Log Extended Statistics

## Overview

- Added extra logging to slow log of 'Thread_id, Schema, Query Cache hit, Rows
  sent and Rows examined'
- Added optional logging to slow log, through log_slow_verbosity, of query plan
  statistics
- Added new session variables log_slow_rate_limit, log_slow_verbosity,
  log_slow_filter
- Added log-slow-file as synonym for 'slow-log-file', as most slow-log
  variables starts with 'log-slow'
- Added log-slow-time as synonym for long-query-time.

## New Session Variables

### log_slow_verbosity

You can set the verbosity of what's logged to the slow query log by setting the
the [log_slow_verbosity](/kb/en/server-system-variables/#log_slow_verbosity) variable to a combination of the following values:

- <code class="highlight fixed" style="white-space:pre-wrap">Query_plan</code>
<ul start="1"><li>For select queries, log information about the query plan. This includes
   "Full_scan", "Full_join", "Tmp_table", "Tmp_table_on_disk", "Filesort",
   "Filesort_on_disk" and number of "Merge_passes during sorting"
</li></ul>
- <code class="highlight fixed" style="white-space:pre-wrap">explain</code> (Starting from 10.0.5)
<ul start="1"><li>EXPLAIN output is logged in the slow query log. See [explain-in-the-slow-query-log](/mariadb-administration/server-monitoring-logs/slow-query-log/explain-in-the-slow-query-log/) for details.
</li></ul>
- <code class="highlight fixed" style="white-space:pre-wrap">Innodb</code>
<ul start="1"><li>Reserved for future use.
</li></ul>

The default value is ' ', to be compatible with MySQL 5.1.

Multiple options are separated by ','.

log_slow_verbosity is not supported when log_output='TABLE'.

### log_slow_filter

You can define which queries to log to the slow query log by setting the
variable [log_slow_filter](/kb/en/server-system-variables/#log_slow_filter) to a combination of the following values:

- <code class="highlight fixed" style="white-space:pre-wrap">admin</code>
<ul start="1"><li>Log administrative statements (create, optimize, drop etc...)
</li></ul>
- <code class="highlight fixed" style="white-space:pre-wrap">filesort</code>
<ul start="1"><li>Log statement if it uses filesort
</li></ul>
- <code class="highlight fixed" style="white-space:pre-wrap">filesort_on_disk</code>
<ul start="1"><li>Log statement if it uses filesort that needs temporary tables on disk
</li></ul>
- <code class="highlight fixed" style="white-space:pre-wrap">full_join</code>
<ul start="1"><li>Log statements that doesn't uses indexes to join tables
</li></ul>
- <code class="highlight fixed" style="white-space:pre-wrap">full_scan</code>
<ul start="1"><li>Log statements that uses full table scans
</li></ul>
- <code class="highlight fixed" style="white-space:pre-wrap">query_cache</code>
<ul start="1"><li>Log statements that are resolved by the query cache
</li></ul>
- <code class="highlight fixed" style="white-space:pre-wrap">query_cache_miss</code>
<ul start="1"><li>Log statements that are not resolved by the query cache
</li></ul>
- <code class="highlight fixed" style="white-space:pre-wrap">tmp_table</code>
<ul start="1"><li>Log statements that uses in memory temporary tables
</li></ul>
- <code class="highlight fixed" style="white-space:pre-wrap">tmp_table_on_disk</code>
<ul start="1"><li>Log statements that uses temporary tables on disk
</li></ul>

Multiple options are separated by ','. If you don't specify any options everything will be logged.

### log_slow_rate_limit

The [log_slow_rate_limit](/kb/en/server-system-variables/#log_slow_rate_limit) variable limits logging to the slow query log by not logging every query (only one query / log_slow_rate_limit is logged). This
is mostly useful when debugging and you get too much information to the slow
query log.

Note that in any case, only queries that takes longer than <strong>log_slow_time</strong> or
<strong>long_query_time</strong>' are logged (as before).

This addition is based on the
[microslow](http://www.percona.com/percona-builds/Percona-SQL-5.0/Percona-SQL-5.0-5.0.87-b20/patches/microslow_innodb.patch)
patch from [Percona](http://www.percona.com/).