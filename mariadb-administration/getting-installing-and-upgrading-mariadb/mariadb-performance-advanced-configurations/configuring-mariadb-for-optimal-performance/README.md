# Configuring MariaDB for Optimal Performance

This article is to help you configure MariaDB for optimal performance.

Note that by default MariaDB is configured to work on a desktop system and should because of this not take a lot of resources. To get things to work for a dedicated server, you have to do a few minutes of work.

For this article we assume that you are going to run MariaDB on a dedicated server.

Feel free to update this article if you have more ideas!

## [my.cnf](/kb/en/configuring-mariadb-with-mycnf/) Files

MariaDB is normally configured by editing the [my.cnf](/mariadb-administration/getting-installing-and-upgrading-mariadb/mysqld-configuration-files-and-groups) file.

The following my.cnf example files were included with MariaDB until [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/). If present, you can examine them to see more complete examples of some of the many ways to configure MariaDB and use the one that fits you best as a base. Note that these files are now quite outdated, so what was huge a few years ago may no longer be seen as such.

- my-small.cnf
- my-medium.cnf
- my-large.cnf
- my-huge.cnf

## [InnoDB &amp; XtraDB](/kb/en/xtradb-and-innodb/) Storage Engine

InnoDB or XtraDB is normally the default storage engine with MariaDB.

- You should set [innodb_buffer_pool_size](/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_size) to about 80% of your memory.  The goal is to ensure that 80 % of your working set is in memory!

The other most important InnoDB variables are:

- [innodb_log_file_size](/kb/en/innodb-system-variables/#innodb_log_file_size)
- [innodb_flush_method](/kb/en/innodb-system-variables/#innodb_flush_method)
- [innodb_thread_sleep_delay](/kb/en/innodb-system-variables/#innodb_thread_sleep_delay)

Some other important InnoDB variables:

- [innodb_adaptive_max_sleep_delay](/kb/en/innodb-system-variables/#innodb_adaptive_max_sleep_delay)
- [innodb_buffer_pool_instances](/kb/en/innodb-system-variables/#innodb_buffer_pool_instances)
- [innodb_buffer_pool_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_size)
- [innodb_max_dirty_pages_pct_lwm](/kb/en/innodb-system-variables/#innodb_max_dirty_pages_pct_lwm)
- [innodb_read_ahead_threshold](/kb/en/innodb-system-variables/#innodb_read_ahead_threshold)
- [innodb_thread_concurrency](/kb/en/innodb-system-variables/#innodb_thread_concurrency)

## [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine) Storage Engine

- MariaDB uses by default the Aria storage engine for internal temporary files. If you have many temporary files, you should set [aria_pagecache_buffer_size](/kb/en/aria-server-system-variables/#aria_pagecache_buffer_size) to a reasonably large value so that temporary overflow data is not flushed to disk. The default is 128M.

## [MyISAM](/kb/en/myisam/)

- If you don't use MyISAM tables explicitly (true for most [MariaDB 10.4](/kb/en/what-is-mariadb-104/)+ users), you can set [key_buffer_size](/kb/en/myisam-system-variables/#key_buffer_size) to a very low value, like 64K.

## Lots of Connections

### A Lot of Fast Connections + Small Set of Queries + Disconnects

- If you are doing a lot of fast connections / disconnects, you should increase [back_log](/kb/en/server-system-variables/#back_log) and if you are running [MariaDB 10.1](/kb/en/what-is-mariadb-101/) or below [thread_cache_size](/kb/en/server-system-variables/#thread_cache_size).
- If you have a lot (&gt; 128) of simultaneous running fast queries, you should consider setting [thread_handling](/kb/en/thread-pool-system-and-status-variables/#thread_handling) to <code class="highlight fixed" style="white-space:pre-wrap">pool_of_threads</code>.

### Connecting From a Lot of Different Machines

- If you are connecting from a lot of different machines you should increase [host_cache_size](/kb/en/server-system-variables/#host_cache_size) to the max number of machines (default 128) to cache the resolving of hostnames.  If you don't connect from a lot of machines, you can set this to a very low value!

## See Also

- [MariaDB Memory Allocation](/replication/optimization-and-tuning/mariadb-memory-allocation)
- [Full List of MariaDB Options, System and Status Variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables)
- [Server system variables](/replication/optimization-and-tuning/system-variables/server-system-variables)
- [mysqld options](/kb/en/mysqld-options-full-list/)
- [Performance schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema) helps you understand what is taking time and resources.
- [Slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log) is used to find queries that are running slow.
- [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table) helps you defragment tables.

## External Links

- [http://www.tocker.ca/2013/09/17/what-to-tune-in-mysql-56-after-installation.html](http://www.tocker.ca/2013/09/17/what-to-tune-in-mysql-56-after-installation.html)
- [http://www.percona.com/resources/technical-presentations/optimizing-mysql-configuration-percona-mysql-university-montevideo](http://www.percona.com/resources/technical-presentations/optimizing-mysql-configuration-percona-mysql-university-montevideo)