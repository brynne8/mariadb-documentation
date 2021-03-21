# MariaDB Optimization for MySQL Users

MariaDB contains many new options and optimizations which, for
compatibility or other reasons, are not enabled in the default install.
Enabling them helps you gain extra performance from the same hardware
when upgrading from MySQL to MariaDB. This article contains information
on options and settings which you should enable, or at least look in
to, when making the switch.

```sql
aria-pagecache-buffer-size=##
```

If you are using a log of on-disk temporary tables, increase the above to as much as you can afford. See [Aria Storage Engine](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine) for more details.

```sql
key-cache-segments=8
```

If you use/have a lot of MyISAM files, increase the above to 4 or 8. See [Segmented Key Cache](/replication/optimization-and-tuning/system-variables/segmented-key-cache) and [Segmented Key Cache Performance](/kb/en/segmented-key-cache-performance/) for more information.

```sql
thread-handling=pool-of-threads
```

Threadpool is a great way to increase performance in situations where queries are relatively short and the load is CPU bound (e.g. OLTP workloads). To enable it, add the above to your my.cnf file. See [Threadpool in 5.5](/kb/en/threadpool-in-55/) for more information.

```sql
innodb-buffer-pool-instances=
```