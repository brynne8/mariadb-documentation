# Filesort with Small LIMIT Optimization

## Optimization description

MySQL 5.6 has an optimization for `ORDER BY ...LIMIT n` queries. When `n` is sufficiently small, the optimizer will use a [priority queue](http://en.wikipedia.org/wiki/Priority_queue)  for sorting. The alternative is, roughly speaking, to sort the entire output and then pick only first `n` rows.

The optimization was ported into [MariaDB 10.0](/kb/en/what-is-mariadb-100/) in version 10.0.0. The server would not give any indication of whether the optimization was used, though. (This is how the feature was designed by Oracle. In MySQL 5.6, the only way one can see this feature is to examine the optimizer_trace, which is not currently supported by MariaDB).

## Optimization visibility in MariaDB

##### MariaDB starting with [10.0.13](/kb/en/mariadb-10013-release-notes/)

Starting from [MariaDB 10.0.13](/kb/en/mariadb-10013-release-notes/), there are two ways to check whether filesort has used a priority queue.

### Status variable

The first way is to check the [Sort_priority_queue_sorts](/kb/en/server-status-variables/#sort_priority_queue_sorts) status variable. It shows the number of times that sorting was done through a priority queue. (The total number of times sorting was done is a sum [Sort_range](/kb/en/server-status-variables/#sort_range) and [Sort_scan](/kb/en/server-status-variables/#sort_scan)).

### Slow query log

The second way is to check the slow query log. When one uses [Extended statistics in the slow query log](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/slow-query-log-extended-statistics) and  specifies [log_slow_verbosity=query_plan](/kb/en/server-system-variables/#log_slow_verbosity), [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log) entries look like this

```sql
# Time: 140714 18:30:39
# User@Host: root[root] @ localhost []
# Thread_id: 3  Schema: test  QC_hit: No
# Query_time: 0.053857  Lock_time: 0.000188  Rows_sent: 11  Rows_examined: 100011
# Full_scan: Yes  Full_join: No  Tmp_table: No  Tmp_table_on_disk: No
# Filesort: Yes  Filesort_on_disk: No  Merge_passes: 0  Priority_queue: Yes
SET timestamp=1405348239;SET timestamp=1405348239;
select * from t1 where col1 between 10 and 20 order by col2 limit 100;
```

Note the "Priority_queue: Yes" on the last comment line. (`pt-query-digest` is able to parse slow query logs with the Priority_queue field)

As for `EXPLAIN`, it will give no indication whether filesort uses priority queue or the generic quicksort and merge algorithm. `Using filesort` will be shown in both cases, by both MariaDB and MySQL.

## See also

- [LIMIT Optimization](http://dev.mysql.com/doc/refman/5.6/en/limit-optimization.html) page in the MySQL 5.6 manual (search for "priority queue").
- MySQL WorkLog entry, [WL#1393](http://dev.mysql.com/worklog/task/?id=1393)
- [MDEV-415](https://jira.mariadb.org/browse/MDEV-415), [MDEV-6430](https://jira.mariadb.org/browse/MDEV-6430)