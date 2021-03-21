# Parallel Replication

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

Parallel Replication was introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/).

From [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/), some writes, [replicated](/replication/standard-replication/) from the master can be executed in parallel (simultaneously) on the slave. Note that for parallel replication to work, both the master and slave need to be [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/) or later.

## Parallel Replication Overview

MariaDB replication in general takes place in three parts:

- Replication events are read from the master by the IO thread and queued in
the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/).
- Replication events are fetched one at a time by the SQL thread from the relay log
- Each event is applied on the slave to replicate all changes done on the
master.

Before MariaDB 10, the third step was also performed by the SQL thread; this
meant that only one event could execute at a time, and replication was
essentially single-threaded. Since MariaDB 10, the third step can optionally be
performed by a pool of separate replication worker threads, and thereby
potentially increase replication performance by applying multiple events in parallel.

## How to Enable Parallel Slave

To enable, specify [slave-parallel-threads=#](/kb/en/replication-and-binary-log-server-system-variables/#slave_parallel_threads) in your [my.cnf](/kb/en/mysqld-startup-options/) file as an argument to mysql.
Parallel replication can in addition be disabled on a per-multi-source
connection by setting [@@connection_name.slave-parallel-mode](/kb/en/replication-and-binary-log-server-system-variables/#slave_parallel_mode) to "none".

The value (#) of slave_parallel_threads specifies how many threads will be created in a pool of worker
threads used to apply events in parallel for *all* your slaves (this includes
[multi-source replication](/replication/standard-replication/multi-source-replication/)). If the value is zero,
then no worker threads are created, and old-style replication is used where
events are applied inside the SQL thread. Usually the value, if non-zero,
should be at least two times the number of multi-source master connections
used. It makes little sense to use only a single worker thread for one
connection; this will incur some overhead in inter-thread communication
between the SQL thread and the worker thread, but with just a single worker
thread events can not be applied in parallel anyway.

<code class="highlight fixed" style="white-space:pre-wrap">slave-parallel-threads=#</code> is a dynamic variable that can be changed without restarting mysqld. All slaves connections must however be stopped when changing the value.

## Configuring the Slave Parallel Mode

Parallel replication can be in-order or out-of-order:

- In-order executes transactions in parallel, but orders the
commit step of the transactions to happen in the exact same order as on the
master. Transactions are only executed in parallel to the extent that this can
be automatically verified to be possible without any conflicts. This means
that the use of parallelism is completely transparent to the application.
- Out-of-order can execute and commit transactions in different order on the
slave than originally on the master. This means that the application must be
tolerant to seeing updates occur in different order. The application is also
responsible for ensuring that there are no conflicts between transactions that
are replicated out-of-order. Out-of-order is only used in GTID mode and only
when explicitly enabled by the application, using the replication domain that
is part of the GTID.

### In-Order Parallel Replication

#### Optimistic Mode of In-Order Parallel Replication

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

Optimistic mode of in-order parallel replication was introduced in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/).

Optimistic mode of in-order parallel replication provides a lot of opportunities for parallel apply on the slave while still preserving exact transaction semantics from the point of view of applications. It is the default mode from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/).

Optimistic mode of in-order parallel replication can be configured by setting the <a undefined>slave_parallel_mode</a> system variable to `optimistic` on the slave.

Any transactional DML (INSERT/UPDATE/DELETE) is allowed to run in parallel, up
to the limit of [@@slave_domain_parallel_threads](/kb/en/replication-and-binary-log-server-system-variables/#slave_domain_parallel_threads). This may cause conflicts on
the slave, eg. if two transactions try to modify the same row. Any such
conflict is detected, and the latter of the two transactions is rolled back,
allowing the former to proceed. The latter transaction is then re-tried once
the former has completed.

The term "optimistic" is used for this mode, because the server optimistically
assumes that few conflicts will occur, and that the extra work spent rolling
back and retrying conflicting transactions is justified from the gain from
running most transactions in parallel.

There are a few heuristics to try to avoid needless conflicts. If a
transaction executed a row lock wait on the master, it will not be run in parallel
on the slave. Transactions can also be marked explicitly as potentially
conflicting on the master, by setting the variable
[@@skip_parallel_replication](/kb/en/replication-and-binary-log-server-system-variables/#skip_parallel_replication). More such heuristics may be added in later
MariaDB versions. There is a further [--slave-parallel-mode](/kb/en/replication-and-binary-log-server-system-variables/#slave_parallel_mode) called
"aggressive", where these heuristics are disabled, allowing even more
transactions to be applied in parallel.

Non-transactional DML and DDL is not safe to optimistically apply in parallel,
as it cannot be rolled back in case of conflicts. Thus, in optimistic mode,
non-transactional (such as MyISAM) updates are not applied in parallel with
earlier events (it is however possible to apply a MyISAM update in parallel
with a later InnoDB update). DDL statements are not applied in parallel with
any other transactions, earlier or later.

The different kind of transactions can be identified in the output of
[mysqlbinlog](/clients-utilities/mysqlbinlog/). For example:

```sql
#150324 13:06:26 server id 1  end_log_pos 6881 	GTID 0-1-42 ddl
...
#150324 13:06:26 server id 1  end_log_pos 7816 	GTID 0-1-47
...
#150324 13:06:26 server id 1  end_log_pos 8177  GTID 0-1-49 trans
/*!100101 SET @@session.skip_parallel_replication=1*//*!*/;
...
#150324 13:06:26 server id 1  end_log_pos 9836 	GTID 0-1-59 trans waited
```

GTID 0-1-42 is marked as being DDL. GTID 0-1-47 is marked as being
non-transactional DML, while GTID 0-1-49 is transactional DML (seen on the
"trans" keyword). GTID 0-1-49 was additionally run with
[@@skip_parallel_replication](/kb/en/replication-and-binary-log-server-system-variables/#skip_parallel_replication)
set on the master. GTID 0-1-59 is transactional DML that had a row lock wait when run on the
master (the "waited" keyword).

#### Aggressive Mode of In-Order Parallel Replication

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

Aggressive mode of in-order parallel replication was introduced in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/).

Aggressive mode of in-order parallel replication is very similar to optimistic mode. The main difference is that the slave does not consider whether transactions conflicted on the master when deciding whether to apply the transactions in parallel.

Aggressive mode of in-order parallel replication can be configured by setting the <a undefined>slave_parallel_mode</a> system variable to `aggressive` on the slave.

#### Conservative Mode of In-Order Parallel Replication

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

Conservative mode of in-order parallel replication was introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

Conservative mode of in-order parallel replication uses the [group commit](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/) on the master to discover potential for parallel apply of events on the slave. If two transactions commit together in a [group commit](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/) on the master, they are written into the binlog with the same commit id. Such events are certain to not conflict with each other, and they can be scheduled by the parallel replication to run in different worker threads.

Conservative mode of in-order parallel replication is the default mode until [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/), but it can also be configured by setting the <a undefined>slave_parallel_mode</a> system variable to `conservative` on the slave.

Two transactions that were committed separately on the master can potentially
conflict (eg. modify the same row of a table). Thus, the worker that applies
the second transaction will not start immediately, but wait until the first
transaction begins the commit step; at this point it is safe to start the
second transaction, as it can no longer disrupt the execution of the first
one.

Here is example output from [mysqlbinlog](/clients-utilities/mysqlbinlog/) that shows how GTID events are marked
with commit id. The GTID 0-1-47 has no commit id, and can not run in
parallel. The GTIDs 0-1-48 and 0-1-49 have the same commit id 630, and can
thus replicate in parallel with one another on a slave:

```sql
#150324 12:54:24 server id 1  end_log_pos 20052 	GTID 0-1-47 trans
...
#150324 12:54:24 server id 1  end_log_pos 20212 	GTID 0-1-48 cid=630 trans
...
#150324 12:54:24 server id 1  end_log_pos 20372 	GTID 0-1-49 cid=630 trans
```

In either case, when the two transactions reach the point where the low-level
commit happens and commit order is determined, the two commits are sequenced to
happen in the same order as on the master, so that operation is transparent to
applications.

The opportunities for parallel replication on slaves can be highly increased
if more transactions are committed in a [group commit](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/) on the master. This can be tuned
using the [binlog_commit_wait_count](/kb/en/replication-and-binary-log-server-system-variables/#binlog_commit_wait_count) and
[binlog_commit_wait_usec](/kb/en/replication-and-binary-log-server-system-variables/#binlog_commit_wait_usec) variables. If for example the
application can tolerate up to 50 milliseconds extra delay for transactions on
the master, one can set <code class="highlight fixed" style="white-space:pre-wrap">binlog_commit_wait_usec=50000</code> and
<code class="highlight fixed" style="white-space:pre-wrap">binlog_commit_wait_count=20</code> to get up to 20 transactions at
a time available for replication in parallel. Care must however be taken to
not set <code class="highlight fixed" style="white-space:pre-wrap">binlog_commit_wait_usec</code> too high, as this could
cause significant slowdown for applications that run a lot of small
transactions serially one after the other.

Note that even if there is no parallelism available from the master [group commit](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/), there is still an opportunity for speedup from in-order parallel
replication, since the actual commit steps of different transactions can run
in parallel. This can be particularly effective on a slave with binlog enabled
([log_slave_updates=1](/kb/en/replication-and-binary-log-server-system-variables/#log_slave_updates)), and more so if slave is configured
to be crash-safe ([sync_binlog=1](/kb/en/replication-and-binary-log-server-system-variables/#sync_binlog) and
[innodb_flush_log_at_trx_commit=1](/kb/en/xtradbinnodb-server-system-variables/#innodb_flush_log_at_trx_commit)), as this makes [group commit](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/) possible on the slave.

#### Minimal Mode of In-Order Parallel Replication

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

Minimal mode of in-order parallel replication was introduced in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/).

Minimal mode of in-order parallel replication <em>only</em>allows the commit step of
transactions to be applied in parallel; all other steps are applied serially.

Minimal mode of in-order parallel replication can be configured by setting the <a undefined>slave_parallel_mode</a> system variable to `minimal` on the slave.

### Out-of-Order Parallel Replication

Out-of-order parallel replication happens (only) when using GTID mode, when
GTIDs with different replication domains are used. The replication domain is
set by the DBA/application using the variable <code class="highlight fixed" style="white-space:pre-wrap">gtid_domain_id</code>.

Two transactions having GTIDs with different domain_id are scheduled to
different worker threads by parallel replication, and are allowed to execute
completely independently from each other. It is the responsibility of the
application to only set different domain_ids for transactions that are truly
independent, and are guaranteed to not conflict with each other. The
application must also be able to work correctly even though the transactions
with different domain_id are seen as committing in different order between the
slave and the master, and between different slaves.

Out-of-order parallel replication can potentially give more performance gain
than in-order parallel replication, since the application can explicitly
give more opportunities for running transactions in parallel than what the
server can determine on its own automatically.

One simple but effective usage is to run long-running statements, such as
ALTER TABLE, in a separate replication domain. This allows replication of
other transactions to proceed uninterrupted:

```sql
SET SESSION gtid_domain_id=1
ALTER TABLE t ADD INDEX myidx(b)
SET SESSION gtid_domain_id=0```

Normally, a long-running ALTER TABLE or other query will stall all following
transactions, causing the slave to become behind the master as least as long
time as it takes to run the long-running query. By using out-of-order parallel
replication by setting the replication domain id, this can be avoided. The
DBA/application must ensure that no conflicting transactions will be
replicated while the ALTER TABLE runs.

Another common opportunity for out-of-order parallel replication comes in
connection with multi-source replication. Suppose we have two different
masters M1 and M2, and we are using multi-source replication to have S1 as a
slave of both M1 and M2. S1 will apply events received from M1 in parallel
with events received from M2. If we now have a third-level slave S2 that
replicates from S1 as master, we want S2 to also be able to apply events that
originated on M1 in parallel with events that originated on M2. This can be
achieved with out-of-order parallel replication, by setting
<code class="highlight fixed" style="white-space:pre-wrap">gtid_domain_id</code> different on M1 and M2.

Note that there are no special restrictions on what operations can be
replicated in parallel using out-of-order; such operations can be on the same
database/schema or even on the same table. The only restriction is that the
operations must not conflict, that is they must be able to be applied in any
order and still end up with the same result.

When using out-of-order parallel replication, the current slave position in
the master's binlog becomes multi-dimensional - each replication domain can
have reached a different point in the master binlog at any one time. The
current position can be seen from the variable
<code class="highlight fixed" style="white-space:pre-wrap">gtid_slave_pos</code>. When the slave is stopped, restarted, or
switched to replicate from a different master using CHANGE MASTER, MariaDB
automatically handles restarting each replication domain at the appropriate
point in the binlog.

Out-of-order parallel replication is disabled when
[--slave-parallel-mode=minimal](/kb/en/replication-and-binary-log-server-system-variables/#slave_parallel_mode) (or none).

## Checking Worker Thread Status in SHOW PROCESSLIST

The worker threads will be listed as "system user" in [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/). Their
state will show the query they are currently working on, or it can show one of
these:

- "Waiting for work from main SQL threads". This means that the worker thread
is idle, no work is available for it at the moment.
- "Waiting for prior transaction to start commit before starting next
transaction". This means that the previous batch of transactions that
committed together on the master master has to complete first. This worker
thread is waiting for that to happen before it can start working on the
following batch.
- "Waiting for prior transaction to commit". This means that the transaction
has been executed by the worker thread. In order to ensure in-order commit,
the worker thread is waiting to commit until the previous transaction is ready
to commit before it.

## Expected Performance Gain

Here is an article showing up to ten times improvement when using parallel
replication: [http://kristiannielsen.livejournal.com/18435.html](http://kristiannielsen.livejournal.com/18435.html).

## Configuring the Maximum Size of the Parallel Slave Queue

The <a undefined>slave_parallel_max_queued</a> system variable can be used to configure the maximum size of the parallel slave queue. This system variable is only meaningful when parallel
replication is configured (i.e. when <a undefined>slave_parallel_threads</a> &gt; `0`).

When parallel replication is used, the [SQL thread](/kb/en/replication-threads/#slave-sql-thread) will read ahead in the relay logs, queueing events in memory while looking for opportunities for executing events in parallel. The <a undefined>slave_parallel_max_queued</a> system variable sets a
limit for how much memory it will use for this.

The configured value of the <a undefined>slave_parallel_max_queued</a> system variable is actually allocated for each [worker thread](/kb/en/replication-threads/#worker-threads), so the total allocation is actually equivalent to the following:

<a undefined>slave_parallel_max_queued</a> * <a undefined>slave_parallel_threads</a>

If this value is set too high, and the slave is far (eg. gigabytes of binlog)
behind the master, then the [SQL thread](/kb/en/replication-threads/#slave-sql-thread) can quickly read all of that and fill up memory with huge amounts of binlog events faster than the [worker threads](/kb/en/replication-threads/#worker-threads) can consume them.

On the other hand, if set too low, the [SQL thread](/kb/en/replication-threads/#slave-sql-thread) might not have sufficient space for queuing enough events to keep the worker threads busy, which could reduce performance. In this case, the [SQL thread](/kb/en/replication-threads/#slave-sql-thread) will have the [thread state](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-states/) that states `Waiting for room in worker thread event queue`. For example:

```sql
+----+-------------+-----------+------+---------+--------+-----------------------------------------------+------------------+----------+
| Id | User        | Host      | db   | Command | Time   | State                                         | Info             | Progress |
+----+-------------+-----------+------+---------+--------+-----------------------------------------------+------------------+----------+
|  3 | system user |           | NULL | Connect |    139 | closing tables                                | NULL             |    0.000 |
|  4 | system user |           | NULL | Connect |    139 | Waiting for work from SQL thread              | NULL             |    0.000 |
|  6 | system user |           | NULL | Connect | 264274 | Waiting for master to send event              | NULL             |    0.000 |
| 10 | root        | localhost | NULL | Sleep   |     43 |                                               | NULL             |    0.000 |
| 21 | system user |           | NULL | Connect |     45 | Waiting for room in worker thread event queue | NULL             |    0.000 |
| 54 | root        | localhost | NULL | Query   |      0 | init                                          | SHOW PROCESSLIST |    0.000 |
+----+-------------+-----------+------+---------+--------+-----------------------------------------------+------------------+----------+
```

The <a undefined>slave_parallel_max_queued</a> system variable does not define a hard limit, since the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) events that are currently executing always need to be held in-memory. This means that at least two events per [worker thread](/kb/en/replication-threads/#worker-threads) can always be queued in-memory, regardless of the value of <a undefined>slave_parallel_threads</a>.

Usually, the <a undefined>slave_parallel_threads</a> system variable should be set large enough that the [SQL thread](/kb/en/replication-threads/#slave-sql-thread) is able to read far enough ahead in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) to exploit all possible parallelism. In normal operation, the slave will hopefully not be too far
behind, so there will not be a need to queue much data in-memory. The <a undefined>slave_parallel_max_queued</a> system variable could be set fairly high (eg. a few hundred kilobytes) to not limit throughtput. It should just be set low enough that total allocation of the parallel slave queue will not cause the server to run out of memory.

## Configuration Variable slave_domain_parallel_threads

The pool of replication worker threads is shared among all multi-source master
connections, and among all replication domains that can replicate in parallel
using out-of-order.

If one master connection or replication domain is currently processing a
long-running query, it is possible that it will allocate all the worker
threads in the pool, only to have them wait for the long-running query to
complete, stalling any other master connection or replication domain, which
will have to wait for a worker thread to become free.

This can be avoided by setting
[slave_domain_parallel_threads](/kb/en/replication-and-binary-log-server-system-variables/#slave_domain_parallel_threads) to a number that is lower
than <code class="highlight fixed" style="white-space:pre-wrap">slave_parallel_threads</code>. When set different from zero,
each replication domain in one master connection can reserve at most that many
worker threads at any one time, leaving the rest (up to the value of
[slave_parallel_threads](/kb/en/replication-and-binary-log-server-system-variables/#slave_parallel_threads)) free for other master connections or replication domains to use in parallel.

The <code class="highlight fixed" style="white-space:pre-wrap">slave_domain_parallel_threads</code> variable is dynamic and
can be changed without restarting the server; all slaves must be stopped while
changing it, though.

## Implementation Details

The implementation is described in [MDEV-4506](https://jira.mariadb.org/browse/MDEV-4506).

## See Also

- [Better Parallel Replication for MariaDB and MySQL](https://mariadb.com/blog/better-parallel-replication-mariadb-and-mysql) (MariaDB.com blog)
- [Evaluating MariaDB &amp; MySQL Parallel Replication Part 2: Slave Group Commit](https://mariadb.com/blog/evaluating-mariadb-mysql-parallel-replication-part-2-slave-group-commit) (MariaDB.com blog)