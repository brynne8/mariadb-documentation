# InnoDB Monitors

The [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) Monitor refers to particular kinds of monitors included in MariaDB and since the early versions of MySQL.

There are four types: the standard InnoDB monitor, the InnoDB Lock Monitor, InnoDB Tablespace Monitor and the InnoDB Table Monitor.

## Standard InnoDB Monitor

The standard InnoDB Monitor returns extensive InnoDB information, particularly lock, semaphore, I/O and buffer activity:

To enable the standard InnoDB Monitor, from [MariaDB 10.0.14](/kb/en/mariadb-10014-release-notes/), set the [innodb_status_output](/kb/en/xtradbinnodb-server-system-variables/#innodb_status_output) system variable to 1. Before [MariaDB 10.0.14](/kb/en/mariadb-10014-release-notes/), running the following statement was the method used:

```sql
CREATE TABLE innodb_monitor (a INT) ENGINE=INNODB;
```

To disable the standard InnoDB monitor, either set the system variable to zero, or, before [MariaDB 10.0.14](/kb/en/mariadb-10014-release-notes/), drop the table

```sql
DROP TABLE innodb_monitor;
```

The CREATE TABLE and DROP TABLE method of enabling and disabling the InnoDB Monitor has been deprecated, and may be removed in a future version of MariaDB.

For a description of the output, see [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status/).

## InnoDB Lock Monitor

The InnoDB Lock Monitor displays additional lock information.

To enable the InnoDB Lock Monitor, the standard InnoDB monitor must be enabled. Then, from [MariaDB 10.0.14](/kb/en/mariadb-10014-release-notes/), set the [innodb_status_output_locks](/kb/en/xtradbinnodb-server-system-variables/#innodb_status_output_locks) system variable to 1. 
Before [MariaDB 10.0.14](/kb/en/mariadb-10014-release-notes/), running the following statement was the method used:

```sql
CREATE TABLE innodb_lock_monitor (a INT) ENGINE=INNODB;
```

To disable the standard InnoDB monitor, either set the system variable to zero, or, before [MariaDB 10.0.14](/kb/en/mariadb-10014-release-notes/), drop the table

```sql
DROP TABLE innodb_lock_monitor;
```

The CREATE TABLE and DROP TABLE method of enabling and disabling the InnoDB Lock Monitor has been deprecated, and may be removed in a future version of MariaDB.

## InnoDB Tablespace Monitor

The InnoDB Tablespace Monitor is deprecated, and may be removed in a future version of MariaDB.

Enabling the Tablespace Monitor outputs a list of file segments in the shared tablespace to the error log, and validates the tablespace allocation data structures.

To  enable the Tablespace Monitor, run the following statement:

```sql
CREATE TABLE innodb_tablespace_monitor (a INT) ENGINE=INNODB;
```

To disable it, drop the table:

```sql
DROP TABLE innodb_tablespace_monitor;
```

## InnoDB Table Monitor

The InnoDB Table Monitor is deprecated, and may be removed in a future version of MariaDB.

Enabling the Table Monitor outputs the contents of the InnoDB internal data dictionary to the error log every fifteen seconds.

To  enable the Table Monitor, run the following statement:

```sql
CREATE TABLE innodb_table_monitor (a INT) ENGINE=INNODB;
```

To disable it, drop the table:

```sql
DROP TABLE innodb_table_monitor;
```

## SHOW ENGINE INNODB STATUS

The [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status/) statement can be used to obtain the standard InnoDB Monitor output when required, rather than sending it to the error log. It will also display the InnoDB Lock Monitor information if the [innodb_status_output_locks](/kb/en/xtradbinnodb-server-system-variables/#innodb_status_output_locks) system variable is set to `1`.