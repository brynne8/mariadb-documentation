# Optimizing table_open_cache

<em>[table_open_cache](/kb/en/server-system-variables/#table_open_cache)</em> can be a useful variable to adjust to improve performance.

Each concurrent session accessing the same table does so independently. This improves performance, although it comes at a cost of extra memory usage.

<em>table_open_cache</em> indicates the maximum number of tables the server can keep open in any one table cache instance. Ideally, you'd like this set so as to re-open a table as infrequently as possible.

However, note that this is not a hard limit. When the server needs to open a table, it evicts the least recently used closed table from the cache, and adds the new table. If all tables are used, the server adds the new table and does not evict any table. As soon as a table is not used anymore, it will be evicted from the list even if no table needs to be open, until the number of open tables will be equal to <em>table_open_cache</em>

<em>table_open_cache</em> has defaulted to 2000 since [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/). Before that, the default was 400.

You can view the current setting in the my.cnf file, or by running:

```sql
select @@table_open_cache;
+--------------------+
| @@table_open_cache |
+--------------------+
|                400 |
+--------------------+
```

To evaluate whether you could do with a higher table_open_cache, look at the number of opened tables, in conjunction with the server uptime ([Opened_tables](/kb/en/server-status-variables/#opened_tables) and [Uptime](/kb/en/server-status-variables/#uptime) status variables):

```sql
show global status like 'opened_tables';
+---------------+--------+
| Variable_name | Value  |
+---------------+--------+
| Opened_tables | 354858 |
+---------------+--------+
```

If the number of opened tables is increasing rapidly, you should look at increasing the table_open_cache value. Try to find a value that sees a slow, or possibly even no, increase in the number of opened tables.

Make sure that your operating system can cope with the number of open file descriptors required by the table_open_cache setting. If table_open_cache is set too high, MariaDB may start to refuse connections as the operating system runs out of file descriptors. Also note that the MyISAM (and Aria?) storage engines need two file descriptors per open table.

It's possible that the open_table_cache can even be reduced.

If your number of open_tables has not yet reached the table_open_cache_size, and the server has been up a while, you can look at decreasing the value.

```sql
show global status like 'open_tables';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Open_tables   | 354   |
+---------------+-------+
```

The open table cache can be emptied with [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) or with the `flush-tables` or `refresh` [mysqladmin](/clients-utilities/mysqladmin) commands.

## Automatic Creation of New Table Open Cache Instances

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, MariaDB Server can create multiple instances of the table open cache. It initially creates just a single instance. However, whenever it detects contention on the existing instances, it will automatically create a new instance. When the number of instances has been increased due to contention, it does not decrease again.

When MariaDB Server creates a new instance, it prints a message like the following to the [error log](/mariadb-administration/server-monitoring-logs/error-log):

```sql
[Note] Detected table cache mutex contention at instance 1: 25% waits. Additional 
  table cache instance activated. Number of instances after activation: 2.
```

The maximum number of instances is defined by the <a undefined>table_open_cache_instances</a> system variable. The default value of the <a undefined>table_open_cache_instances</a> system variable is `8`, which is expected to handle up to 100 CPU cores. If your system is larger than this, then you may benefit from increasing the value of this system variable.

Depending on the ratio of actual available file handles, and <a undefined>table_open_cache</a> size, the max. instance count may be auto adjusted to a lower value on server startup.

The implementation and behavior of this feature is different than the same feature in MySQL 5.6.