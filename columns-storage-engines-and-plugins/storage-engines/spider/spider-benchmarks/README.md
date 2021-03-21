# Spider Benchmarks

This is best run on a cluster of 3 nodes intel NUC servers 12 virtual cores model name	: Intel<span>®</span> Core(TM) i3-3217U CPU @ 1.80GHz

All nodes have been running a mysqlslap client attached to the local spider node in the best run.

```sql
/usr/local/skysql/mysql-client/bin/mysqlslap --user=skysql --password=skyvodka --host=192.168.0.201 --port=5012 -i1000000 -c32 -q "insert into test(c) values('0-31091-138522330')" --create-schema=test
```

`spider_conn_recycle_mode=1;`

<img src="/kb/en/spider-benchmarks/+image/spbench4" alt="spbench4" title="spbench4">

The read point select is produce with a 10M rows sysbench table

<img src="/kb/en/spider-benchmarks/+image/spbench5" alt="spbench5" title="spbench5">

The write insert a single string into a memory table

<img src="/kb/en/spider-benchmarks/+image/spbench6" alt="spbench6" title="spbench6">

Before Engine Condition Push Down patch .

<img src="/kb/en/spider-benchmarks/+image/benchspider7" alt="benchspider7" title="benchspider7">

Spider can benefit by 10% additional performance with Independent Storage Engine Statistics.

```sql
set global use_stat_tables='preferably';
USE backend; 
ANALYZE TABLE sbtest; 
```