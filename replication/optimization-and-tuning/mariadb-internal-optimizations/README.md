# MariaDB Internal Optimizations

Different optimizations strategies done internally in MariaDB

- [Binary Log Group Commit and InnoDB Flushing Performance](/columns-storage-engines-and-plugins/storage-engines/innodb/binary-log-group-commit-and-innodb-flushing-performance/) — Improvement for group commit for InnoDB transactions with the binary log enabled
- [Fair Choice Between Range and Index_merge Optimizations](/replication/optimization-and-tuning/mariadb-internal-optimizations/fair-choice-between-range-and-index_merge-optimizations/) — index_merge is a method used by the optimizer to retrieve rows from a
singl...
- [Improvements to ORDER BY Optimization](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/improvements-to-order-by/) — Several Improvements to the ORDER BY Optimizer in Version 10.1 of MariaDB.
- [Multi Range Read Optimization](/replication/optimization-and-tuning/mariadb-internal-optimizations/multi-range-read-optimization/) — An optimization for improving performance of IO-bound queries which scan many rows.