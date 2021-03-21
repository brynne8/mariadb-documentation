# Server Monitoring &amp; Logs

MariaDB can keep a number of log files, including the error log, the binary log, the general query log and the slow query log.

- [Overview of MariaDB Logs](/mariadb-administration/server-monitoring-logs/overview-of-mariadb-logs/) — What to log and what not to log
- [Error Log](/mariadb-administration/server-monitoring-logs/error-log/) — Record of critical errors that occurred during the server's operation.
- [Setting the Language for Error Messages](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/setting-the-language-for-error-messages/) — Specifying the language for the server error messages.
- [General Query Log](/mariadb-administration/server-monitoring-logs/general-query-log/) — Log of every SQL query received from a client, as well as connects/disconnects
- [Slow Query Log](/mariadb-administration/server-monitoring-logs/slow-query-log/) — Logging slow queries
- [Rotating Logs on Unix and Linux](/mariadb-administration/server-monitoring-logs/rotating-logs-on-unix-and-linux/) — Rotating logs on Unix and Linux with logrotate.
- [Binary Log](/mariadb-administration/server-monitoring-logs/binary-log/) — Contains a record of all changes to the databases, both data and structure
- [InnoDB Redo Log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) — The redo log is used by InnoDB during crash recovery.
- [InnoDB Undo Log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log/) — InnoDB Undo log.
- [MyISAM Log](/mariadb-administration/server-monitoring-logs/myisam-log/) — Records all changes to MyISAM tables
- [Transaction Coordinator Log](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log/) — The transaction coordinator log (tc_log) is used to coordinate transactions...
- [SQL Error Log Plugin](/mariadb-administration/server-monitoring-logs/sql-error-log-plugin/) — Records SQL-level errors to a log file.
- [Writing Logs Into Tables](/mariadb-administration/server-monitoring-logs/writing-logs-into-tables/) — The general query log and the slow query log can be written into system tables
- [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/) — Monitoring server performance.
- [MariaDB Audit Plugin](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/) — Logging user activity with the MariaDB Audit Plugin.