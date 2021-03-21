# MariaDB Memory Allocation

## Allocating RAM for MariaDB - The Short Answer

If only using [MyISAM](/kb/en/myisam/), set [key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size) to 20% of <strong>available</strong> RAM. (Plus [innodb_buffer_pool_size=0](/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_size))

If only using InnoDB, set [innodb_buffer_pool_size](/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_size) to 70% of <strong>available</strong> RAM. (Plus [key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size) = 10M, small, but not zero.)

Rule of thumb for tuning:

- Start with released copy of my.cnf / my.ini.
- Change [key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size) and [innodb_buffer_pool_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_size) according to engine usage and RAM.
- Slow queries can usually be 'fixed' via indexes, schema changes, or SELECT changes, not by tuning.
- Don't get carried away with the [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache) until you understand what it can and cannot do.
- Don't change anything else unless you run into trouble (eg, max connections).
- Be sure the changes are under the [mysqld] section, not some other section.

The 20%/70% assumes you have at least 4GB of RAM. If you have a tiny antique, or a tiny VM, then those percentages are too high.

Now for the gory details.

## How to troubleshoot out-of-memory issues

If the MariaDB server is crashing because of 'out-of-memory' then it is probably wrongly configured.

There are two kind of buffers in MariaDB:

- Global ones that are only allocated once during the lifetime of the server:
<ul start="1"><li>Storage engine buffers ([innodb_buffer_pool_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_size), [key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size), [aria_pagecache_buffer_size](/kb/en/aria-system-variables/#aria_pagecache_buffer_size), etc)
</li><li>Query cache [query_cache_size](/kb/en/server-system-variables/#query_cache_size).
</li></ul>

- Global caches onces that grow and shrink dynamically on demand up to max limit:
<ul start="1"><li>[max_user_connections](/kb/en/server-system-variables/#max_user_connections)
</li><li>[table_open_cache](/kb/en/server-system-variables/#table_open_cache)
</li><li>[table_definition_cache](/kb/en/server-system-variables/#table_definition_cache)
</li><li>[thread_cache_size](/kb/en/server-system-variables/#thread_cache_size)
</li></ul>

- Local buffers that are allocated on demand whenever needed
<ul start="1"><li>Internal ones used during engine index creation
  ([myisam_sort_buffer_size](/kb/en/myisam-system-variables/#myisam_sort_buffer_size), [aria_sort_buffer_size).](/kb/en/aria-system-variables/#aria_sort_buffer_size)
</li><li>Internal buffers for storing blobs.
<ul start="1"><li>Some storage engine will keep a temporary cache to store the largest blob seen so far when scanning a table. This will be freed at end of query. Note that temporary blob storage is <strong>not</strong> included in the memory information in [information_schema.processlist](/kb/en/information-schema-processlist-table/) but only in the total memory used (<code class="fixed" style="white-space:pre-wrap">show global status like "memory_used"</code>).
</li></ul>
</li><li>Buffers and caches used during query execution:
</li></ul>

<table><tbody><tr><th>Variable</th><th>Description</th></tr>
<tr><td><a href="/kb/en/server-system-variables/#join_buffer_size">join_buffer_size</a></td><td>Used when no keys can be used to find a row in next table</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#mrr_buffer_size">mrr_buffer_size</a></td><td>Size of buffer to use when using multi-range read with range access</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#net_buffer_length">net_buffer_length</a></td><td>Max size of network packet</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#read_buffer_size">read_buffer_size</a></td><td>Used by some storage engines when doing bulk insert</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#sort_buffer_size">sort_buffer_size</a></td><td>When doing ORDER BY or GROUP BY</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#max_heap_table_size">max_heap_table_size</a></td><td>Used to store temporary tables in memory.  See <a href="/kb/en/optimizing-memory-tables/">Optimizing memory tables</a></td></tr>
</tbody></table>

If any variables in the last group is very large and you have a lot
of simultaneous users that are executing queries that are using these
buffers then you can run into trouble.

In a default MariaDB installation the default of most of the above
variables are quite small to ensure that one does not run out of memory.

You can check which variables that have been changed in your setup by
executing the following sql statement. If you are running into
out-of-memory issues, it is very likely that the problematic variable is
in this list!

```sql
select information_schema.system_variables.variable_name,
information_schema.system_variables.default_value,
global_variables.variable_value from
information_schema.system_variables,information_schema.global_variables where
system_variables.variable_name=global_variables.variable_name and
system_variables.default_value <> global_variables.variable_value and
system_variables.default_value <> 0
```

## What is the Key Buffer?

[MyISAM](/kb/en/myisam/) does two different things for caching.

- Index blocks (1KB each, BTree structured, from .MYI file) live in the "key buffer".
- Data block caching (from .MYD file) is left to the OS, so be sure to leave a bunch of free space for this.
Caveat: Some flavors of OS always claim to be using over 90%, even when there is really lots of free space.

```sql
SHOW GLOBAL STATUS LIKE 'Key%';
```

then calculate [Key_read_requests](/kb/en/server-status-variables/#key_read_requests) / [Key_reads](/kb/en/server-status-variables/#key_reads). If it is high (say, over 10), then the key buffer is big enough, otherwise you should adjust the [key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size) value.

## What is the Buffer Pool?

InnoDB does all its caching in a the [buffer pool](/kb/en/xtradbinnodb-buffer-pool/), whose size is controlled by [innodb_buffer_pool_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_size). By default it contains 16KB data and index blocks from the open tables (see [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size)), plus some maintenance overhead.

From [MariaDB 5.5](/kb/en/what-is-mariadb-55/), multiple buffer pools are permitted; this can help because there is one mutex per pool, thereby relieving some of the mutex bottleneck.

[More on InnoDB tuning](http://www.mysqlperformanceblog.com/2007/11/01/innodb-performance-optimization-basics/)

## Another Algorithm

This will set the main cache settings to the minimum; it could be important to systems with lots of other processes and/or RAM is 2GB or smaller.

Do [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status) for all the tables in all the databases.

Add up Index_length for all the MyISAM tables. Set [key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size) no larger than that size.

Add up Data_length + Index_length for all the InnoDB tables. Set [innodb_buffer_pool_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_size) to no more than 110% of that total.

If that leads to swapping, cut both settings back. Suggest cutting them down proportionately.

Run this to see the values for your system. (If you have a lot of tables, it can take minute(s).)

```sql
SELECT  ENGINE,
        ROUND(SUM(data_length) /1024/1024, 1) AS "Data MB",
        ROUND(SUM(index_length)/1024/1024, 1) AS "Index MB",
        ROUND(SUM(data_length + index_length)/1024/1024, 1) AS "Total MB",
        COUNT(*) "Num Tables"
    FROM  INFORMATION_SCHEMA.TABLES
    WHERE  table_schema not in ("information_schema", "PERFORMANCE_SCHEMA", "SYS_SCHEMA", "ndbinfo")
    GROUP BY  ENGINE;
```

## Query Memory Allocation

There are two variables that dictates how memory are allocated by MariaDB while parsing and executing a query.
[query_prealloc_size](/kb/en/server-system-variables/#query_prealloc_size) defines the standard buffer for memory used  for query execution and [query_alloc_block_size](/kb/en/server-system-variables/#query_alloc_block_size) that is size of memory blocks if `query_prealloc_size` was not big enough. Getting these variables right will reduce memory fragmentation in the server.

## Mutex Bottleneck

MySQL was designed in the days of single-CPU machines, and designed to be easily ported to many different architectures. Unfortunately, that lead to some sloppiness in how to interlock actions. There are a small number (too small) of "mutexes" to gain access to several critical processes. Of note:

- MyISAM's key_buffer
- The Query Cache
- InnoDB's buffer_pool
With multi-core boxes, the mutex problem is causing performance problems. In general, past 4-8 cores, MySQL gets slower, not faster. MySQL 5.5 and Percona's XtraDB made that somewhat better in InnoDB; the practical limit for cores is more like 32, and performance tends plateaus after that rather than declining. 5.6 claims to scale up to about 48 cores.

## HyperThreading and Multiple Cores (CPUs)

Short answers (for older versions of MySQL and MariaDB):

- Turn off HyperThreading
- Turn off any cores beyond 8
- HyperThreading is mostly a thing of the past, so this section may not apply.

HyperThreading is great for marketing, lousy for performance. It involves having two processing units sharing a single hardware cache. If both units are doing the same thing, the cache will be reasonably useful. If the units are doing different things, they will be clobbering each other's cache entries.

Furthermore MySQL is not great on using multiple cores. So, if you turn off HT, the remaining cores run a little faster.

## 32-bit OS and MariaDB

First, the OS (and the hardware?) may conspire to not let you use all 4GB, if that is what you have. If you have more than 4GB of RAM, the excess beyond 4GB is _totally_ inaccessable and unusable on a 32-bit OS.

Secondly, the OS probably has a limit on how much RAM it will allow any process to use.

Example: FreeBSD's maxdsiz, which defaults to 512MB.

Example:

```sql
$ ulimit -a
...
max memory size (kbytes, -m) 524288
```

So, once you have determined how much RAM is available to mysqld, then apply the 20%/70%, but round down some.

If you get an error like `[ERROR] /usr/libexec/mysqld: Out of memory (Needed xxx bytes)`, it probably means that MySQL exceeded what the OS is willing to give it. Decrease the cache settings.

## 64-bit OS with 32-bit MariaDB

The OS is not limited by 4GB, but MariaDB is.

If you have at least 4GB of RAM, then maybe these would be good:

- [key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size) = 20% of _all_ of RAM, but not more than 3G
- [innodb_buffer_pool_size](/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_size) = 3G

You should probably upgrade MariaDB to 64-bit.

## 64-bit OS and MariaDB

MyISAM only: [key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size): Use about 20% of RAM. Set (in my.cnf / my.ini) [innodb_buffer_pool_size=0](/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_size) = 0.

InnoDB only: [innodb_buffer_pool_size=0](/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_size) = 70% of RAM. If you have lots of RAM and are using 5.5 (or later), then consider having multiple pools. Recommend 1-16 [innodb_buffer_pool_instances](/kb/en/innodb-system-variables/#innodb_buffer_pool_instances), such that each one is no smaller than 1GB. (Sorry, no metric on how much this will help; probably not a lot.)

Meanwhile, set [key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size) = 20M (tiny, but non-zero)

If you have a mixture of engines, lower both numbers.

max_connections, thread_stack
Each "thread" takes some amount of RAM. This used to be about 200KB; 100 threads would be 20MB, not a significant size. If you have [max_connections](/kb/en/server-system-variables/#max_connections) = 1000, then you are talking about 200MB, maybe more. Having that many connections probably implies other issues that should be addressed.

In 5.6 (or [MariaDB 5.5](/kb/en/what-is-mariadb-55/)), optional thread pooling interacts with [max_connections](/kb/en/server-system-variables/#max_connections). This is a more advanced topic.

Thread stack overrun rarely happens. If it does, do something like thread_stack=256K

[More on max_connections, wait_timeout, connection pooling, etc](http://www.mysqlperformanceblog.com/2013/11/28/mysql-error-too-many-connections/)

## table_open_cache

(In older versions this was called table_cache)

The OS has some limit on the number of open files it will let a process have. Each table needs 1 to 3 open files. Each PARTITION is effectively a table. Most operations on a partitioned table open _all_ partitions.

In *nix, ulimit tells you what the file limit is. The maximum value is in the tens of thousands, but sometimes it is set to only 1024. This limits you to about 300 tables. More discussion on ulimit

(This paragraph is in disputed.) On the other side, the table cache is (was) inefficiently implemented -- lookups were done with a linear scan. Hence, setting table_cache in the thousands could actually slow down mysql. (Benchmarks have shown this.)

You can see how well your system is performing via [SHOW GLOBAL STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status); and computing the opens/second via [Opened_files](/kb/en/server-status-variables/#opened_files) / [Uptime](/kb/en/server-status-variables/#uptime) If this is more than, say, 5, [table_open_cache](/kb/en/server-system-variables/#table_open_cache) should be increased. If it is less than, say, 1, you might get improvement by decreasing [table_open_cache](/kb/en/server-system-variables/#table_open_cache).

From [MariaDB 10.1](/kb/en/what-is-mariadb-101/), [table_open_cache](/kb/en/server-system-variables/#table_open_cache) defaults to 2000.

## Query Cache

Short answer: [query_cache_type](/kb/en/server-system-variables/#query_cache_type) = OFF and [query_cache_size](/kb/en/server-system-variables/#query_cache_size) = 0

The [Query Cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache) (QC) is effectively a hash mapping SELECT statements to resultsets.

Long answer... There are many aspects of the "Query cache"; many are negative.

- Novice Alert! The QC is totally unrelated to the key_buffer and buffer_pool.
- When it is useful, the QC is blazingly fast. It would not be hard to create a benchmark that runs 1000x faster.
- There is a single mutex controlling the QC.
- The QC, unless it is OFF &amp; 0, is consulted for _every_ SELECT.
- Yes, the mutex is hit even if [query_cache_type](/kb/en/server-system-variables/#query_cache_type) = DEMAND (2).
- Yes, the mutex is hit even for [SQL_NO_CACHE](/kb/en/query-cache/#sql_no_cache-and-sql_cache).
- Any change to a query (even adding a space) leads (potentially) to a different entry in the QC.
- If my.cnf says type=ON and you later turn it OFF, it is not fully OFF. Ref: [https://bugs.mysql.com/bug.php?id=60696](https://bugs.mysql.com/bug.php?id=60696)

"Pruning" is costly and frequent:

- When _any_ write happens on a table, _all_ entries in the QC for _that_ table are removed.
- It happens even on a readonly Slave.
- Purges are performed with a linear algorithm, so a large QC (even 200MB) can be noticeably slow.

To see how well your QC is performing, SHOW GLOBAL STATUS LIKE 'Qc%'; then compute the read hit rate: [Qcache_hits](/kb/en/server-status-variables/#qcache_hits) / [Qcache_inserts](/kb/en/server-status-variables/#qcache_inserts) If it is over, say, 5, the QC might be worth keeping.

If you decide the QC is right for you, then I recommend

- [query_cache_size](/kb/en/server-system-variables/#query_cache_size) = no more than 50M
- [query_cache_type](/kb/en/server-system-variables/#query_cache_type) = DEMAND
- [SQL_CACHE or SQL_NO_CACHE](%5Bquery-cache/#sql_no_cache-and-sql_cache) in all SELECTs, based on which queries are likely to benefit from caching.

- [Why to turn off the QC](http://dba.stackexchange.com/a/136814/1876)
- [Discussion about size](https://haydenjames.io/mysql-query-cache-size-performance/)

## thread_cache_size

It is not necessary to tune [thread_cache_size](/kb/en/server-system-variables/#thread_cache_size) from [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/). Previously, it was minor tunable variable.  Zero will slow down thread (connection) creation. A small (say, 10), non-zero number is good. The setting has essentially no impact on RAM usage.

It is the number of extra processes to hang onto. It does not restrict the number of threads; [max_connections](/kb/en/server-system-variables/#max_connections) does.

## Binary Logs

If you have turned on [binary logging](/mariadb-administration/server-monitoring-logs/binary-log) (via [log_bin](/kb/en/replication-and-binary-log-system-variables/#log_bin)) for replication and/or point-in-time recovery, the system will create binary logs forever. That is, they can slowly fill up the disk. Suggest setting [expire_logs_days](/kb/en/replication-and-binary-log-system-variables/#expire_logs_days) = 14 to keep only 14 days' worth of logs.

## Swappiness

RHEL, in its infinite wisdom, decided to let you control how aggressively the OS will pre-emptively swap RAM. This is good in general, but lousy for MariaDB.

MariaDB would love for RAM allocations to be reasonably stable -- the caches are (mostly) pre-allocated; the threads, etc, are (mostly) of limited scope. ANY swapping is likely to severely hurt performance of MariaDB.

With a high value for swappiness, you lose some RAM because the OS is trying to keep a lot of space free for future allocations (that MySQL is not likely to need).

With swappiness = 0, the OS will probably crash rather than swap. I would rather have MariaDB limping than die. The latest recommendation is swappiness = 1. (2015)

[More confirmation](http://www.mysqlperformanceblog.com/2014/04/28/oom-relation-vm-swappiness0-new-kernel/)

Somewhere in between (say, 5?) might be a good value for a MariaDB-only server.

## NUMA

OK, it's time to complicate the architecture of how a CPU talks to RAM. NUMA (Non-Uniform Memory Access) enters the picture. Each CPU (or maybe socket with several cores) has a part of the RAM hanging off each. This leads to memory access being faster for local RAM, but slower (tens of cycles slower) for RAM hanging off other CPUs.

Then the OS enters the picture. In at least one case (RHEL?), two things seem to be done:

- OS allocations are pinned to the 'first' CPU's RAM.]
- Other allocations go by default to the first CPU until it is full.

Now for the problem.

- The OS and MariaDB have allocated all the 'first' RAM.
- MariaDB has allocated some of the second RAM.
- The OS needs to allocate something.
Ouch -- it is out of room in the one CPU where it is willing to allocate its stuff, so it swaps out some of MariaDB. Bad.

dmesg | grep -i numa # to see if you have numa

Probable solution: Configure the BIOS to "interleave" the RAM allocations. This should prevent the premature swapping, at the cost of off-CPU RAM accesses half the time. Well, you have the costly accesses anyway, since you really want to use all of RAM. Older MySQL versions: numactl --interleave=all. Or: [innodb_numa_interleave](/kb/en/innodb-system-variables/#innodb_numa_interleave)=1

Another possible solution: Turn numa off (if the OS has a way of doing that)

Overall performance loss/gain: A few percent.

## Huge Pages

This is another hardware performance gimmick.

For a CPU to access RAM, especially mapping a 64-bit address to somewhere in, say, 128GB or 'real' RAM, the TLB is used. (TLB = Translation Lookup Buffer.) Think of the TLB as a hardware associative memory lookup table; given a 64-bit virtual address, what is the real address.

Because it is an associative memory of finite size, sometimes there will be "misses" that require reaching into real RAM to resolve the lookup. This is costly, so should be avoided.

Normally, RAM is 'paged' in 4KB pieces; the TLB actually maps the top (64-12) bits into a specific page. Then the bottom 12 bits of the virtual address are carried over intact.

For example, 128GB of RAM broken 4KB pages means 32M page-table entries. This is a lot, and probably far exceeds the capacity of the TLB. So, enter the "Huge page" trick.

With the help of both the hardware and the OS, it is possible to have some of RAM in huge pages, of say 4MB (instead of 4KB). This leads to far fewer TLB entries, but it means the unit of paging is 4MB for such parts of RAM. Hence, huge pages tend to be non-pagable.

Now RAM is broken into pagable and non pagable parts; what parts can reasonably be non pagable? In MariaDB, the [Innodb Buffer Pool](/kb/en/xtradbinnodb-buffer-pool/) is a perfect candidate. So, by correctly configuring these, InnoDB can run a little faster:

- Huge pages enabled
- Tell the OS to allocate the right amount (namely to match the buffer_pool)
- Tell MariaDB to use huge pages

- [innodb memory usage vs swap](http://forums.mysql.com/read.php?22,384707,385002)

That thread has more details on what to look for and what to set.

Overall performance gain: A few percent. Yawn. Too much hassle for too little benefit.

Jumbo Pages? Turn off.

## ENGINE=MEMORY

The [Memory Storage Engine](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine) is a little-used alternative to [MyISAM](/kb/en/myisam/) and [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb). The data is not persistent, so it has limited uses. The size of a MEMORY table is limited to [max_heap_table_size](/kb/en/server-system-variables/#max_heap_table_size), which defaults to 16MB. I mention it in case you have changed the value to something huge; this would stealing from other possible uses of RAM.

## How to Set Variables

In the text file my.cnf (my.ini on Windows), add or modify a line to say something like

[innodb_buffer_pool_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_size) = 5G

That is, VARIABLE name, "=", and a value. Some abbreviations are allowed, such as M for million (1048576), G for billion.

For the server to see it, the settings must be in the "[mysqld]" section of the file.

The settings in my.cnf or my.ini will not take effect until you restart the server.

Most settings can be changed on the live system by connecting as user root (or other user with SUPER privilege) and doing something like

```sql
SET @@global.key_buffer_size = 77000000;
```

Note: No M or G suffix is allowed here.

To see the setting a global VARIABLE do something like

```sql
SHOW GLOBAL VARIABLES LIKE "key_buffer_size";
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| key_buffer_size | 76996608 |
+-----------------+----------+
```

Note that this particular setting was rounded down to some multiple that MariaDB liked.

You may want to do both (SET, and modify my.cnf) in order to make the change immediately and have it so that the next restart (for whatever reason) will again get the value.

## Web Server

A web server like Apache runs multiple threads. If each thread opens a connection to MariaDB, you could run out of connections. Make sure MaxClients (or equivalent) is set to some civilized number (under 50).

## Tools

- MySQLTuner
- TUNING-PRIMER

There are several tools that advise on memory. One misleading entry they come up with

Maximum possible memory usage: 31.3G (266% of installed RAM)

Don't let it scare you -- the formulas used are excessively conservative. They assume all of [max_connections](/kb/en/server-system-variables/#max_connections) are in use and active, and doing something memory-intensive.

Total fragmented tables: 23 This implies that OPTIMIZE TABLE _might_ help. I suggest it for tables with either a high percentage of "free space" (see SHOW TABLE STATUS) or where you know you do a lot of DELETEs and/or UPDATEs. Still, don't bother to OPTIMIZE too often. Once a month might suffice.

## MySQL 5.7

5.7 stores a lot more information in RAM, leading to the footprint being perhaps half a GB more than 5.6. See [Memory increase in 5.7](https://blogs.oracle.com/svetasmirnova/entry/memory_summary_tables_in_performance).

## Postlog

Created 2010; Refreshed Oct, 2012, Jan, 2014

The tips in this document apply to MySQL, MariaDB, and Percona.

## See Also

- [Configuring MariaDB for Optimal Performance](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-performance-advanced-configurations/configuring-mariadb-for-optimal-performance)
- More in-depth: Tocker's tuning for 5.6
- Irfan's InnoDB performance optimization basics (redux)
- 10 MySQL settings to tune after installation
- Magento
- Peter Zaitsev's take on such (5/2016)

Rick James graciously allowed us to use this article in the Knowledge Base.

[Rick James' site](http://mysql.rjweb.org/) has other useful tips, how-tos,
optimizations, and debugging tips.

Original source: [http://mysql.rjweb.org/doc.php/random](http://mysql.rjweb.org/doc.php/random)