# MariaDB Replication

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

Replication is a feature allowing the contents of one or more primary servers to be mirrored on one or more replica servers.

- [Replication Overview](/replication/standard-replication/replication-overview/) — Allow the contents of one or more master servers to be mirrored on one or more slave servers.
- [Replication Commands](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/) — List of replication-related commands.
- [Setting Up Replication](/replication/standard-replication/setting-up-replication/) — Getting replication working involves steps on both the master server/s and the slave server/s.
- [Setting up a Replication Slave with Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/setting-up-a-replication-slave-with-mariabackup/) — Setting up a replication slave with Mariabackup.
- [Read-Only Replicas](/replication/standard-replication/read-only-replicas/) — Making replicas read-only.
- [Replication as a Backup Solution](/mariadb-administration/backing-up-and-restoring-databases/replication-as-a-backup-solution/) — Replication can be used to support the backup strategy.
- [Multi-Source Replication](/replication/standard-replication/multi-source-replication/) — Using replication with many masters.
- [Replication Threads](/replication/standard-replication/replication-threads/) — Types of threads that are used to enable replication.
- [Global Transaction ID](/replication/standard-replication/gtid/) — Improved replication using global transaction IDs.
- [Parallel Replication](/replication/standard-replication/parallel-replication/) — Executing queries replicated from the master in parallel on the slave.
- [Replication and Binary Log System Variables](/replication/standard-replication/replication-and-binary-log-system-variables/) — Replication and binary log system variables.
- [Replication and Binary Log Status Variables](/replication/standard-replication/replication-and-binary-log-status-variables/) — Replication and binary log status variables.
- [Binary Log](/mariadb-administration/server-monitoring-logs/binary-log/) — Contains a record of all changes to the databases, both data and structure
- [Unsafe Statements for Statement-based Replication](/replication/standard-replication/unsafe-statements-for-statement-based-replication/) — Statements that are not safe for statement-based replication.
- [Replication and Foreign Keys](/replication/standard-replication/replication-and-foreign-keys/) — Cascading deletes or updates based on foreign key relations are not written to the binary log
- [Relay Log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) — Event log created by the replica from the primary binary log.
- [Enhancements for START TRANSACTION WITH CONSISTENT SNAPSHOT](/replication/standard-replication/enhancements-for-start-transaction-with-consistent-snapshot/) — Enhancements for START TRANSACTION WITH CONSISTENT SNAPSHOT.
- [Group Commit for the Binary Log](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/) — Optimization when the server is run with innodb_flush_logs_at_trx_commit or sync_binlog.
- [Selectively Skipping Replication of Binlog Events](/replication/standard-replication/selectively-skipping-replication-of-binlog-events/) — @@skip_replication and --replicate-events-marked-for-skip.
- [Binlog Event Checksums](/replication/standard-replication/binlog-event-checksums/) — Including a checksum in binlog events.
- [Binlog Event Checksum Interoperability](/replication/standard-replication/binlog-event-checksum-interoperability/) — Replicating between servers with differing binlog checksum availability
- [Annotate_rows_log_event](/clients-utilities/mysqlbinlog/annotate_rows_log_event/) — Annotate_rows events accompany row events and describe the query which caused the row event.
- [Row-based Replication With No Primary Key](/replication/standard-replication/row-based-replication-with-no-primary-key/) — MariaDB improves on row-based replication of tables with no primary key
- [Replication Filters](/replication/standard-replication/replication-filters/) — Replication filters allow users to configure replication slaves to intentio...
- [Running Triggers on the Slave for Row-based Events](/replication/standard-replication/running-triggers-on-the-slave-for-row-based-events/) — Running triggers on the slave for row-based events.
- [Semisynchronous Replication](/replication/standard-replication/semisynchronous-replication/) — Semisynchronous replication.
- [Using MariaDB Replication with MariaDB Galera Cluster](/replication/galera-cluster/using-mariadb-replication-with-mariadb-galera-cluster/) — Information on using MariaDB replication with MariaDB Galera Cluster.
- [Compressing Events to Reduce Size of the Binary Log](/replication/standard-replication/compressing-events-to-reduce-size-of-the-binary-log/) — Binlog events can be compressed to save space on disk and in network transfers
- [Delayed Replication](/replication/standard-replication/delayed-replication/) — Specify that a slave should lag behind the master by (at least) a specified amount of time.
- [Replication When the Master and Slave Have Different Table Definitions](/replication/standard-replication/replication-when-the-master-and-slave-have-different-table-definitions/) — Slave and the master table definitions can differ while replicating.
- [Restricting speed of reading binlog from master by a slave](/replication/standard-replication/restricting-speed-of-reading-binlog-from-master-by-a-slave/) — The read_binlog_speed_limit option can be used to reduce load on the master.
- [Changing a Slave to Become the Master](/replication/standard-replication/changing-a-slave-to-become-the-master/) — How to change a slave to master and old master as a slave for the new master.
- [Replication with Secure Connections](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/replication-with-secure-connections/) — Enabling TLS encryption in transit for MariaDB replication.
- [Obsolete Replication Information](/replication/standard-replication/obsolete-replication-information/) — This section is for replication-related items that are obsolete