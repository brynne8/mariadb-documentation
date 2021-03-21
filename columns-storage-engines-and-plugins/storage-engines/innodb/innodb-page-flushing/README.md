# InnoDB Page Flushing

## Page Flushing with InnoDB Page Cleaner Threads

InnoDB page cleaner threads flush dirty pages from the [InnoDB buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool). These dirty pages are flushed using a least-recently used (LRU) algorithm.

### Page Flushing with Multiple InnoDB Page Cleaner Threads

##### MariaDB [10.2.2](/kb/en/mariadb-1022-release-notes/) - [10.5.1](/kb/en/mariadb-1051-release-notes/)

The [innodb_page_cleaners](/kb/en/innodb-system-variables/#innodb_page_cleaners) system variable was added in [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), and makes it possible to use multiple InnoDB page cleaner threads. It is deprecated and ignored from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/), as the original reasons for for splitting the buffer pool have mostly gone away.

The number of InnoDB page cleaner threads can be configured by setting the [innodb_page_cleaners](/kb/en/innodb-system-variables/#innodb_page_cleaners) system variable. This system variable can be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) prior to starting up the server. For example:

```sql
[mariadb]
...
innodb_page_cleaners=8
```

In [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/) and later, this system variable can also be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL innodb_page_cleaners=8;
```

This system variable's default value is either `4` or the configured value of the [innodb_buffer_pool_instances](/kb/en/innodb-system-variables/#innodb_buffer_pool_instances) system variable, whichever is lower.

### Page Flushing with a Single InnoDB Page Cleaner Thread

In [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) and before, and from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/), when the original reasons for splitting the buffer pool have mostly gone away, only a single InnoDB page cleaner thread is supported.

## Page Flushing with Multi-threaded Flush Threads

##### MariaDB [10.1.0](/kb/en/mariadb-1010-release-notes/) - [10.3.2](/kb/en/mariadb-1032-release-notes/)

InnoDB's multi-thread flush feature was first added in [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/). It was deprecated in [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/) and removed in [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/).

In [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/) and before, InnoDB's multi-thread flush feature can be used. This is especially useful in [MariaDB 10.1](/kb/en/what-is-mariadb-101/), which only supports a single page cleaner thread.

InnoDB's multi-thread flush feature can be enabled by setting the [innodb_use_mtflush](/kb/en/innodb-system-variables/#innodb_use_mtflush) system variable. The number of threads cane be configured by setting the [innodb_mtflush_threads](/kb/en/innodb-system-variables/#innodb_mtflush_threads) system variable. This system variable can be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) prior to starting up the server. For example:

```sql
[mariadb]
...
innodb_use_mtflush = ON
innodb_mtflush_threads = 8
```

The [innodb_mtflush_threads](/kb/en/innodb-system-variables/#innodb_mtflush_threads) system variable's default value is `8`. The maximum value is `64`.  In multi-core systems, it is recommended to set its value close to the configured value of the [innodb_buffer_pool_instances](/kb/en/innodb-system-variables/#innodb_buffer_pool_instances) system variable. However, it is also recommended to use your own benchmarks to find a suitable value for your particular application.

InnoDB's multi-thread flush feature was deprecated in [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/) and removed from [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/). In later versions of MariaDB, use multiple InnoDB page cleaner threads instead.

## Configuring the InnoDB I/O Capacity

Increasing the amount of I/O capacity available to InnoDB can also help increase the performance of page flushing.

The amount of I/O capacity available to InnoDB can be configured by setting the [innodb_io_capacity](/kb/en/innodb-system-variables/#innodb_io_capacity) system variable. This system variable can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL innodb_io_capacity=20000;
```

This system variable can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) prior to starting up the server. For example:

```sql
[mariadb]
...
innodb_io_capacity=20000
```

The maximum amount of I/O capacity available to InnoDB in an emergency can be configured by setting the [innodb_io_capacity_max](/kb/en/innodb-system-variables/#innodb_io_capacity_max) system variable. This system variable can be changed dynamically with [SET GLOBAL](/kb/en/set/#global-session). For example:

```sql
SET GLOBAL innodb_io_capacity_max=20000;
```

This system variable can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) prior to starting up the server. For example:

```sql
[mariadb]
...
innodb_io_capacity_max=20000
```

## See Also

- [Significant performance boost with new MariaDB page compression on FusionIO](https://blog.mariadb.org/significant-performance-boost-with-new-mariadb-page-compression-on-fusionio/)