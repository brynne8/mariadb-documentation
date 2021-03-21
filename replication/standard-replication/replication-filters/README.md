# Replication Filters

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

Replication filters allow users to configure [replication slaves](/replication/standard-replication/replication-overview) to intentionally skip certain events.

## Binary Log Filters for Replication Masters

MariaDB provides options that can be used on a [replication master](/replication) to restrict local changes to specific databases from getting written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log), which also determines whether any replication slaves replicate those changes.

### Binary Log Filter Options

The following options are available, and they are evaluated in the order that they are listed below:

#### `binlog_do_db`

The  [binlog_do_db](/kb/en/mysqld-options/#-binlog-do-db) option allows you to configure a [replication master](/replication) to write statements and transactions affecting databases that match a specified name into its [binary log](/mariadb-administration/server-monitoring-logs/binary-log). Since the filtered statements or transactions will not be present in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log), its replication slaves will not be able to replicate them.

This option will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](#statement-based-logging) section for more information.

This option can <strong>not</strong> be set dynamically.

When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), the option does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the option multiple times. For example:

```sql
[mariadb]
...
binlog_do_db=db1
binlog_do_db=db2
```

This will tell the master to do the following:

- Write statements and transactions affecting the database named <em>db1</em> into the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).
- Write statements and transactions affecting the database named <em>db2</em> into the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).
- Don't write statements and transactions affecting any other databases into the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).

#### `binlog_ignore_db`

The [binlog_ignore_db](/kb/en/mysqld-options/#-binlog-ignore-db) option allows you to configure a [replication master](/replication) to <strong>not</strong> write statements and transactions affecting databases that match a specified name into its [binary log](/mariadb-administration/server-monitoring-logs/binary-log). Since the filtered statements or transactions will not be present in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log), its replication slaves will not be able to replicate them.

This option will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](#statement-based-logging) section for more information.

This option can <strong>not</strong> be set dynamically.

When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), the option does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the option multiple times. For example:

```sql
[mariadb]
...
binlog_ignore_db=db1
binlog_ignore_db=db2
```

This will tell the master to do the following:

- Don't write statements and transactions affecting the database named <em>db1</em> into the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).
- Don't write statements and transactions affecting the database named <em>db2</em> into the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).
- Write statements and transactions affecting any other databases into the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).

The [binlog_ignore_db](#binlog_ignore_db) option is effectively ignored if the [binlog_do_db](#binlog_do_db) option is set, so those two options should not be set together.

## Replication Filters for Replication Slaves

MariaDB provides options and system variables that can be used on used on a [replication slave](/replication) to filter events replicated in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log).

### Replication Filter Options

The following options and system variables are available, and they are evaluated in the order that they are listed below:

#### `replicate_rewrite_db`

The [replicate_rewrite_db](/kb/en/mysqld-options/#-replicate-rewrite-db) option allows you to configure a [replication slave](/replication) to rewrite database names. It uses the format `master_database-&gt;slave_database`. If a slave encounters a [binary log](/mariadb-administration/server-monitoring-logs/binary-log) event in which the default database (i.e. the one selected by the [USE](/sql-statements-structure/sql-statements/administrative-sql-statements/use) statement) is `master_database`, then the slave will apply the event in `slave_database` instead.

This option will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](#statement-based-logging) section for more information.

This option only affects statements that involve tables. This option does not affect statements involving the database itself, such as [CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database), [ALTER DATABASE](/sql-statements-structure/sql-statements/data-definition/alter/alter-database), and [DROP DATABASE](/sql-statements-structure/sql-statements/data-definition/drop/drop-database).

This option's rewrites are evaluated <em>before</em> any other replication filters configured by the `replicate_*` system variables.

Statements that use table names qualified with database names do not work with other replication filters such as [replicate_do_table](#replicate_do_table).

This option can <strong>not</strong> be set dynamically.

When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), the option does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the option multiple times. For example:

```sql
[mariadb]
...
replicate_rewrite_db=db1->db3
replicate_rewrite_db=db2->db4
```

This will tell the slave to do the following:

- If a [binary log](/mariadb-administration/server-monitoring-logs/binary-log) event is encountered in which the default database was <em>db1</em>, then apply the event in <em>db3</em> instead.
- If a [binary log](/mariadb-administration/server-monitoring-logs/binary-log) event is encountered in which the default database was <em>db2</em>, then apply the event in <em>db4</em> instead.

See [Configuring Replication Filter Options with Multi-Source Replication](#configuring-replication-filter-options-with-multi-source-replication) for how to configure this system variable with [multi-source replication](/replication/standard-replication/multi-source-replication).

#### `replicate_do_db`

The [replicate_do_db](/kb/en/replication-and-binary-log-server-system-variables/#replicate_do_db) system variable allows you to configure a [replication slave](/replication) to apply statements and transactions affecting databases that match a specified name.

This system variable will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging) or when using [mixed-based logging](/kb/en/binary-log-formats/#statement-based-logging) and the statement is logged statement based. For statement-based replication, only the default database (that is, the one selected by USE) is considered, not any explicitly mentioned tables in the query.
See the [Statement-Based Logging](#statement-based-logging) section for more information.

When setting it dynamically with [SET GLOBAL](/kb/en/set/#global-session), the system variable accepts a comma-separated list of filters.

When setting it dynamically, it is not possible to specify database names that contain commas. If you need to specify database names that contain commas, then you will need to specify them by either providing the command-line options or configuring them in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) when the server is [started](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

When setting it dynamically, the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) must be stopped. For example:

```sql
STOP SLAVE;
SET GLOBAL replicate_do_db='db1,db2';
START SLAVE;
```

When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), the system variable does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the system variable multiple times. For example:

```sql
[mariadb]
...
replicate_do_db=db1
replicate_do_db=db2
```

This will tell the slave to do the following:

- Replicate statements and transactions affecting the database named <em>db1</em>.
- Replicate statements and transactions affecting the database named <em>db2</em>.
- Ignore statements and transactions affecting any other databases.

See [Configuring Replication Filter Options with Multi-Source Replication](#configuring-replication-filter-options-with-multi-source-replication) for how to configure this system variable with [multi-source replication](/replication/standard-replication/multi-source-replication).

#### `replicate_ignore_db`

The [replicate_ignore_db](/kb/en/replication-and-binary-log-server-system-variables/#replicate_ignore_db) system variable allows you to configure a [replication slave](/replication) to ignore statements and transactions affecting databases that match a specified name.

This system variable will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging) or when using [mixed-based logging](/kb/en/binary-log-formats/#statement-based-logging) and the statement is logged statement based. For statement-based replication, only the default database (that is, the one selected by USE) is considered, not any explicitly mentioned tables in the query.
See the [Statement-Based Logging](#statement-based-logging) section for more information.

When setting it dynamically with [SET GLOBAL](/kb/en/set/#global-session), the system variable accepts a comma-separated list of filters.

When setting it dynamically, it is not possible to specify database names that contain commas. If you need to specify names or patterns that contain commas, then you will need to specify them by either providing the command-line options or configuring them in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) when the server is [started](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

When setting it dynamically, the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) must be stopped. For example:

```sql
STOP SLAVE;
SET GLOBAL replicate_ignore_db='db1,db2';
START SLAVE;
```

When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), the system variable does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the system variable multiple times. For example:

```sql
[mariadb]
...
replicate_ignore_db=db1
replicate_ignore_db=db2
```

This will tell the slave to do the following:

- Ignore statements and transactions affecting databases named <em>db1</em>.
- Ignore statements and transactions affecting databases named <em>db2</em>.
- Replicate statements and transactions affecting any other databases.

The [replicate_ignore_db](#replicate_ignore_db) system variable is effectively ignored if the [replicate_do_db](#replicate_do_db) system variable is set, so those two system variables should not be set together.

See [Configuring Replication Filter Options with Multi-Source Replication](#configuring-replication-filter-options-with-multi-source-replication) for how to configure this system variable with [multi-source replication](/replication/standard-replication/multi-source-replication).

#### `replicate_do_table`

The [replicate_do_table](/kb/en/replication-and-binary-log-server-system-variables/#replicate_do_table) system variable allows you to configure a [replication slave](/replication) to apply statements and transactions that affect tables that match a specified name. The table name is specified in the format: `dbname.tablename`.

This system variable will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](#statement-based-logging) section for more information.

This option only affects statements that involve tables. This option does not affect statements involving the database itself, such as [CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database), [ALTER DATABASE](/sql-statements-structure/sql-statements/data-definition/alter/alter-database), and [DROP DATABASE](/sql-statements-structure/sql-statements/data-definition/drop/drop-database).

When setting it dynamically with [SET GLOBAL](/kb/en/set/#global-session), the system variable accepts a comma-separated list of filters.

When setting it dynamically, it is not possible to specify database or table names
or patterns that contain commas. If you need to specify database or table names that contain commas, then you will need to specify them by either providing the command-line options or configuring them in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) when the server is [started](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

When setting it dynamically, the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) must be stopped. For example:

```sql
STOP SLAVE;
SET GLOBAL replicate_do_table='db1.tab,db2.tab';
START SLAVE;
```

When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), the system variable does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the system variable multiple times. For example:

```sql
[mariadb]
...
replicate_do_table=db1.tab
replicate_do_table=db2.tab
```

This will tell the slave to do the following:

- Replicate statements and transactions affecting tables in databases named <em>db1</em> and which are named <em>tab</em>.
- Replicate statements and transactions affecting tables in databases named <em>db2</em> and which are named <em>tab</em>.
- Ignore statements and transactions affecting any other tables.

See [Configuring Replication Filter Options with Multi-Source Replication](#configuring-replication-filter-options-with-multi-source-replication) for how to configure this system variable with [multi-source replication](/replication/standard-replication/multi-source-replication).

#### `replicate_ignore_table`

The [replicate_ignore_table](/kb/en/replication-and-binary-log-server-system-variables/#replicate_ignore_table) system variable allows you to configure a [replication slave](/replication) to ignore statements and transactions that affect tables that match a specified name. The table name is specified in the format: `dbname.tablename`.

This system variable will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](#statement-based-logging) section for more information.

When setting it dynamically with [SET GLOBAL](/kb/en/set/#global-session), the system variable accepts a comma-separated list of filters.

When setting it dynamically, it is not possible to specify database or table names that contain commas. If you need to specify database or table names that contain commas, then you will need to specify them by either providing the command-line options or configuring them in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) when the server is [started](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

When setting it dynamically, the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) must be stopped. For example:

```sql
STOP SLAVE;
SET GLOBAL replicate_ignore_table='db1.tab,db2.tab';
START SLAVE;
```

When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), the system variable does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the system variable multiple times. For example:

```sql
[mariadb]
...
replicate_ignore_table=db1.tab
replicate_ignore_table=db2.tab
```

This will tell the slave to do the following:

- Ignore statements and transactions affecting tables in databases named <em>db1</em> and which are named <em>tab</em>.
- Ignore statements and transactions affecting tables in databases named <em>db2</em> and which are named <em>tab</em>.
- Replicate statements and transactions affecting any other tables.

The [replicate_ignore_table](#replicate_ignore_table) system variable is effectively ignored if either the [replicate_do_table](#replicate_do_table) system variable or the [replicate_wild_do_table](#replicate_wild_do_table) system variable is set, so the [replicate_ignore_table](#replicate_ignore_table) system variable should not be used with those two system variables.

See [Configuring Replication Filter Options with Multi-Source Replication](#configuring-replication-filter-options-with-multi-source-replication) for how to configure this system variable with [multi-source replication](/replication/standard-replication/multi-source-replication).

#### `replicate_wild_do_table`

The [replicate_wild_do_table](/kb/en/replication-and-binary-log-server-system-variables/#replicate_wild_do_table) system variable allows you to configure a [replication slave](/replication) to apply statements and transactions that affect tables that match a specified wildcard pattern.

The wildcard pattern uses the same semantics as the [LIKE](/built-in-functions/string-functions/like) operator. This means that the the following characters have a special meaning:

- `_` - The `_` character matches any single character.
- `%` - The `%` character matches zero or more characters.
- `\` - The `\` character is used to escape the other special characters in cases where you need the literal character.

This system variable will work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](#statement-based-logging) section for more information.

The system variable does filter databases, tables, [views](/programming-customizing-mariadb/views) and [triggers](/programming-customizing-mariadb/triggers-events/triggers).

The system variable does not filter [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures), [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions), and [events](/kb/en/stored-programs-and-views-events/). The [replicate_do_db](/kb/en/replication-and-binary-log-server-system-variables/#replicate_do_db) system variable will need to be used to filter those.

If the table name pattern for a filter is just specified as `%`, then all tables in the database will be matched. In this case, the filter will also affect certain database-level statements, such as [CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database), [ALTER DATABASE](/sql-statements-structure/sql-statements/data-definition/alter/alter-database) and [DROP DATABASE](/sql-statements-structure/sql-statements/data-definition/drop/drop-database).

When setting it dynamically with [SET GLOBAL](/kb/en/set/#global-session), the system variable accepts a comma-separated list of filters.

When setting it dynamically, it is not possible to specify database or table names or patterns that contain commas. If you need to specify database or table names or patterns that contain commas, then you will need to specify them by either providing the command-line options or configuring them in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) when the server is [started](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

When setting it dynamically, the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) must be stopped. For example:

```sql
STOP SLAVE;
SET GLOBAL replicate_wild_do_table='db%.tab%,app1.%';
START SLAVE;
```

When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), the system variable does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the system variable multiple times. For example:

```sql
[mariadb]
...
replicate_wild_do_table=db%.tab%
replicate_wild_do_table=app1.%
```

This will tell the slave to do the following:

- Replicate statements and transactions affecting tables in databases that start with <em>db</em> and whose table names start with <em>tab</em>.
- Replicate statements and transactions affecting the database named <em>app1</em>.
- Ignore statements and transactions affecting any other tables and databases.

See [Configuring Replication Filter Options with Multi-Source Replication](#configuring-replication-filter-options-with-multi-source-replication) for how to configure this system variable with [multi-source replication](/replication/standard-replication/multi-source-replication).

#### `replicate_wild_ignore_table`

The [replicate_wild_ignore_table](/kb/en/replication-and-binary-log-server-system-variables/#replicate_wild_ignore_table) system variable allows you to configure a [replication slave](/replication) to ignore statements and transactions that affect tables that match a specified wildcard pattern.

The wildcard pattern uses the same semantics as the [LIKE](/built-in-functions/string-functions/like) operator. This means that the the following characters have a special meaning:

- `_` - The `_` character matches any single character.
- `%` - The `%` character matches zero or more characters.
- `\` - The `\` character is used to escape the other special characters in cases where you need the literal character.

This system variable will work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](#statement-based-logging) section for more information.

The system variable does filter databases, tables, [views](/programming-customizing-mariadb/views) and [triggers](/programming-customizing-mariadb/triggers-events/triggers).

The system variable does not filter [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures), [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions), and [events](/kb/en/stored-programs-and-views-events/). The [replicate_ignore_db](/kb/en/replication-and-binary-log-server-system-variables/#replicate_ignore_db) system variable will need to be used to filter those.

If the table name pattern for a filter is just specified as `%`, then all tables in the database will be matched. In this case, the filter will also affect certain database-level statements, such as [CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database), [ALTER DATABASE](/sql-statements-structure/sql-statements/data-definition/alter/alter-database) and [DROP DATABASE](/sql-statements-structure/sql-statements/data-definition/drop/drop-database).

When setting it dynamically with [SET GLOBAL](/kb/en/set/#global-session), the system variable accepts a comma-separated list of filters.

When setting it dynamically, it is not possible to specify database or table names or patterns that contain commas. If you need to specify database or table names or patterns that contain commas, then you will need to specify them by either providing the command-line options or configuring them in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) when the server is [started](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

When setting it dynamically, the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) must be stopped. For example:

```sql
STOP SLAVE;
SET GLOBAL replicate_wild_ignore_table='db%.tab%,app1.%';
START SLAVE;
```

When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), the system variable does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the system variable multiple times. For example:

```sql
[mariadb]
...
replicate_wild_ignore_table=db%.tab%
replicate_wild_ignore_table=app1.%
```

This will tell the slave to do the following:

- Ignore statements and transactions affecting tables in databases that start with <em>db</em> and whose table names start with <em>tab</em>.
- Ignore statements and transactions affecting the database named <em>app1</em>.
- Replicate statements and transactions affecting any other tables and databases.

The [replicate_ignore_table](#replicate_wild_ignore_table) system variable is effectively ignored if either the [replicate_do_table](#replicate_do_table) system variable or the [replicate_wild_do_table](#replicate_wild_do_table) system variable is set, so the [replicate_ignore_table](#replicate_wild_ignore_table) system variable should not be used with those two system variables.

See [Configuring Replication Filter Options with Multi-Source Replication](#configuring-replication-filter-options-with-multi-source-replication) for how to configure this system variable with [multi-source replication](/replication/standard-replication/multi-source-replication).

#### Configuring Replication Filter Options with Multi-Source Replication

How you configure replication filters with [multi-source replication](/replication/standard-replication/multi-source-replication) depends on whether you are configuring them dynamically or whether you are configuring them in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

##### Setting Replication Filter Options Dynamically with Multi-Source Replication

The usage of dynamic replication filters changes somewhat when [multi-source replication](/replication/standard-replication/multi-source-replication) is in use. By default, the variables are addressed to the default connection, so in a multi-source environment, the required connection needs to be specified. There are two ways to do this.

###### Prefixing the Replication Filter Option with the Connection Name

One way to change a replication filter for a multi-source connection is to explicitly specify the name when changing the filter. For example:

```sql
STOP SLAVE 'gandalf';
SET GLOBAL gandalf.replicate_do_table='database1.table1,database1.table2,database1.table3';
START SLAVE 'gandalf';
```

###### Changing the Default Connection

Alternatively, the default connection can be changed by setting the [default_master_connection](/kb/en/replication-and-binary-log-server-system-variables/#default_master_connection) system variable, and then the replication filter can be changed in the usual fashion. For example:

```sql
SET default_master_connection = 'gandalf';
STOP SLAVE; 
SET GLOBAL replicate_do_table='database1.table1,database1.table2,database1.table3';
START SLAVE;
```

##### Setting Replication Filter Options in Option Files with Multi-Source Replication

If you are using [multi-source replication](/replication/standard-replication/multi-source-replication) and if you would like to make this filter persist server restarts by adding it to a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), then the option file can also include the connection name that each filter would apply to. For example:

```sql
[mariadb]
...
gandalf.replicate_do_db=database1
saruman.replicate_do_db=database2
```

### CHANGE MASTER Options

The [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) statement has a few options that can be used to filter certain types of [binary log](/mariadb-administration/server-monitoring-logs/binary-log) events.

#### `IGNORE_SERVER_IDS`

The [IGNORE_SERVER_IDS](/kb/en/change-master-to/#ignore_server_ids) option for `CHANGE MASTER` can be used to configure a [replication slave](/replication) to ignore [binary log](binary_log) events that originated from certain servers. Filtered [binary log](binary_log) events will not get logged to the slave’s [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log), and they will not be applied by the slave.

#### `DO_DOMAIN_IDS`

The [DO_DOMAIN_IDS](/kb/en/change-master-to/#do_domain_ids) option for `CHANGE MASTER` can be used to configure a [replication slave](/replication) to only apply [binary log](binary_log) events if the transaction's [GTID](/kb/en/global-transaction-id/) is in a specific [gtid_domain_id](/kb/en/gtid/#gtid_domain_id) value. Filtered [binary log](binary_log) events will not get logged to the slave’s [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log), and they will not be applied by the slave.

#### `IGNORE_DOMAIN_IDS`

The [IGNORE_DOMAIN_IDS](/kb/en/change-master-to/#ignore_domain_ids) option for `CHANGE MASTER` can be used to configure a [replication slave](/replication) to ignore [binary log](binary_log) events if the transaction's [GTID](/kb/en/global-transaction-id/) is in a specific [gtid_domain_id](/kb/en/gtid/#gtid_domain_id) value. Filtered [binary log](binary_log) events will not get logged to the slave’s [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log), and they will not be applied by the slave.

## Replication Filters and Binary Log Formats

The way that a replication filter is interpreted can depend on the [binary log format](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats).

### Statement-Based Logging

When an event is logged in its statement-based format, many replication filters that affect a database will test the filter against the default database (i.e. the one selected by the [USE](/sql-statements-structure/sql-statements/administrative-sql-statements/use) statement). This applies to the following replication filters:

- [binlog_do_db](#binlog_do_db)
- [binlog_ignore_db](#binlog_ignore_db)
- [replicate_rewrite_db](#replicate_rewrite_db)
- [replicate_do_db](#replicate_do_db)
- [replicate_ignore_db](#replicate_ignore_db)

When an event is logged in its statement-based format, many replication filters that affect a table will test the filter against the table in the default database (i.e. the one selected by the [USE](/sql-statements-structure/sql-statements/administrative-sql-statements/use) statement). This applies to the following replication filters:

- [replicate_do_table](#replicate_do_table)
- [replicate_ignore_table](#replicate_ignore_table)

This means that cross-database updates <strong>not</strong> work with replication filters and statement-based binary logging. For example, if [replicate_do_table=db2.tab](replicate_do_table%3Ddb2.tab) were set, then the following would not replicate with statement-based binary logging:

```sql
USE db1;
INSERT INTO db2.tab VALUES (1);
```

If you need to be able to support cross-database updates with replication filters and statement-based binary logging, then you should use the following replication filters:

- [replicate_wild_do_table](#replicate_wild_do_table)
- [replicate_wild_ignore_table](#replicate_wild_ignore_table)

### Row-Based Logging

When an event is logged in its row-based format, many replication filters that affect a database will test the filter against the database that is actually affected by the event.

Similarly, when an event is logged in its row-based format, many replication filters that affect a table will test the filter against the table in the the database that is actually affected by the event.

This means that cross-database updates work with replication filters and statement-based binary logging.

Keep in mind that DDL statements are always logged to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) in statement-based format, even when the [binlog_format](/kb/en/replication-and-binary-log-system-variables/#binlog_format) system variable is set to `ROW`. This means that the notes mentioned in [Statement-Based Logging](#statement-based-logging) always apply to DDL.

## Replication Filters and Galera Cluster

When using Galera cluster, replication filters should be used with caution. See [Configuring MariaDB Galera Cluster: Replication Filters](/kb/en/configuring-mariadb-galera-cluster/#replication-filters) for more details.

## See Also

- [Dynamic replication filters — our wheel will be square!](https://mariadb.org/dynamic-replication-filters-our-wheel-will-be-square/)