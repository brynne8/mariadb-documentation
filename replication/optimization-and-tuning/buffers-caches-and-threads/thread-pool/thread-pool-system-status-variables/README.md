# Thread Pool System and Status Variables

This article describes the system and status variables used by the MariaDB thread pool. For a full description, see [Thread Pool in MariaDB](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/).

## System variables

#### `extra_max_connections`

- <strong>Description:</strong> The number of connections on the <a undefined>extra_port</a>.
<ul start="1"><li>See [Thread Pool in MariaDB: Configuring the Extra Port](/kb/en/thread-pool-in-mariadb/#configuring-the-extra-port) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--extra-max-connections=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` to `100000`

---

#### `extra_port`

- <strong>Description:</strong> Extra port number to use for TCP connections in a `one-thread-per-connection` manner. If set to `0`, then no extra port is used.
<ul start="1"><li>See [Thread Pool in MariaDB: Configuring the Extra Port](/kb/en/thread-pool-in-mariadb/#configuring-the-extra-port) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--extra-port=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`

---

#### `thread_handling`

- <strong>Description:</strong> Determines how the server handles threads for client connections. In addition to threads for client connections, this also applies to certain internal server threads, such as [Galera slave threads](/kb/en/about-galera-replication/#galera-slave-threads).
<ul start="1"><li>When the default `one-thread-per-connection` mode is enabled, the server uses one thread to handle each client connection.
</li><li>When the `pool-of-threads` mode is enabled, the server uses the [thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/) for client connections.
</li><li>When the `no-threads` mode is enabled, the server uses a single thread for all client connections, which is really only usable for debugging.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--thread-handling=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `one-thread-per-connection`
- <strong>Valid Values:</strong> `no-threads`, `one-thread-per-connection`, `pool-of-threads`.
- <strong>Documentation:</strong> [Using the thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/).
- <strong>Notes:</strong> In MySQL the thread pool is only available in MySQL enterprise 5.5.16 and above. In MariaDB it's available in all versions.

---

#### `thread_pool_dedicated_listener`

- <strong>Description:</strong> If set to 1, then each group will have its own dedicated listener, and the listener thread will not pick up work items. As a result, the queueing time in the [Information Schema Threadpool_Queues](/kb/en/information-schema-threadpool_queues-table/) and the actual queue size in the [Information Schema Threadpool_Groups](/kb/en/information-schema-threadpool_groups-table/) table will be more exact, since
IO requests are immediately dequeued from poll, without delay.
<ul start="1"><li>This system variable is only meaningful on <strong>Unix</strong>.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">thread-pool-dedicated-listener={0|1}</code>
- <strong>Scope:</strong>
- <strong>Dynamic:</strong>
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`
- <strong>Introduced:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `thread_pool_exact_stats`

- <strong>Description:</strong> If set to 1, provides better queueing time statistics by using a high precision timestamp, at a small performance cost, for the time when the connection was added to the queue. This timestamp helps
calculate the queuing time shown in the [Information Schema Threadpool_Queues](/kb/en/information-schema-threadpool_queues-table/) table.
<ul start="1"><li>This system variable is only meaningful on <strong>Unix</strong>.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">thread-pool-exact-stats={0|1}</code>
- <strong>Scope:</strong>
- <strong>Dynamic:</strong>
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`
- <strong>Introduced:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `thread_pool_idle_timeout`

- <strong>Description:</strong> The number of seconds before an idle worker thread exits. The default value is `60`. If there is currently no work to do, how long should an idle thread wait before exiting?
<ul start="1"><li>This system variable is only meaningful on <strong>Unix</strong>.
</li><li>The <a undefined>thread_pool_min_threads</a> system variable is comparable for Windows.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">thread-pool-idle-timeout=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `60`
- <strong>Documentation:</strong> [Using the thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/).

---

#### `thread_pool_max_threads`

- <strong>Description:</strong> The maximum number of threads in the [thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/). Once this limit is reached, no new threads will be created in most cases.
<ul start="1"><li>On Unix, in rare cases, the actual number of threads can slightly exceed this, because each [thread group](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-groups-in-the-unix-implementation-of-the-thread-pool/) needs at least two threads (i.e. at least one worker thread and at least one listener thread) to prevent deadlocks.
</li></ul>
- <strong>Scope:</strong>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">thread-pool-max-threads=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> 
<ul start="1"><li>`65536` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/))
</li><li>`1000` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/), &gt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/))
</li><li>`500` (&lt;= [MariaDB 10.0](/kb/en/what-is-mariadb-100/))
</li></ul>
- <strong>Range:</strong> `1` to `65536`
- <strong>Documentation:</strong> [Using the thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/).

---

#### `thread_pool_min_threads`

- <strong>Description:</strong> Minimum number of threads in the [thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/). In bursty environments, after a period of inactivity, threads would normally be retired. When the next burst arrives, it would take time to reach the optimal level. Setting this value higher than the default would prevent thread retirement even if inactive.
<ul start="1"><li>This system variable is only meaningful on <strong>Windows</strong>.
</li><li>The <a undefined>thread_pool_idle_timeout</a> system variable is comparable for Unix.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">thread-pool-min-threads=#</code>
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Documentation:</strong> [Using the thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/).

---

#### `thread_pool_oversubscribe`

- <strong>Description:</strong> Determines how many worker threads in a thread group can remain active at the same time once a thread group is oversubscribed due to stalls. The default value is `3`. Usually, a thread group only has one active worker thread at a time. However, the timer thread can add more active worker threads to a thread group if it detects a stall. There are trade-offs to consider when deciding whether to allow <strong>only one</strong> thread per CPU to run at a time, or whether to allow <strong>more than one</strong> thread per CPU to run at a time. Allowing only one thread per CPU means that the thread can have unrestricted access to the CPU while its running, but it also means that there is additional overhead from putting threads to sleep or waking them up more frequently. Allowing more than one thread per CPU means that the threads have to share the CPU, but it also means that there is less overhead from putting threads to sleep or waking them up.
<ul start="1"><li>See [Thread Groups in the Unix Implementation of the Thread Pool: Thread Group Oversubscription](/kb/en/thread-groups-in-the-unix-implementation-of-the-thread-pool/#thread-group-oversubscription) for more information.
</li><li>This is primarily for <strong>internal</strong> use, and it is <strong>not</strong> meant to be changed for most users.
</li><li>This system variable is only meaningful on <strong>Unix</strong>.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `3`
- <strong>Range:</strong> `1` to `65536`
- <strong>Documentation:</strong> [Using the thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/).

---

#### `thread_pool_prio_kickup_timer`

- <strong>Description:</strong> Time in milliseconds before a dequeued low-priority statement is moved to the high-priority queue.
<ul start="1"><li>This system variable is only meaningful on <strong>Unix</strong>.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">thread-pool-kickup-timer=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1000`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)
- <strong>Documentation:</strong> [Using the thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/).

---

#### `thread_pool_priority`

- <strong>Description:</strong> [Thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/) priority. High-priority connections usually start executing earlier than low-priority.
If set to 'auto' (the default), the actual priority (low or high) is determined by whether or not the connection is inside a transaction.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--thread-pool-priority=#</code>
- <strong>Scope:</strong> Global,Connection
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `auto`
- <strong>Valid Values:</strong> `high`, `low`, `auto`.
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)
- <strong>Documentation:</strong> [Using the thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/).

---

#### `thread_pool_size`

- <strong>Description:</strong> The number of [thread groups](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-groups-in-the-unix-implementation-of-the-thread-pool/) in the [thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/), which determines how many statements can execute simultaneously. The default value is the number of CPUs on the system. When setting this system variable's value at system startup, the max value is 100000. However, it is not a good idea to set it that high. When setting this system variable's value dynamically, the max value is either 128 or the value that was set at system startup--whichever value is higher.
<ul start="1"><li>See [Thread Groups in the Unix Implementation of the Thread Pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-groups-in-the-unix-implementation-of-the-thread-pool/) for more information.
</li><li>This system variable is only meaningful on <strong>Unix</strong>.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--thread-pool-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> Based on the number of processors (but see [MDEV-7806](https://jira.mariadb.org/browse/MDEV-7806)).
- <strong>Range:</strong> `1` to `128` (&lt; [MariaDB 5.5.37](/kb/en/mariadb-5537-release-notes/), [MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/)), `1` to `100000` (&gt;= [MariaDB 5.5.37](/kb/en/mariadb-5537-release-notes/), [MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/))
- <strong>Documentation:</strong> [Using the thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/).

---

#### `thread_pool_stall_limit`

- <strong>Description:</strong> The number of milliseconds between each stall check performed by the timer thread. The default value is `500`. Stall detection is used to prevent a single client connection from monopolizing a thread group. When the timer thread detects that a thread group is stalled, it wakes up a sleeping worker thread in the thread group, if one is available. If there isn't one, then it creates a new worker thread in the thread group. This temporarily allows several client connections in the thread group to run in parallel. However, note that the timer thread will not create a new worker thread if the number of threads in the thread pool is already greater than or equal to the maximum defined by the <a undefined>thread_pool_max_threads</a> variable, unless the thread group does not already have a listener thread.
<ul start="1"><li>See [Thread Groups in the Unix Implementation of the Thread Pool: Thread Group Stalls](/kb/en/thread-groups-in-the-unix-implementation-of-the-thread-pool/#thread-group-stalls) for more information.
</li><li>This system variable is only meaningful on <strong>Unix</strong>.
</li><li>Note that if you are migrating from the MySQL Enterprise thread pool plugin, then the unit used in their implementation is 10ms, not 1ms.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--thread-pool-stall-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `500`
- <strong>Range:</strong> `10` to `4294967295` (&lt; [MariaDB 10.5](/kb/en/what-is-mariadb-105/)),  `1` to `4294967295` (&gt;= [MariaDB 10.5](/kb/en/what-is-mariadb-105/))
- <strong>Documentation:</strong> [Using the thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/).

---

## Status variables

#### `Threadpool_idle_threads`

- <strong>Description:</strong> Number of inactive threads in the [thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/). Threads become inactive for various reasons, such as by waiting for new work. However, an inactive thread is not necessarily one that has not been assigned work. Threads are also considered inactive if they are being blocked while waiting on disk I/O, or while waiting on a lock, etc.
<ul start="1"><li>This status variable is only meaningful on <strong>Unix</strong>.
</li></ul>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Threadpool_threads`

- <strong>Description:</strong> Number of threads in the [thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/). In rare cases, this can be slightly higher than <a undefined>thread_pool_max_threads</a>, because each thread group needs at least two threads (i.e. at least one worker thread and at least one listener thread) to prevent deadlocks.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

## See Also

- [Thread Pool in MariaDB](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb/)