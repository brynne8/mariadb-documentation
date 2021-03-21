# Understanding MariaDB Architecture

MariaDB architecture is partly different from the architecture of traditional DBMSs, like SQL Server. Here we will examine the main components that a new MariaDB DBA needs to know. We will also discuss a bit of history, because this may help understand MariaDB philosophy and certain design choices.

This section is an overview of the most important components. More information is included in specific sections of this migration guide, or in other pages of the MariaDB Knowledge Base (see the links scattered over the text).

## Storage Engines

MariaDB was born from the source code of MySQL, in 2008. Therefore, its history begins with MySQL.

MySQL was born at the beginning of the 90s. Back in the days, if compared to its existing competitors, MySQL was lightweight, simple to install, easy to learn. While it had a very limited set of features, it was also fast in certain common operations. And it was open source. These characteristics made it suitable to back the simple websites that existed at that time.

The web evolved rapidly, and the same happened to MySQL. Being open source helped a lot in this respect, because the community needed functionalities that weren’t supported at that time.

MySQL was probably the first database system to support a [pluggable storage engine architecture](/columns-storage-engines-and-plugins/storage-engines). Basically, this means that MySQL knows very little about creating or populating a table, reading from it, building proper indexes and caches. It just delegated all these operations to a special plugin type called a storage engine.

One of the first plugins developed by third parties was [InnoDB](/kb/en/understanding-mariadb-architecture/#innodb). It is very fast, and it adds two important features that are not otherwise supported: transactions and [foreign keys](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys).

Note that when MariaDB asks a storage engine to write or read a row, the storage engine could theoretically do anything. This led to the creation of very interesting alternative engines, like [BLACKHOLE](/columns-storage-engines-and-plugins/storage-engines/blackhole) (which doesn’t write or read any data, acting like the /dev/null file in Linux), or [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect) (which can read and write to files written in many different formats, or remote DBMSs, or some other special data sources).

Nowadays InnoDB is the default MariaDB storage engine, and it is the best choice for most use cases. But for particular needs, sometimes using a different storage engine is desirable. In case of doubts about the best storage engine to use for a specific case, check the [Choosing the Right Storage Engine](/columns-storage-engines-and-plugins/storage-engines/choosing-the-right-storage-engine) page.

When we create a table, we specify its storage engine or use the default one. It is possible to convert an existing table to another storage engine, though this is a blocking operation which requires a complete table copy. Third-party storage engines can also be installed while MariaDB is running.

Note that it is perfectly possible to use tables with different storage engines in the same transaction (even if some engines are not transactional). It is even possible to use different engines in the same query, for example with JOINs and subqueries.

The default storage engine can be changed by changing the [default_storage_engine](/kb/en/server-system-variables/#default_storage_engine) variable. A different default can be specified for temporary tables by setting [default_tmp_storage_engine](/kb/en/server-system-variables/#default_tmp_storage_engine). MariaDB uses [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine) for system tables and temporary tables created internally to store the intermediate results of a query.

### InnoDB

It is worth spending some more words here about [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb), the default storage engine.

#### Primary Key and Indexes

InnoDB primary keys are always the equivalent of SQL Server clustered indexes. In other words, an InnoDB table is always ordered by the primary key.

If an InnoDB table doesn't have a user-defined primary key, the first `UNIQUE` index whose columns are all `NOT NULL` is used as a primary key. If there is no such index, the table will have a <em>clustered index</em>. The terminology here can be a bit confusing for SQL Server and other DBMS users. A clustered index in InnoDB is a 6 bytes value that is added to the table. This index and its values are completely invisible to the users. It's important to note that clustered indexes are governed by a global mutex that greatly reduces their scalability.

Secondary indexes are ordered by the columns that are part of the index, and contain a reference to each entry's corresponding primary key value.

Some consequences of these design choices are the following:

- For performance reasons, a primary key value should be inserted in order. In other words, the last inserted value should be the highest. This order is normally followed when inserting values into an `AUTO_INCREMENT` primary key. The reason is that inserting values in the middle of an ordered data structure is slower, unless they fit into existing holes. If we insert primary key values randomly, InnoDB often has to rearrange pages to make some room for the new data.
- A big primary keys means that all secondary indexes are also big.
- A query by primary key will require a single search. A query on a secondary index that also reads columns not contained in the index will require one search on the index, plus one more search for each row that satisfies the index condition.
- We shouldn't explicitly include the primary key in a secondary index. If we do so, the primary key column will be duplicated in the index.

#### Tablespaces

For InnoDB, a <em>tablespace</em> is a file containing data (not a file group as in SQL Server). The types of tablespaces are:

- [System tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces).
- [File-per-table tablespaces](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces).
- [Temporary tablespaces](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-temporary-tablespaces).

The system tablespace is stored in the file `ibdata`. It contains information used by InnoDB internally, like rollback segments, as well as some system tables. Historically, the system tablespace also contained all tables created by the user. In modern MariaDB versions, a table is created in the system tablespace only if the [innodb_file_per_table](/kb/en/innodb-system-variables/#innodb_file_per_table) system variable is set to 0 at the moment of the table creation. By default, innodb_file_per_table is 1.

Tables created while `innodb_file_per_table=1` are written into their own tablespace. These are `.ibd` files.

Starting from [MariaDB 10.2](/kb/en/what-is-mariadb-102/), temporary tables are written into temporary tablespaces, which means `ibtmp*` files. Previously, they were created in the system tablespace or in file-per-table tablespaces according to the value of `innodb_file_per_table`, just like regular tables. Temporary tablespaces, if present, are deleted when MariaDB starts.

<strong>It is important to remember that tablespaces can never shrink</strong>. If a file-per-table tablespace grows too much, deleting data won't recover space. Instead, a new table must be created and data needs to be copied. Finally, the old table will be deleted. If the system tablespace grows too much, the only solution is to move data into a new MariaDB installation.

#### Transaction Logs

In SQL Server, the transaction log contains both the undo log and the redo log. Usually we have only one transaction log.

In MariaDB the undo log and the redo log are stored separately. By default, the [redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log) is written to two files, called `ib_logfile0` and `ib_logfile1`. The [undo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log) by default is written to the <em>system tablespace</em>, which is in the `ibdata1` file. However, it is possible to write it in separate files in a specified directory.

MariaDB provides no way to inspect the contents of the transaction logs. However, it is possible to inspect the [binary log](/kb/en/understanding-mariadb-architecture/#the-binary-log).

InnoDB transaction logs are written in a circular fashion: their size is normally fixed, and when the end is reached, InnoDB continues to write from the beginning. However, if very long transactions are running, InnoDB cannot overwrite the oldest data, so it has to expand the log size instead.

#### InnoDB Buffer Pool

MariaDB doesn't have a central buffer pool. Each storage engine may or may not have a buffer pool. The [InnoDB buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool) is typically assigned a big amount of memory. See [MariaDB Memory Allocation](/replication/optimization-and-tuning/mariadb-memory-allocation).

MariaDB has no extension like the SQL Server buffer pool extension.

A part of the buffer pool is called the [change buffer](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-change-buffering). It contains dirty pages that have been modified in memory and not yet flushed.

#### InnoDB Background Threads

InnoDB has background threads that take care of flushing dirty pages from the change buffer to the tablespaces. They don't directly affect the latency of queries, but they are very important for performance.

[SHOW ENGINE InnoDB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status) shows information about them in the `BACKGROUND THREAD` section. They can also be seen using the [threads](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table) table, in the [performance_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema).

InnoDB flushing is similar to <em>lazy writes</em> and <em>checkpoints</em> in SQL Server. It has no equivalent for <em>eager writing</em>.

For more information, see [InnoDB Page Flushing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-flushing) and [InnoDB Purge](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-purge).

#### Checksums and Doublewrite Buffer

InnoDB pages have checksums. After writing pages to disk, InnoDB verifies that the checksums match. The checksum algorithm is determined by [innodb_checksum_algorithm](/kb/en/innodb-system-variables/#innodb_checksum_algorithm). Check the variable documentation for its consequences on performance, backward compatibility and encryption.

In case of a system crash, hardware failure or power outage, a page could be half-written on disk. For some pages, this causes a disaster. Therefore, InnoDB writes essential pages to disk twice. A backup copy of the new page version is written first. Then, the old page is overwritten. The backup copies are written into a file called the <em>doublewrite buffer</em>.

- If an event prevents the first page from being written, the old version of the page will still be available.
- If an event prevents the old page from being completely overwritten by its new version, the page can still be recovered using the doublewrite buffer.

The doublewrite buffer can disabled using the [innodb_doublewrite](/kb/en/innodb-system-variables/#innodb_doublewrite) variable, but this usually doesn't bring big performance benefits. The doublewrite buffer location can be changed with [innodb_doublewrite_file](/kb/en/innodb-system-variables/#innodb_doublewrite_file).

### Aria

Even if we only create InnoDB tables, we use Aria indirectly, in two ways:

- For system tables.
- For internal temporary tables.

Aria is a non-transactional storage engine. By default it is crash-safe, meaning that all changes to data are written and fsynced to a write-ahead log and can always be recovered in case of a crash.

Aria caches indexes into the pagecache. Data are not directly cached by Aria, so it's important that the underlying filesystem caches reads and writes.

The pagecache size is determined by the [aria_pagecache_buffer_size](/kb/en/aria-system-variables/#aria_pagecache_buffer_size) system variable. To know if it is big enough we can check the proportion of free pages (the ratio between [Aria_pagecache_blocks_used](/kb/en/aria-status-variables/#aria_pagecache_blocks_used) and [Aria_pagecache_blocks_unused](/kb/en/aria-status-variables/#aria_pagecache_blocks_unused)) and the proportion of cache misses (the ratio between [Aria_pagecache_read_requests](/kb/en/aria-status-variables/#aria_pagecache_read_requests) and [Aria_pagecache_reads](/kb/en/aria-status-variables/#aria_pagecache_reads).

The proportion of dirty pages is the ratio between [Aria_pagecache_blocks_used](/kb/en/aria-status-variables/#aria_pagecache_blocks_used) and [Aria_pagecache_blocks_not_flushed](/kb/en/aria-status-variables/#aria_pagecache_blocks_not_flushed) tells us if the log file is big enough.

The size of Aria log is determined by [aria_log_file_size](/kb/en/aria-system-variables/#aria_pagecache_buffer_size).

## Databases

MariaDB does not support the concept of schema. In MariaDB SQL, <em>schema</em> and <em>schemas</em> are synonyms for <em>database</em> and <em>databases</em>.

When a user connects to MariaDB, they don't connect to a specific database. Instead, they can access any table they have permissions for. There is however a concept of <em>default database</em>, see below.

A database is a container for database objects like tables and views. A database serves the following purposes:

- A database is a namespace.
- A database is a logical container to separate objects.
- A database has a default [character set](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets) and collation, which are inherited by their tables.
- Permissions can be assigned on a whole database, to make permission maintenance simpler.
- Physical data files are stored in a directory which has the same name as the database to which they belong.

### System Databases

MariaDB has the following system databases:

- [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) is for internal use only, and should not be read or written directly.
- [information_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema) contains all information that can be found in SQL Server's information_schema and more. However, while SQL Server's `information_schema` is a schema containing information about the local database, MariaDB's `information_schema` is a database that contains information about all databases.
- [performance_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema) contains information about MariaDB runtime. It is disabled by default. Enabling it requires setting the [performance_schema](/kb/en/performance-schema-system-variables/#performance_schema) system variable to 1 and restarting MariaDB.

### Default Database

When a user connects to MariaDB, they can optionally specify a default database. A default database can also be specified or changed later, with the [USE](/sql-statements-structure/sql-statements/administrative-sql-statements/use) command.

Having a default database specified allows one to specify tables without specifying the name of the database where they are located. If no default database is specified, all table names must be fully qualified.

For example, the two following snippets are equivalent:

```sql
SELECT * FROM my_database.my_table;

-- is equivalent to:
USE my_database;
SELECT * FROM my_table;
```

Even if a default database is specified, tables from other databases can be accessed by specifying their fully qualified names:

```sql
-- this query joins my_database.my_table to your_database.your_table
USE my_database;
SELECT m.*
    FROM my_table m
    JOIN your_database.your_table y
        ON m.xyz = y.xyz;
```

MariaDB has the [DATABASE()](/built-in-functions/secondary-functions/information-functions/database) function to determine the current database:

```sql
SELECT DATABASE();
```

Stored procedures and triggers don't inherit a default database from the session, nor by a caller procedure. In that context, the default database is the database which contains the procedure. `USE` can be used to change it. The default database will only be valid for the rest of the procedure.

## The Binary Log

Different tables can be built using different storage engines. It is important to note that not all engines are transactional, and that different engines implement the transaction logs in different ways. For this reason, MariaDB cannot replicate data from a master to a slave using an equivalent of SQL Server transactional replication.

Instead, it needs a global mechanism to log the changes that are applied to data. This mechanism is the [binary log](/mariadb-administration/server-monitoring-logs/binary-log), often abbreviated to binlog.

The binary log can be written in the following formats:

- STATEMENT logs SQL statements that modify data;
- ROW logs a reference to the rows that have been modified, if any (usually it’s the primary key), and the new values that have been added or modified, in a binary format.
- MIXED is a combination of the above formats. It means that ROW is used for statements that can safely be logged in this way (see below), and STATEMENT is used in other cases. This is the default format from [MariaDB 10.2](/kb/en/what-is-mariadb-102/).

In most cases, STATEMENT is slower because the SQL statement needs to be re-executed by the slave, and because certain statements may produce a different result in the slave (think about queries that use LIMIT without ORDER BY, or the CURRENT_TIMESTAMP() function). But there are exceptions, and besides, DDL statements are always logged as STATEMENT to avoid flooding the binary log. Therefore, the binary log may well contain both ROW and STATEMENT entries.

See [Binary Log Formats](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats).

The binary log allows:

- replication, if enabled on the master;
- promoting a slave to a master, if enabled on that slave;
- incremental backups;
- seeing data as they were in a point of time in the past ([flashback](/mariadb-administration/server-monitoring-logs/binary-log/flashback));
- restoring a backup and re-appling the binary log, with the exception of a data change which caused problems (human mistake, application bug, SQL injection);
- Capture Data Changes (CDC), by streaming the binary log to technologies like Apache Kafka.

If you don't plan to use any of these features on a server, it is possible to [disable](/kb/en/replication-and-binary-log-system-variables/#log_bin) the binary log to slightly improve the performance.

The binary log can be inspected using the [mysqlbinlog](/clients-utilities/mysqlbinlog) utility, which comes with MariaDB. Enabling or disabling the binary log requires restarting MariaDB.

See also [MariaDB Replication Overview for SQL Server Users](/mariadb-administration/getting-installing-and-upgrading-mariadb/migrating-from-sql-server-to-mariadb/mariadb-replication-overview-for-sql-server-users) and [MariaDB Backups Overview for SQL Server Users](/mariadb-administration/getting-installing-and-upgrading-mariadb/migrating-from-sql-server-to-mariadb/mariadb-backups-overview-for-sql-server-users) for a better understanding of how the binary log is used.

## Plugins

Storage engines are a special type of [plugin](/columns-storage-engines-and-plugins/plugins). But others exist. For example, plugins can add authentication methods, new features, SQL syntax, functions, informative tables, and more.

A plugin may add some server variables and some status variables. Server variables can be used to configure the plugin, and status variables can be used to monitor its activities and status. These variables generally use the plugin's name as a prefix. For example InnoDB has a server variable called innodb_buffer_pool_size to configure the size of its buffer pool, and a status variable called Innodb_pages_read which indicates the number of memory pages read from the buffer pool. The category [system variables](/replication/optimization-and-tuning/system-variables) of the MariaDB Knowledge Base has specific pages for system and status variables associated with various plugins.

Many plugins are installed by default, or available but not installed by default. They can be installed or uninstalled at runtime with SQL statements, like `INSTALL PLUGIN`, `UNINSTALL PLUGIN` and others; see [Plugin SQL Statements](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements). 3rd party plugins can be made available for installation by simply copying them to the [plugin_dir](/kb/en/server-system-variables/#plugin_dir).

It is important to note that different plugins may have different maturity levels. It is possible to prevent the installation of plugins we don’t consider production-ready by setting the [plugin_maturity](/kb/en/server-system-variables/#plugin_maturity) system variable. For plugins that are distributed with MariaDB, the maturity level is determined by the MariaDB team based on the bugs reported and fixed.

Some plugins are developed by 3rd parties. Even some 3rd party plugins are included in MariaDB official distributions - the ones available on mariadb.org.

In MariaDB every authorization method (including the default one) is provided by an [authentication plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins). A user can be required to use a certain authentication plugin. This gives us much flexibility and control. Windows users may be interested in [gsapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi) (which supports Windows authentication, Kerberos and NTLM) and [named_pipe](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-named-pipe) (which uses named pipe impersonation).

Other plugins that can be very useful include [userstat](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics), which includes statistics about resources and table usage, and [METADATA_LOCK_INFO](/kb/en/metadata_lock_info/), which provides information about metadata locks.

## Thread Pool

MariaDB supports [thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool). It works differently on UNIX and on Windows. On Windows, it is enabled by default and its implementation is quite similar to SQL Server. It uses the Windows native CreateThreadpool API.

If we don't use the thread pool, MariaDB will use its traditional method to handle connections. It consists of using a dedicated thread for each client connection. Creating a new thread has a cost in terms of CPU time. To mitigate this cost, after a client disconnects, the thread may be preserved for a certain time in the [thread cache](/kb/en/server-system-variables/#thread_cache_size).

Whichever connection method we use, MariaDB has a maximum number of simultaneous connections, which can be changed at runtime. When the limit is reached, if more clients try to connect they will receive an error. This prevents MariaDB from consuming all the server resources and freezing or crashing. See [Handling Too Many Connections](/replication/optimization-and-tuning/system-variables/handling-too-many-connections).

## Configuration

MariaDB has many settings that 
control the server behavior. These can be set up when starting mysqld ([mysqld options](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options)), and the vast majority are also accessible as [server system variables](/replication/optimization-and-tuning/system-variables/server-system-variables). These can be classified in these ways:

- <strong>Dynamic</strong> or <strong>static</strong>;
- <strong>Global</strong>, <strong>session</strong>, or both.

Note that server system variables are not to be confused with [user-defined variables](/sql-statements-structure/sql-language-structure/user-defined-variables). The latter are not used for MariaDB configuration.

### Configuration Files

MariaDB can use several [configuration files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). Configuration files are searched in several locations, including in the user directory, and if present they all are read and used. They are read in a consistent order. These locations depend on the operating system; see [Default Option File Locations](/kb/en/configuring-mariadb-with-option-files/#default-option-file-locations). It is possible to tell MariaDB which files it should read; see [Global Options Related to Option Files](/kb/en/configuring-mariadb-with-option-files/#global-options-related-to-option-files).

On Linux, by default the configuration files are called `my.cnf`. On Windows, by default the configuration files can be called `my.ini` or `my.cnf`. The former is more common.

If a variable is mentioned multiple times in different files, the occurrence that is read last will overwrite the others. Similarly, if a variable is mentioned several times in a single file, the occurrence that is read last overwrites the others.

The contents of each configuration file are organized by <em>option groups</em>. MariaDB Server and client programs read different groups. The read groups also depend on the MariaDB version. See [Option Groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) for the details. Most commonly, the `[server]` or `[mysqld]` groups are used to contain all server configuration. The `[client-server]` group can be used for options that are shared by the server and the clients (like the port to use), to avoid repeating those variables multiple times.

### Dynamic and Static Variables

Dynamic variables have a value that can be changed at runtime, using the [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set) SQL statement. Static variables have a value that is decided at startup (see below) and cannot be changed without a restart.

The [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables) page states if variables are dynamic or static.

### Scope

A global system variable is one that affects the general behavior of MariaDB. For example [innodb_buffer_pool_size](/kb/en/innodb-system-variables/#innodb_buffer_pool_size) determines the size of the InnoDB buffer pool, which is used by read and write operations, no matter which user issued them. A session system variable is one that affects MariaDB behavior for the current connection; changing it will not affect other connected users, or future connections from the current user.

A variable could exist in both the global and session scopes. In this case, the session value is what affects the current connection. When a user connects, the current global value is copied to the session scope. Changing the global value afterward will not change existing connections.

The [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables) page states the scope of each variable.

Global variables and some session variables can only be modified by a user with the [SUPER](/kb/en/grant/#global-privileges) privilege (typically root).

### Syntax

To see the value of a system variable:

```sql
-- global variables:
SELECT @@global.variable_name;
-- session variables:
SELECT @@session.variable_name;
-- or just use the shortcut:
SELECT @@variable_name;
```

A longer syntax, which is mostly useful to get multiple variables, makes use of the same pattern syntax that is used by the [LIKE](/built-in-functions/string-functions/like) operator:

```sql
-- global variables whose name starts with 'innodb':
SHOW GLOBAL VARIABLES LIKE 'innodb%';
-- session variables whose name starts with 'innodb':
SHOW SESSION VARIABLES LIKE 'innodb%';
SHOW VARIABLES LIKE 'innodb%';
```

To modify the global or session value of a dynamic variable:

```sql
SET @@global.variable_name = 'new 'value';
SET @@session.variable_name = 'new 'value';
```

Notice that if we modify a global variable in this way, the new value will be lost at server restart. For this reason we probably want to change the value in the configuration file too.

For further information see:

- The [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set) statement.
- The [SHOW VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables) statement.

### Setting System Variables with Startup Parameters

System variables can be set at server startup without writing their values into a configuration file. This is useful if we want a value to be set once, until we change it or restart MariaDB. Values passed in this way override values written in the configuration files.

The general rule is that every global variable can be passed as an argument of `mysqld` by prefixing its name with `--` and by replacing every occurrence of `_` with `-` in its name.

For example, to pass `bind_address` as a startup argument:

```sql
mysqld --bind-address=127.0.0.1
```

### Debugging Configuration

Mistyping a variable can prevent MariaDB from starting. We cannot set a variable that doesn't exist in the MariaDB version in use. In these cases, an error is written in the [error log](/mariadb-administration/server-monitoring-logs/error-log).

Having several configuration files and configuration groups, as well as being able to pass variables as command-line arguments, brings a lot of flexibility but can sometimes be confusing. When we are unsure about which values will be used, we can run:

```sql
mysqld --print-defaults
```

## Status Variables

MariaDB status variables and some system tables allow external tools to monitor a server, building graphs on how they change over time, and allow the user to inspect what is happening inside the server.

[Status variables](/replication/optimization-and-tuning/system-variables/server-status-variables) cannot be directly modified by the user. Their values indicate how MariaDB is operating. Their scope can be:

- <strong>Global</strong>, meaning that the value is about some MariaDB activity.
- <strong>Session</strong>, meaning that the value measures activities taking place in the current session.

Many status variables exist in both scopes. For example,[Cpu_time](/kb/en/server-status-variables/#cpu_time) at global level indicates how much time the CPU was used by the MariaDB process (including all user sessions and all the background threads). At session level, it indicates how much time the CPU was used by the current session.

The status variables created by a plugin, usually, use the plugin name as a prefix.

The [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status) statement prints the values of the status variables that match a certain pattern.

```sql
-- Show all InnoDB global status variables
SHOW GLOBAL STATUS LIKE 'innodb%';
-- Show all InnoDB session status variables
SHOW SESSION STATUS LIKE 'innodb%';
SHOW STATUS LIKE 'innodb%';
-- Show global variables that contain the "size" substring:
SHOW GLOBAL STATUS LIKE '%size%';
```

Some status variables values are reset when [FLUSH STATUS](/kb/en/flush/#flush-status) is executed. A possible use:

```sql
DELIMITER ||
BEGIN NOT ATOMIC
SET @i = 0;
WHILE @i < 60 DO
    SHOW GLOBAL STATUS LIKE 'Com_select';
    FLUSH STATUS;
    DO SLEEP(1);
    SET @i = @i + 1;
END WHILE;
END ||
```