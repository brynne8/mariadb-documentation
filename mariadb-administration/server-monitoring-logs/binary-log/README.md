# Binary Log

The binary log contains a record of all changes to the databases, both data and structure. It consists of a set of binary log files and an index.

It is necessary for [replication](/replication), and can also be used to restore data after a backup.

- [Overview of the Binary Log](/mariadb-administration/server-monitoring-logs/binary-log/overview-of-the-binary-log/) — The binary log contains a record of all changes to the databases
- [Activating the Binary Log](/mariadb-administration/server-monitoring-logs/binary-log/activating-the-binary-log/) — Activating the Binary Log.
- [Using and Maintaining the Binary Log](/mariadb-administration/server-monitoring-logs/binary-log/using-and-maintaining-the-binary-log/) — Using and maintaining the binary log.
- [Binary Log Formats](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats/) — The three binary logging formats.
- [Binary Logging of Stored Routines](/programming-customizing-mariadb/stored-routines/binary-logging-of-stored-routines/) — Stored routines require extra consideration when binary logging.
- [SHOW BINARY LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-binary-logs/) — SHOW BINARY LOGS lists all binary logs on the server.
- [PURGE BINARY LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/purge-binary-logs/) — PURGE BINARY LOGS removes all binary logs from the server, prior to the provided date or log file.
- [SHOW BINLOG EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-binlog-events/) — Show events in the binary log.
- [SHOW MASTER STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-binlog-status/) — Status information about the binary log.
- [Binlog Event Checksums](/replication/standard-replication/binlog-event-checksums/) — Including a checksum in binlog events.
- [Binlog Event Checksum Interoperability](/replication/standard-replication/binlog-event-checksum-interoperability/) — Replicating between servers with differing binlog checksum availability
- [Group Commit for the Binary Log](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/) — Optimization when the server is run with innodb_flush_logs_at_trx_commit or sync_binlog.
- [mysqlbinlog](/clients-utilities/mysqlbinlog/) — mysqlbinlog utility for processing binary log files.
- [Transaction Coordinator Log](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log/) — The transaction coordinator log (tc_log) is used to coordinate transactions...
- [Encrypting Binary Logs](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/encrypting-binary-logs/) — Data-at-rest encryption for binary logs and relay logs.
- [Flashback](/mariadb-administration/server-monitoring-logs/binary-log/flashback/) — Rollback instances/databases/tables to an old snapshot.
- [Relay Log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) — Event log created by the replica from the primary binary log.
- [Replication and Binary Log System Variables](/replication/standard-replication/replication-and-binary-log-system-variables/) — Replication and binary log system variables.