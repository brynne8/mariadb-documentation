# Replication Commands

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

A list of replication-related commands. See [replication](/replication/) for more replication-related information.

- [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) — Set or change replica parameters for connecting to the primary.
- [START SLAVE](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/start-replica/) — Start replica threads.
- [STOP SLAVE](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/stop-replica/) — Stop replica threads.
- [RESET SLAVE](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-slave-connection_name/) — Forget slave connection information and start a new relay log file.
- [SET GLOBAL SQL_SLAVE_SKIP_COUNTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/set-global-sql_slave_skip_counter/) — Skips a number of events from the master.
- [SHOW RELAYLOG EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-relaylog-events/) — Show events in the relay log.
- [SHOW SLAVE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-replica-status/) — Show status for one or all masters.
- [SHOW MASTER STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-binlog-status/) — Status information about the binary log.
- [SHOW SLAVE HOSTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-replica-hosts/) — Display replicas currently registered with the primary.
- [RESET MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-master/) — Delete binary log files.