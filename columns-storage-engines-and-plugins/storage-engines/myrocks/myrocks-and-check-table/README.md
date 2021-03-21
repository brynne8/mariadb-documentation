# MyRocks and CHECK TABLE

MyRocks supports the [CHECK TABLE](/sql-statements-structure/sql-statements/table-statements/check-table/) command.

The command will do a number of checks to verify that the table data is self-consistent.

The details about the errors are printed into the [error log](/mariadb-administration/server-monitoring-logs/error-log/).
If [log_warnings](/kb/en/server-system-variables/#log_warnings) &gt; 2,  the error log will also have some informational messages which can help with troubleshooting.

Besides this, RocksDB has its own (low-level) log in  `#rocksdb/LOG` file.