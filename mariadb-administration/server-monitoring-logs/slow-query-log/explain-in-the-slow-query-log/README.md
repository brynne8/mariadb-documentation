# EXPLAIN in the Slow Query Log

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

Starting from [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/), it is possible to have EXPLAIN output printed in the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log).

## Switching it On

[EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain) output can be switched on by specifying the "<code class="highlight fixed" style="white-space:pre-wrap">explain</code>" keyword in the [log_slow_verbosity](/kb/en/server-system-variables/#log_slow_verbosity) system variable. Alternatively, you can set with the <code class="highlight fixed" style="white-space:pre-wrap">log-slow-verbosity</code> command line argument.

```sql
[mysqld]
log-slow-verbosity=query_plan,explain
```

EXPLAIN output will only be recorded if the slow query log is written to a file (and not to a table - see [Writing logs into tables](/mariadb-administration/server-monitoring-logs/writing-logs-into-tables)). This limitation also applies to other extended statistics that are written into the slow query log.

## What it Looks Like

When explain recording is on, slow query log entries look like this:

```sql
# Time: 131112 17:03:32
# User@Host: root[root] @ localhost []
# Thread_id: 2  Schema: dbt3sf1  QC_hit: No
# Query_time: 5.524103  Lock_time: 0.000337  Rows_sent: 1  Rows_examined: 65633
#
# explain: id   select_type     table   type    possible_keys   key     key_len ref     rows    Extra
# explain: 1    SIMPLE  nation  ref     PRIMARY,n_name  n_name  26      const   1       Using where; Using index
# explain: 1    SIMPLE  customer        ref     PRIMARY,i_c_nationkey   i_c_nationkey   5       dbt3sf1.nation.n_nationkey      3145    Using index
# explain: 1    SIMPLE  orders  ref     i_o_custkey     i_o_custkey     5       dbt3sf1.customer.c_custkey      7       Using index
#
SET timestamp=1384261412;
select count(*) from customer, orders, nation where c_custkey=o_custkey and c_nationkey=n_nationkey and n_name='GERMANY';
```

EXPLAIN lines start with <code class="fixed" style="white-space:pre-wrap"># explain:</code>.

## See Also

- [MDEV-407](https://jira.mariadb.org/browse/MDEV-407)