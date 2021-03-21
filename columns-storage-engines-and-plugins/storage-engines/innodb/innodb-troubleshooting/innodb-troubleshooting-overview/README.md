# InnoDB Troubleshooting Overview

As with most errors, first take a look at the contents of the [MariaDB error log](/mariadb-administration/server-monitoring-logs/error-log/). If dealing with a deadlock, setting the [innodb_print_all_deadlocks](/kb/en/xtradbinnodb-server-system-variables/#innodb_print_all_deadlocks) option (off by default) will output details of all deadlocks to the error log.

It can also help to enable the various [InnoDB Monitors](/columns-storage-engines-and-plugins/storage-engines/innodb/xtradb-innodb-monitors/) relating to the problem you are experiencing. There are four types: the standard InnoDB monitor, the InnoDB Lock Monitor, InnoDB Tablespace Monitor and the InnoDB Table Monitor.

Running [CHECK TABLE](/sql-statements-structure/sql-statements/table-statements/check-table/) will help determine whether there are errors in the table.

For problems with the InnoDB Data Dictionary, see [InnoDB Data Dictionary Troubleshooting](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-troubleshooting/innodb-data-dictionary-troubleshooting/).

## See Also

- [InnoDB Data Dictionary Troubleshooting](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-troubleshooting/innodb-data-dictionary-troubleshooting/)
- [XtraDB/InnoDB Recovery Modes](/kb/en/xtradbinnodb-recovery-modes/)
- [Error Codes](/kb/en/error-codes/)