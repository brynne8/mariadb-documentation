# MyRocks and Replication

Details about how MyRocks works with [replication](/kb/en/high-availability-performance-tuning-mariadb-replication/).

## MyRocks and Statement-Based Replication

[Statement-based](/kb/en/binary-log-formats/#statement-based) replication (SBR) works as follows: SQL statements are executed on the master (possibly concurrently). They are written into the binlog  (this fixes their ordering, "a serialization"). The slave then reads the binlog and executes the statements in their binlog order.

In order to prevent data drift, serial execution of statements on the slave must have the same effect as concurrent execution of these statements on the master. In other words, transaction isolation on the master must be close to `SERIALIZABLE` transaction isolation level (This is not a strict mathematical proof but shows the idea).

InnoDB achieves this by (almost) supporting `SERIALIZABLE` transactional isolation level.  It does so by supporting  "Gap Locks". MyRocks doesn't support `SERIALIZABLE` isolation, and it doesn't support gap locks.

Because of that, generally one cannot use MyRocks and statement-based replication.

Updating a MyRocks table while having SBR on, will result in an error as follow:

```sql
ERROR 4056 (HY000): Can't execute updates on master with binlog_format != ROW.
```

### Can One Still Use SBR with MyRocks?

Yes. In many cases, database applications run a restricted set of SQL statements, and it's possible to prove that lack of Gap Lock support is not a problem and data skew will not occur.

In that case, one can set `@@rocksdb_unsafe_for_binlog=1`  and MyRocks will work with SBR. The user is however responsible for making sure their queries are not causing a data skew.

## Read-Free Slave

TODO

## Differences From Upstream MyRocks

MyRocks upstream (that is, Facebook's MySQL branch) has a number of unique replication enhancements. These are available in upstream's version of MyRocks but not in MariaDB's version of MyRocks.

- Read-Free Replication (see [https://github.com/facebook/mysql-5.6/wiki/Read-Free-Replication](https://github.com/facebook/mysql-5.6/wiki/Read-Free-Replication)) TODO

- <code class="unknown_macro">&lt;&lt;<span class="macro_name">unique</span><span class="macro_arg_string">_check_lag_threshold</span>&gt;&gt;</code>. This is FB/MySQL-5.6 feature where unique checks are disabled if replication lag exceeds a certain threshold.
- <code class="unknown_macro">&lt;&lt;<span class="macro_name">slave</span><span class="macro_arg_string">_gtid_info=OPTIMIZED</span>&gt;&gt;</code>. This is said to be:

```sql
<<quote>>
"Whether SQL threads update mysql.slave_gtid_info table. If this value "
"is OPTIMIZED, updating the table is done inside storage engines to "
"avoid MySQL layer's performance overhead",
<</quote>>```