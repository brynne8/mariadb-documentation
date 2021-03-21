# Thread Groups in the Unix Implementation of the Thread Pool

This article does not apply to the thread pool implementation on Windows. On Windows, MariaDB uses a native thread pool created with the <a undefined>CreateThreadpool</a> APl, which has its own methods to distribute threads between CPUs.

On Unix, the thread pool implementation uses objects called thread groups to divide up client connections into many independent sets of threads. The <a undefined>thread_pool_size</a> system variable defines the number of thread groups on a system. Generally speaking, the goal of the thread group implementation is to have one running thread on each CPU on the system at a time. Therefore, the default value of the <a undefined>thread_pool_size</a> system variable is auto-sized to the number of CPUs on the system.

When setting the <a undefined>thread_pool_size</a> system variable's value at system startup, the max value is `100000`. However, it is not a good idea to set it that high. When setting its value dynamically, the max value is either `128` or the value that was set at system startup--whichever value is higher. It can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL thread_pool_size=32;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
..
thread_handling=pool-of-threads
thread_pool_size=32
```

If you do not want MariaDB to use all CPUs on the system for some reason, then you can set it to a lower value than the number of CPUs. For example, this would make sense if the MariaDB Server process is limited to certain CPUs with the <a undefined>taskset</a> utility on Linux.

If you set the value to the number of CPUs and if you find that the CPUs are still underutilized, then try increasing the value.

The <a undefined>thread_pool_size</a> system variable tends to have the most visible performance effect. It is roughly equivalent to the number of threads that can run at the same time. In this case, run means use CPU, rather than sleep or wait. If a client connection needs to sleep or wait for some reason, then it wakes up another client connection in the thread group before it does so.

One reason that CPU underutilization may occur in rare cases is that the thread pool is not always informed when a thread is going to wait. For example, some waits, such as a page fault or a miss in the OS buffer cache, cannot be detected by MariaDB. Prior to [MariaDB 10.0](/kb/en/what-is-mariadb-100/), network I/O related waits could also be missed.

## Distributing Client Connections Between Thread Groups

When a new client connection is created, its thread group is determined using the following calculation:

```sql
thread_group_id = connection_id %  thread_pool_size
```

The `connection_id` value in the above calculation is the same monotonically increasing number that you can use to identify connections in [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) output or the <a undefined>information_schema.PROCESSLIST</a> table.

This calculation should assign client connections to each thread group in a round-robin manner. In general, this should result in an even distribution of client connections among thread groups.

## Types of Threads

### Thread Group Threads

Thread groups have two different kinds of threads: a <strong>listener thread</strong> and <strong>worker threads</strong>.

- A thread group's <strong>worker threads</strong> actually perform work on behalf of client connections. A thread group can have many <strong>worker threads</strong>, but usually, only one will be actively running at a time. This is not always the case. For example, the thread group can become <em>oversubscribed</em> if the thread pool's <strong>timer thread</strong> detects that the thread group is <em>stalled</em>. This is explained more in the sections below.

- A thread group's <strong>listener thread</strong> listens for I/O events and distributes work to the <strong>worker threads</strong>. If it detects that there is a request that needs to be worked on, then it can wake up a sleeping <strong>worker thread</strong> in the thread group, if any exist. If the <strong>listener thread</strong> is the only thread in the thread group, then it can also create a new <strong>worker thread</strong>. If there is only one request to handle, and if the <a undefined>thread_pool_dedicated_listener</a> system variable is not enabled, then the <strong>listener thread</strong> can also become a <strong>worker thread</strong> and handle the request itself. This helps decrease the overhead that may be introduced by excessively waking up sleeping <strong>worker threads</strong> and excessively creating new <strong>worker threads</strong>.

### Global Threads

The thread pool has one global thread: a <strong>timer thread</strong>. The <strong>timer thread</strong> performs tasks, such as:

- Checks each thread group for stalls.
- Ensures that each thread group has a <strong>listener thread</strong>.

## Thread Creation

A new thread is created in a thread group in the scenarios listed below.

In all of the scenarios below, the thread pool implementation prefers to wake up a sleeping <strong>worker thread</strong> that already exists in the thread group, rather than to create a new thread.

### Worker Thread Creation by Listener Thread

A thread group's <strong>listener thread</strong> can create a new <strong>worker thread</strong> when it has more client connection requests to distribute, but no pre-existing <strong>worker threads</strong> are available to work on the requests. This can help to ensure that the thread group always has enough threads to keep one <strong>worker thread</strong> active at a time.

A thread group's <strong>listener thread</strong> creates a new <strong>worker thread</strong> if all of the following conditions are met:

- The <strong>listener thread</strong> receives a client connection request that needs to be worked on.
- There are more client connection requests in the thread group's work queue that the <strong>listener thread</strong> still needs to distribute to <strong>worker threads</strong>, so the <strong>listener thread</strong> should not become a <strong>worker thread</strong>.
- There are no active <strong>worker threads</strong> in the thread group.
- There are no sleeping <strong>worker threads</strong> in the thread group that the <strong>listener thread</strong> can wake up.
- And one of the following conditions is also met:
<ul start="1"><li>The entire thread pool has fewer than <a undefined>thread_pool_max_threads</a>.
</li><li>There are fewer than two threads in the thread group. This is to guarantee that each thread group can have at least two threads, even if <a undefined>thread_pool_max_threads</a> has already been reached or exceeded.
</li></ul>

### Thread Creation by Worker Threads during Waits

A thread group's <strong>worker thread</strong> can create a new <strong>worker thread</strong> when the thread has to wait on something, and the thread group has more client connection requests queued, but no pre-existing <strong>worker threads</strong> are available to work on them. This can help to ensure that the thread group always has enough threads to keep one <strong>worker thread</strong> active at a time. For most workloads, this tends to be the primary mechanism that creates new <strong>worker threads</strong>.

A thread group's <strong>worker thread</strong> creates a new thread if all of the following conditions are met:

- The <strong>worker thread</strong> has to wait on some request. For example, it might be waiting on disk I/O, or it might be waiting on a lock, or it might just be waiting for a query that called the [SLEEP()](/built-in-functions/secondary-functions/miscellaneous-functions/sleep/) function to finish.
- There are no active <strong>worker threads</strong> in the thread group.
- There are no sleeping <strong>worker threads</strong> in the thread group that the <strong>worker thread</strong> can wake up.
- And one of the following conditions is also met:
<ul start="1"><li>The entire thread pool has fewer than <a undefined>thread_pool_max_threads</a>.
</li><li>There are fewer than two threads in the thread group. This is to guarantee that each thread group can have at least two threads, even if <a undefined>thread_pool_max_threads</a> has already been reached or exceeded.
</li></ul>
- And one of the following conditions is also met:
<ul start="1"><li>There are more client connection requests in the thread group's work queue that the <strong>listener thread</strong> still needs to distribute to <strong>worker threads</strong>. In this case, the new thread is intended to be a <strong>worker thread</strong>.
</li><li>There is currently no <strong>listener thread</strong> in the thread group. For example, if the <a undefined>thread_pool_dedicated_listener</a> system variable is not enabled, then the thread group's <strong>listener thread</strong> can became a <strong>worker thread</strong>, so that it could handle some client connection request. In this case, the new thread can become the thread group's <strong>listener thread</strong>.
</li></ul>

### Listener Thread Creation by Timer Thread

The thread pool's <strong>timer thread</strong> can create a new <strong>listener thread</strong> for a thread group when the thread group has more client connection requests that need to be distributed, but the thread group does not currently have a <strong>listener thread</strong> to distribute them. This can help to ensure that the thread group does not miss client connection requests because it has no <strong>listener thread</strong>.

The thread pool's <strong>timer thread</strong> creates a new <strong>listener thread</strong> for a thread group if all of the following conditions are met:

- The thread group has not handled any I/O events since the last check by the timer thread.
- There is currently no <strong>listener thread</strong> in the thread group. For example, if the <a undefined>thread_pool_dedicated_listener</a> system variable is not enabled, then the thread group's <strong>listener thread</strong> can became a <strong>worker thread</strong>, so that it could handle some client connection request. In this case, the new thread can become the thread group's <strong>listener thread</strong>.
- There are no sleeping <strong>worker threads</strong> in the thread group that the <strong>timer thread</strong> can wake up.
- And one of the following conditions is also met:
<ul start="1"><li>The entire thread pool has fewer than <a undefined>thread_pool_max_threads</a>.
</li><li>There are fewer than two threads in the thread group. This is to guarantee that each thread group can have at least two threads, even if <a undefined>thread_pool_max_threads</a> has already been reached or exceeded.
</li></ul>
- If the thread group already has active <strong>worker threads</strong>, then the following condition also needs to be met:
<ul start="1"><li>A <strong>worker thread</strong> has not been created for the thread group within the <em>throttling interval</em>.
</li></ul>

### Worker Thread Creation by Timer Thread during Stalls

The thread pool's <strong>timer thread</strong> can create a new <strong>worker thread</strong> for a thread group when the thread group is stalled. This can help to ensure that a long query can't monopole its thread group.

The thread pool's <strong>timer thread</strong> creates a new <strong>worker thread</strong> for a thread group if all of the following conditions are met:

- The <strong>timer thread</strong> thinks that the thread group is stalled. This means that the following conditions have been met:
<ul start="1"><li>There are more client connection requests in the thread group's work queue that the <strong>listener thread</strong> still needs to distribute to <strong>worker threads</strong>.
</li><li>No client connection requests have been allowed to be dequeued to run since the last stall check by the <strong>timer thread</strong>.
</li></ul>
- There are no sleeping <strong>worker threads</strong> in the thread group that the <strong>timer thread</strong> can wake up.
- And one of the following conditions is also met:
<ul start="1"><li>The entire thread pool has fewer than <a undefined>thread_pool_max_threads</a>.
</li><li>There are fewer than two threads in the thread group. This is to guarantee that each thread group can have at least two threads, even if <a undefined>thread_pool_max_threads</a> has already been reached or exceeded.
</li></ul>
- A <strong>worker thread</strong> has not been created for the thread group within the <em>throttling interval</em>.

### Thread Creation Throttling

In some of the scenarios listed above, a thread is only created within a thread group if no new threads have been created for the thread group within the <em>throttling interval</em>. The throttling interval depends on the number of threads that are already in the thread group.

##### MariaDB starting with [10.5](/kb/en/what-is-mariadb-105/)

In [MariaDB 10.5](/kb/en/what-is-mariadb-105/) and later, thread creation is not throttled until a thread group has more than 1 + <a undefined>thread_pool_oversubscribe</a> threads:

<table><tbody><tr><th>Number of Threads in Thread Group</th><th>Throttling Interval (milliseconds)</th></tr>
<tr><td>0-(1 + <code><a href="/kb/en/thread-pool-system-status-variables/#thread_pool_oversubscribe">thread_pool_oversubscribe</a></code>)</td><td>0</td></tr>
<tr><td>4-7</td><td>50 * <code>THROTTLING_FACTOR</code></td></tr>
<tr><td>8-15</td><td>100 * <code>THROTTLING_FACTOR</code></td></tr>
<tr><td>16-65536</td><td>20 * <code>THROTTLING_FACTOR</code></td></tr>
</tbody></table>

THROTTLING_FACTOR = (<a undefined>thread_pool_stall_limit</a> / MAX (500,<a undefined>thread_pool_stall_limit</a>))

##### MariaDB until [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and before, thread creation is throttled when a thread group has more than 3 threads:

<table><tbody><tr><th>Number of Threads in Thread Group</th><th>Throttling Interval (milliseconds)</th></tr>
<tr><td>0-3</td><td>0</td></tr>
<tr><td>4-7</td><td>50</td></tr>
<tr><td>8-15</td><td>100</td></tr>
<tr><td>16-65536</td><td>200</td></tr>
</tbody></table>

## Thread Group Stalls

The thread pool has a feature that allows it to detect if a client connection is executing a long-running query that may be monopolizing its thread group. If a client connection were to monopolize its thread group, then that could prevent other client connections in the thread group from running their queries. In other words, the thread group would appear to be <em>stalled</em>.

This stall detection feature is implemented by creating a <strong>timer thread</strong> that periodically checks if any of the thread groups are stalled. There is only a single <strong>timer thread</strong> for the entire thread pool. The <a undefined>thread_pool_stall_limit</a> system variable defines the number of milliseconds between each stall check performed by the timer thread. The default value is `500`. It can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL thread_pool_stall_limit=300;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
..
thread_handling=pool-of-threads
thread_pool_size=32
thread_pool_stall_limit=300
```

The <strong>timer thread</strong> considers a thread group to be stalled if the following is true:

- There are more client connection requests in the thread group's work queue that the <strong>listener thread</strong> still needs to distribute to <strong>worker threads</strong>.
- No client connection requests have been allowed to be dequeued to run since the last stall check by the <strong>timer thread</strong>.

This indicates that the one or more client connections currently using the active <strong>worker threads</strong> may be monopolizing the thread group, and preventing the queued client connections from performing work. When the <strong>timer thread</strong> detects that a thread group is stalled, it wakes up a sleeping <strong>worker thread</strong> in the thread group, if one is available. If there isn't one, then it creates a new <strong>worker thread</strong> in the thread group. This temporarily allows several client connections in the thread group to run in parallel.

The <a undefined>thread_pool_stall_limit</a> system variable essentially defines the limit for what a "fast query" is. If a query takes longer than <a undefined>thread_pool_stall_limit</a>, then the thread pool is likely to think that it is too slow, and it will either wake up a sleeping worker thread or create a new worker thread to let another client connection in the thread group run a query in parallel.

In general, changing the value of the <a undefined>thread_pool_stall_limit</a> system variable has the following effect:

- Setting it to <strong>higher</strong> values can help avoid starting too many parallel threads if you expect a lot of client connections to execute long-running queries.
- Setting it to <strong>lower</strong> values can help prevent deadlocks.

### Thread Group Oversubscription

If the <strong>timer thread</strong> were to detect a stall in a thread group, then it would either wake up a sleeping <strong>worker thread</strong> or create a new <strong>worker thread</strong> in that thread group. At that point, the thread group would have multiple active <strong>worker threads</strong>. In other words, the thread group would be <em>oversubscribed</em>.

You might expect that the thread pool would shutdown one of the <strong>worker threads</strong> when the stalled client connection finished what it was doing, so that the thread group would only have one active <strong>worker thread</strong> again. However, this does not always happen. Once a thread group is oversubscribed, the <a undefined>thread_pool_oversubscribe</a> system variable defines the upper limit for when <strong>worker threads</strong> start shutting down after they finish work for client connections. The default value is `3`. It can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL thread_pool_oversubscribe=10;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
..
thread_handling=pool-of-threads
thread_pool_size=32
thread_pool_stall_limit=300
thread_pool_oversubscribe=10
```

To clarify, the <a undefined>thread_pool_oversubscribe</a> system variable does not play any part in the creation of new <strong>worker threads</strong>. The <a undefined>thread_pool_oversubscribe</a> system variable is only used to determine how many <strong>worker threads</strong> should remain active in a thread group, once a thread group is already oversubscribed due to stalls.

In general, the default value of `3` should be adequate for most users. Most users should not need to change the value of the <a undefined>thread_pool_oversubscribe</a> system variable.