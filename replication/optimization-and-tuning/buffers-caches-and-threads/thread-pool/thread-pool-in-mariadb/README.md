# Thread Pool in MariaDB

## Problem That Thread Pools Solve

The task of scalable server software (and a DBMS like MariaDB is an example of
such software) is to maintain top performance with an increasing number of
clients. MySQL traditionally assigned a thread for every client connection,
and as the number of concurrent users grows this model shows performance drops.
Many active threads are a performance killer, because increasing the number of
threads leads to extensive context switching, bad locality for CPU caches, and
increased contention for hot locks. An ideal solution that would help to reduce
context switching is to maintain a lower number of threads than the number of
clients. But this number should not be too low either, since we also want to
utilize CPUs to their fullest, so ideally, there should be a single active
thread for each CPU on the machine.

## MariaDB Thread Pool Features

The current MariaDB thread pool was implemented in [MariaDB 5.5](/kb/en/what-is-mariadb-55/). It replaced the legacy thread pool that was introduced in [MariaDB 5.1](/kb/en/what-is-mariadb-51/). The main drawback of the previous solution was that this pool was static–it had a fixed number of threads. Static thread pools can have their merits, for some limited use cases, such as cases where callbacks executed by the threads never block and do not depend on each other. For example, iimagine something like an echo server.

However, DBMS clients are more complicated. For example, a thread may depend on another thread's completion, and they may block each other via locks and/or I/O. Thus it is very hard, and sometimes impossible, to predict how many threads would be ideal or even sufficient to prevent deadlocks in every situation. [MariaDB 5.5](/kb/en/what-is-mariadb-55/) implements a dynamic/adaptive pool that itself takes care of creating new threads in times of high demand and retiring threads if they have nothing to do. This is a complete reimplementation of the legacy `pool-of-threads` scheduler, with the following goals:

- Make the pool dynamic, so that it will grow and shrink whenever required.
- Minimize the amount of overhead that is  required to maintain the thread pool itself.
- Make the best use of underlying OS capabilities. For example, if a native thread pool implementation is available, then it should be used, and if not, then the best I/O multiplexing method should be used.
- Limit the resources used by threads.

There are currently two different low-level implementations – depending on OS. One implementation is designed specifically for Windows which utilizes a native <a undefined>CreateThreadpool</a> API. The second implementation is primarily intended to be used in Unix-like systems. Because the implementations are
different, some system variables differ between Windows and Unix.

## When to Use the Thread Pool

Thread pools are most efficient in situations where queries are relatively short and the load is CPU-bound, such as in OLTP workloads. If the workload is not CPU-bound, then you might still benefit from limiting the number of threads to save memory for the database memory buffers.

## When the Thread Pool is Less Efficient

There are special, rare cases where the thread pool is likely to be less efficient.

- If you have a <strong>very bursty workload</strong>, then the thread pool may not work well for you. These tend to be workloads in which there are long periods of inactivity followed by short periods of very high activity by many users. These also tend to be workloads in which delays cannot be tolerated, so the throttling of thread creation that the thread pool uses is not ideal. Even in this situation, performance can be improved by tweaking how often threads are retired. For example, with <a undefined>thread_pool_idle_timeout</a> on Unix, or with <a undefined>thread_pool_min_threads</a> on Windows.

- If you have <strong>many concurrent, long, non-yielding</strong> queries, then the thread pool may not work well for you. In this context, a "non-yielding" query is one that never waits or which does not indicate waits to the thread pool. These kinds of workloads are mostly used in data warehouse scenarios. Long-running, non-yielding queries will delay execution of other queries. However, the thread pool has stall detection to prevent them from totally monopolizing the thread pool. See [Thread Groups in the Unix Implementation of the Thread Pool: Thread Group Stalls](/kb/en/thread-groups-in-the-unix-implementation-of-the-thread-pool/#thread-group-stalls) for more information. Even when the whole thread pool is blocked by non-yielding queries, you can still connect to the server through the <a undefined>extra-port</a> TCP/IP port.

- If you rely on the fact that <strong>simple queries always finish quickly</strong>, no matter how loaded you database server is, then the thread pool may not work well for you. When the thread pool is enabled on a busy server, even simple queries might be queued to be executed later. This means that even if the statement itself doesn't take much time to execute, even a simple `SELECT 1`, might take a bit longer when the thread pool is enabled than with `one-thread-per-connection` if it gets queued.

## Configuring the Thread Pool

The <a undefined>thread_handling</a> system variable is the primary system variable that is used to configure the thread pool.

There are several other system variables as well, which are described in the sections below. Many of the system variables documented below are dynamic, meaning that they can be changed with <a undefined>SET GLOBAL</a> on a running server.

Generally, there is no need to tweak many of these system variables. The goal of the thread pool was to
provide good performance out-of-the box. However, the system variable values can be changed, and we intended to expose as many knobs from the underlying implementation as we could. Feel free to tweak them
as you see fit.

If you find any issues with any of the default behavior, then we encourage you to [submit a bug report](/kb/en/reporting-bugs/).

See [Thread Pool System and Status Variables](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-system-status-variables/) for the full list of the thread pool's system variables.

### Configuring the Thread Pool on Unix

On Unix, if you would like to use the thread pool, then you can use the thread pool by setting the <a undefined>thread_handling</a> system variable to `pool-of-threads` in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
thread_handling=pool-of-threads
```

The following system variables can also be configured on Unix:

- <a undefined>thread_pool_size</a> – The number of [thread groups](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-groups-in-the-unix-implementation-of-the-thread-pool/) in the thread pool, which determines how many statements can execute simultaneously. The default value is the number of CPUs on the system. When setting this system variable's value at system startup, the max value is 100000. However, it is not a good idea to set it that high. When setting this system variable's value dynamically, the max value is either 128 or the value that was set at system startup--whichever value is higher. See [Thread Groups in the Unix Implementation of the Thread Pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-groups-in-the-unix-implementation-of-the-thread-pool/) for more information.

- <a undefined>thread_pool_max_threads</a> – The maximum number of threads in the thread pool. Once this limit is reached, no new threads will be created in most cases. In rare cases, the actual number of threads can slightly exceed this, because each [thread group](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-groups-in-the-unix-implementation-of-the-thread-pool/) needs at least two threads (i.e. at least one worker thread and at least one listener thread) to prevent deadlocks. The default value in [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.0](/kb/en/what-is-mariadb-100/) is `500`. The default value in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) is `1000` in [MariaDB 10.1](/kb/en/what-is-mariadb-101/). The default value in [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later is `65536`.

- <a undefined>thread_pool_stall_limit</a> – The number of milliseconds between each stall check performed by the timer thread. The default value is `500`. Stall detection is used to prevent a single client connection from monopolizing a thread group. When the timer thread detects that a thread group is stalled, it wakes up a sleeping worker thread in the thread group, if one is available. If there isn't one, then it creates a new worker thread in the thread group. This temporarily allows several client connections in the thread group to run in parallel. However, note that the timer thread will not create a new worker thread if the number of threads in the thread pool is already greater than or equal to the maximum defined by the <a undefined>thread_pool_max_threads</a> variable, unless the thread group does not already have a listener thread. See [Thread Groups in the Unix Implementation of the Thread Pool: Thread Group Stalls](/kb/en/thread-groups-in-the-unix-implementation-of-the-thread-pool/#thread-group-stalls) for more information.

- <a undefined>thread_pool_oversubscribe</a> – Determines how many worker threads in a thread group can remain active at the same time once a thread group is oversubscribed due to stalls. The default value is `3`. Usually, a thread group only has one active worker thread at a time. However, the timer thread can add more active worker threads to a thread group if it detects a stall. There are trade-offs to consider when deciding whether to allow <strong>only one</strong> thread per CPU to run at a time, or whether to allow <strong>more than one</strong> thread per CPU to run at a time. Allowing only one thread per CPU means that the thread can have unrestricted access to the CPU while its running, but it also means that there is additional overhead from putting threads to sleep or waking them up more frequently. Allowing more than one thread per CPU means that the threads have to share the CPU, but it also means that there is less overhead from putting threads to sleep or waking them up. This is primarily for <strong>internal</strong> use, and it is <strong>not</strong> meant to be changed for most users. See [Thread Groups in the Unix Implementation of the Thread Pool: Thread Group Oversubscription](/kb/en/thread-groups-in-the-unix-implementation-of-the-thread-pool/#thread-group-oversubscription) for more information.

- <a undefined>thread_pool_idle_timeout</a> – The number of seconds before an idle worker thread exits. The default value is `60`. If there is currently no work to do, how long should an idle thread wait before exiting?

### Configuring the Thread Pool on Windows

The Windows implementation of the thread pool uses a native thread pool created with the <a undefined>CreateThreadpool</a> API.

On Windows, if you would like to use the thread pool, then you do not need to do anything, because the default for the <a undefined>thread_handling</a> system variable is already preset to `pool-of-threads`.

However, if you would like to use the old one thread per-connection behavior on Windows, then you can use use that by setting the <a undefined>thread_handling</a> system variable to `one-thread-per-connection` in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
thread_handling=one-thread-per-connection
```

On older versions of Windows, such as XP and 2003, `pool-of-threads` is not
implemented, and the server will silently switch to using the legacy
`one-thread-per-connection` method.

The native <a undefined>CreateThreadpool</a> API allows applications to set the minimum and maximum number of threads in the pool. The following system variables can be used to configure those values on Windows:

- <a undefined>thread_pool_min_threads</a> – The minimum number of threads in the pool. Default is 1.
  This applicable in a special case of very “bursty” workloads. Imagine having
  longer periods of inactivity after periods of high activity. While the thread pool
  is idle, Windows would decide to retire pool threads (based on
  experimentation, this seems to happen after thread had been idle for 1
  minute). Next time high load comes, it could take some milliseconds or
  seconds until the thread pool size stabilizes again to optimal value. To avoid
  thread retirement, one could set the parameter to a higher value.

- <a undefined>thread_pool_max_threads</a> – The maximum number of threads in the pool.
  Threads are not created when this value is reached. The default from [MariaDB 5.5](/kb/en/what-is-mariadb-55/) to [MariaDB 10.0](/kb/en/what-is-mariadb-100/) is 500 (this has been increased to 1000 in [MariaDB 10.1](/kb/en/what-is-mariadb-101/)).  This
  parameter can be used to prevent the creation of new threads if the pool can
  have short periods where many or all clients are blocked (for example, with
  “FLUSH TABLES WITH READ LOCK”, high contention on row locks, or similar). New
  threads are created if a blocking situation occurs (such as after a
  throttling interval), but sometimes you want to cap the number of threads,
  if you’re familiar with the application and need to, for example, save
  memory. If your application constantly pegs at 500 threads, it might be a
  strong indicator for high contention in the application, and the thread pool does
  not help much.

### Configuring Priority Scheduling

Starting with [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), it is possible to configure connection prioritization. The priority behavior is configured by the <a undefined>thread_pool_priority</a> system variable.

By default, if <a undefined>thread_pool_priority</a> is set to `auto`, then queries would be given a higher priority, in case the current connection is inside a transaction. This allows the running transaction to finish faster, and has the effect of lowering the number of transactions running in parallel. The default setting will generally improve throughput for transactional workloads. But it is also possible to explicitly set the priority for the current connection to either 'high' or 'low'.

There is also a mechanism in place to ensure that higher priority connections are not monopolizing the worker threads in the pool (which would cause indefinite delays for low priority connections). On Unix, low priority connections are put into the high priority queue after the timeout specified by the <a undefined>thread_pool_prio_kickup_timer</a> system variable.

### Configuring the Extra Port

MariaDB allows you to configure an extra port for administrative connections. This is primarily intended to be used in situations where all threads in the thread pool are blocked, and you still need a way to access the server. However, it can also be used to ensure that monitoring systems (including MaxScale's monitors) always have access to the system, even when all connections on the main port are used. This extra port uses the old `one-thread-per-connection` thread handling.

You can enable this and configure a specific port by setting the <a undefined>extra_port</a> system variable.

You can configure a specific number of connections for this port by setting the <a undefined>extra_max_connections</a> system variable.

These system variables can be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
extra_port = 8385
extra_max_connections = 10
```

Once you have the extra port configured, you can use the [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) client with the `-P` option to connect to the port.

```sql
$ mysql -u root -P 8385 -p
```

## Monitoring Thread Pool Activity

Currently there are two status variables exposed to monitor pool activity.

<table><tbody><tr><th>Variable</th><th>Description</th></tr>
<tr><td><code><a href="/kb/en/thread-pool-system-and-status-variables/#threadpool_threads">Threadpool_threads</a></code></td><td>Number of threads in the thread pool. In rare cases, this can be slightly higher than <code><a href="/kb/en/thread-pool-system-status-variables/#thread_pool_max_threads">thread_pool_max_threads</a></code>, because each thread group needs at least two threads (i.e. at least one worker thread and at least one listener thread) to prevent deadlocks.</td></tr>
<tr><td><code><a href="/kb/en/thread-pool-system-and-status-variables/#threadpool_idle_threads">Threadpool_idle_threads</a></code></td><td>Number of inactive threads in the thread pool. Threads become inactive for various reasons, such as by waiting for new work. However, an inactive thread is not necessarily one that has not been assigned work. Threads are also considered inactive if they are being blocked while waiting on disk I/O, or while waiting on a lock, etc. This status variable is only meaningful on <strong>Unix</strong>.</td></tr>
</tbody></table>

## Thread Groups in the Unix Implementation of the Thread Pool

On Unix, the thread pool implementation uses objects called thread groups to divide up client connections into many independent sets of threads. See [Thread Groups in the Unix Implementation of the Thread Pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-groups-in-the-unix-implementation-of-the-thread-pool/) for more information.

## Fixing a Blocked Thread Pool

When using global locks, even with a high value on the <a undefined>thread_pool_max_threads</a> system variable, it is still possible to block the entire pool.

Imagine the case where a client performs [FLUSH TABLES WITH READ LOCK](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) then pauses.  If then the number of other clients connecting to the server to start write operations exceeds the maximum number of threads allowed in the pool, it can block the Server.  This makes it impossible to issue the <a undefined>UNLOCK TABLES</a> statement.  It can also block MaxScale from monitoring the Server.

To mitigate the issue, MariaDB allows you to configure an extra port for administrative connections.  See [Configuring the Extra Port](#configuring-the-extra-port) for information on how to configure this.

Once you have the extra port configured, you can use the [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) client with the `-P` option to connect to the port.

```sql
$ mysql -u root -P 8385 -p
```

This ensures that your administrators can access the server in cases where the number of threads is already equal to the configured value of the <a undefined>thread_pool_max_threads</a> system variable, and all threads are blocked.  It also ensures that MaxScale can still access the server in such situations for monitoring information.

Once you are connected to the extra port, you can solve the issue by increasing the value on the <a undefined>thread_pool_max_threads</a> system variable, or by killing the offending connection, (that is, the connection that holds the global lock, which would be in the `sleep` state).

## MariaDB Thread Pool vs Oracle MySQL Enterprise Thread Pool

Commercial editions of MySQL since 5.5 include an Oracle MySQL Enterprise
thread pool implemented as a plugin, which delivers similar functionality.
Official documentation of the feature can be found in the
[MySQL Reference Manual](http://dev.mysql.com/doc/refman/5.5/en/thread-pool-plugin.html)
and a detailed discussion about the design of the feature is at
[Mikael Ronstrom's blog](http://mikaelronstrom.blogspot.com/2011/10/mysql-thread-pool-summary.html).
Here is the summary of similarities and differences, based on the above
materials.

### Similarities

- On Unix, both MariaDB and Oracle MySQL Enterprise Threadpool will partition
  client connections into groups. The [thread_pool_size](/kb/en/thread-pool-system-and-status-variables/#thread_pool_size) parameter thus has
  the same meaning for both MySQL and MariaDB.

- Both implementations use similar schema checking for thread stalls, and both
  have the same parameter name for [thread_pool_stall_limit](/kb/en/thread-pool-system-and-status-variables/#thread_pool_stall_limit) (though in
  MariaDB it is measured using millisecond units, not 10ms units like in Oracle
  MySQL).

### Differences

- The Windows implementation is completely different – MariaDB's uses native
  Windows threadpooling, while Oracle's implementation includes a convenience
  function <em>WSAPoll()</em> (provided for convenience to port Unix applications).
  As a consequence of relying on <em>WSAPoll()</em>, Oracle's implementation will
  not work with named pipes and shared memory connections.

- MariaDB uses the most efficient I/O multiplexing facilities for each
  operating system: Windows (the I/O completion port is used internally by the
  native threadpool), Linux (epoll), Solaris (event ports), FreeBSD and OSX
  (kevent). Oracle uses optimized I/O multiplexing only on Linux, with epoll,
  and uses poll() otherwise.

- Unlike Oracle MySQL Enterprise Threadpool, MariaDB's one is builtin, not a
  plugin.

## MariaDB Thread Pool vs  Percona Thread Pool

[Percona's implementation](https://www.percona.com/doc/percona-server/5.7/performance/threadpool.html) is a port of the MariaDB's threadpool with some added features. In particular, Percona added priority scheduling to its 5.5-5.7 releases. [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and Percona priority scheduling work in a similar fashion, but there are some differences in details.

- MariaDB's 10.2 thread_pool_priority=auto,high, low correspond to Percona's thread_pool_high_prio_mode=transactions,statements,none
- Percona has a thread_pool_high_prio_tickets connection variable to allow every nth low priority query to be put into the high priority queue. MariaDB does not have corresponding settings.
- MariaDB has a thread_pool_prio_kickup_timer setting, which Percona does not have.

## Thread Pool Internals

Low-level implementation details are documented in the
[WL#246](http://askmonty.org/worklog/Server-BackLog/?tid=246)

## Running Benchmarks

When running sysbench and maybe other benchmarks, that create many threads on 
the same machine as the server, it is advisable to run benchmark driver and 
server on different CPUs to get the realistic results.
Running lots of driver threads and only several server threads on the same CPUs 
will have the effect that OS scheduler will schedule benchmark driver threads 
to run with much higher probability than the server threads, that is driver 
will pre-empt the server.
Use "taskset –c" on Linuxes, and "set /affinity" on Windows to separate 
benchmark driver and server CPUs, as the preferred method to fix this situation.

A possible alternative on Unix (if taskset or a separate machine running the benchmark is not desired for some reason) would be to increase thread_pool_size to make  the server threads more "competitive" against the client threads.

When running sysbench, a good rule of thumb could be to give 1/4 of all CPUs to the sysbench, and 3/4 of CPUs to mysqld. It is also good idea to run sysbench and mysqld on different NUMA nodes, if possible.

## Notes

The [thread_cache_size](/kb/en/server-system-variables/#thread_cache_size) system variable is not used when the thread pool is used and the [Threads_cached](/kb/en/server-status-variables/#threads_cached) status variable will have a value of 0.

## See Also

- [Thread Pool System and Status Variables](/kb/en/thread-pool-system-and-status-variables/)
- [Threadpool Benchmarks](/kb/en/threadpool-benchmarks/)
- [Thread Pool in MariaDB 5.1 - 5.3](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb-51-53/)