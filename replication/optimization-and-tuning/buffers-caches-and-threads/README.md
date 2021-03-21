# Buffers, Caches and Threads

MariaDB makes use of a number of buffering, caching and threading techniques to improve performance.

- [Thread Pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/) — MariaDB thread pool
- [Thread States](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-states/) — Descriptions of the various thread states
- [InnoDB Buffer Pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/) — The most important memory buffer used by InnoDB.
- [InnoDB Change Buffering](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-change-buffering/) — Buffering INSERT, UPDATE and DELETE  statements for greater efficiency.
- [Query Cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache/) — Caching SELECT queries for better performance.
- [Segmented Key Cache](/replication/optimization-and-tuning/system-variables/segmented-key-cache/) — Collection of structures for regular MyISAM key caches
- [Subquery Cache](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/subquery-cache/) — Subquery cache for optimizing the evaluation of correlated subqueries.
- [Thread Command Values](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-command-values/) — Thread command values from SHOW PROCESSLIST or Information Schema PROCESSLIST Table