# InnoDB Buffer Pool

The [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) buffer pool is a key component for optimizing MariaDB. It stores data and indexes, and you usually want it as large as possible so as to keep as much of the data and indexes in memory, reducing disk IO, as main bottleneck.

## How the Buffer Pool Works

The buffer pool attempts to keep frequently-used blocks in the buffer, and so essentially works as two sublists, a <em>new</em> sublist of recently-used information, and an <em>old</em> sublist of older information. By default, 37% of the list is reserved for the old list.

When new information is accessed that doesn't appear in the list, it is placed at the top of the old list, the oldest item in the old list is removed, and everything else bumps back one position in the list.

When information is accessed that appears in the <em>old</em> list, it is moved to the top the new list, and everything above moves back one position.

## innodb_buffer_pool_size

The most important [server system variable](/replication/optimization-and-tuning/system-variables/server-system-variables) is [innodb_buffer_pool_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_size), which you can set from 70-80% of the total available memory on a dedicated database server with only or primarily InnoDB tables.

Be aware that the total memory allocated is about 10% more than the specified size as extra space is also reserved for control structures and buffers. The space must also be contiguous. If you're running a Windows system that loads DLL's at specific addresses, this may cause difficulties.

The larger the size, the longer it will take to initialize. On a modern 64-bit server with a 10GB memory pool, this can take five seconds or more.

Make sure that the size is not too large, causing swapping. The benefit of a larger buffer pool size is more than undone if your operating system is regularly swapping.

Since [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), the buffer pool can be set dynamically, and new variables are introduced that may affect the size and performance. See [Setting Innodb Buffer Pool Size Dynamically](/replication/optimization-and-tuning/system-variables/setting-innodb-buffer-pool-size-dynamically).

## innodb_buffer_pool_instances

The functionality described below was disabled in [MariaDB 10.5](/kb/en/what-is-mariadb-105/), and removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/), as the original reasons for for splitting the buffer pool have mostly gone away.

If innodb_buffer_pool_size is set to more than 1GB, [innodb_buffer_pool_instances](/kb/en/innodb-system-variables/#innodb_buffer_pool_instances) divides the InnoDB buffer pool into a specific number of instances. The default was 1 in [MariaDB 5.5](/kb/en/what-is-mariadb-55/), but for large systems with buffer pools of many gigabytes, many instances can help reduce contention concurrency. The default is 8 in [MariaDB 10.0](/kb/en/what-is-mariadb-100/), with the exception of 32-bit Windows, where it depends on the value of [innodb_buffer_pool_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_instances). Each instance manages its own data structures and takes an equal portion of the total buffer pool size, so for example if innodb_buffer_pool_size is 4GB and innodb_buffer_pool_instances is set to 4, each instance will be 1GB. Each instance should ideally be at least 1GB in size.

## innodb_old_blocks_pct and innodb_old_blocks_time

The default 37% reserved for the old list can be adjusted by changing the value of [innodb_old_blocks_pct](/kb/en/innodb-system-variables/#innodb_old_blocks_pct). It can accept anything between between 5% and 95%.

The [innodb_old_blocks_time](/kb/en/innodb-system-variables/#innodb_old_blocks_time) variable specifies the delay before a block can be moved from the old to the new sublist. `0` means no delay, while the default has been set to `1000`.

Before changing either of these values from their defaults, make sure you understand the impact and how your system currently uses the buffer. Their main reason for existence is to reduce the impact of full table scans, which are usually infrequent, but large, and previously could clear everything from the buffer. Setting a non-zero delay could help in situations where full table scans are performed in quick succession.

Temporarily changing these values can also be useful to avoid the negative impact of a full table scan, as explained in [InnoDB logical backups](/kb/en/backup-and-restore-overview/#innodb-logical-backups).

## Dumping and Restoring the Buffer Pool

When the server starts, the buffer pool is empty. As it starts to access data, the buffer pool will slowly be populated. As more data will be accessed, the most frequently accessed data will be put into the buffer pool, and old data may be evicted. This means that a certain period of time is necessary before the buffer pool is really useful. This period of time is called the warmup.

Since [MariaDB 10.0](/kb/en/what-is-mariadb-100/), InnoDB can dump the buffer pool before the server shuts down, and restore it when it starts again. If this feature is used, no warmup is necessary. To enable the buffer pool dump at shutdown and the restore at startup respectively, the [innodb_buffer_pool_dump_at_shutdown](/kb/en/innodb-system-variables/#innodb_buffer_pool_dump_at_shutdown) and [innodb_buffer_pool_load_at_startup](/kb/en/innodb-system-variables/#innodb_buffer_pool_load_at_startup) system variables can be set to ON.

It is also possible to dump the InnoDB buffer pool at any moment while the server is running, and it is possible to restore the last buffer pool dump at any moment. To do this, the special [innodb_buffer_pool_dump_now](/kb/en/innodb-system-variables/#innodb_buffer_pool_dump_now) and [innodb_buffer_pool_load_now](/kb/en/innodb-system-variables/#innodb_buffer_pool_load_now) system variables can be set to ON. When selected, their value is always OFF.

A buffer pool restore, both at startup or at any other moment, can be aborted by setting [innodb_buffer_pool_load_abort](/kb/en/innodb-system-variables/#innodb_buffer_pool_load_abort) to ON.

The file which contains the buffer pool dump is specified via the [innodb_buffer_pool_filename](/kb/en/innodb-system-variables/#innodb_buffer_pool_filename) system variable.

## See Also

- [InnoDB Change Buffering](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-change-buffering)
- [Information Schema INNODB_BUFFER_POOL_STATS Table](/kb/en/information-schema-innodb_buffer_pool_stats-table/)
- [Setting Innodb Buffer Pool Size Dynamically](/replication/optimization-and-tuning/system-variables/setting-innodb-buffer-pool-size-dynamically)