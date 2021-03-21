# Server System Variables

## About the Server System Variables

MariaDB has many system variables that can be changed to suit your needs.

The full list of server variables are listed in the contents on this page, and most are described on this page, but some are described elsewhere:

- [Aria System Variables](/columns-storage-engines-and-plugins/storage-engines/aria/aria-system-variables/)
- [CONNECT System Variables](/columns-storage-engines-and-plugins/storage-engines/connect/connect-system-variables/)
- [Galera System Variables](/kb/en/mariadb-galera-cluster-configuration-variables/)
- [Global Transaction ID System Variables](/kb/en/gtid/#system-variables-for-global-transaction-id)
- [HandlerSocket Plugin System Variables](/sql-statements-structure/nosql/handlersocket/handlersocket-configuration-options/)
- [InnoDB System Variables](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-system-variables/)
- [Mroonga System Variables](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-system-variables/)
- [MyRocks System Variables](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-system-variables/)
- [MyISAM System Variables](/kb/en/myisam-server-system-variables/)
- [Performance Schema System Variables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-system-variables/)
- [Replication and Binary Log System Variables](/kb/en/replication-and-binary-log-server-system-variables/)
- [S3 Storage Engine System Variables](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/s3-storage-engine-system-variables/)
- [Server_Audit System Variables](/kb/en/server_audit-system-variables/)
- [Spider System Variables](/columns-storage-engines-and-plugins/storage-engines/spider/spider-server-system-variables/)
- [SQL_ERROR_LOG Plugin System Variables](/replication/optimization-and-tuning/system-variables/sql_error_log-plugin-system-variables/)
- [SSL System Variables](/kb/en/ssl-server-system-variables/)
- [Threadpool System Variables](/kb/en/thread-pool-system-and-status-variables/)
- [TokuDB System Variables](/columns-storage-engines-and-plugins/storage-engines/tokudb/tokudb-system-variables/)

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

Most of these can be set with [command line options](/kb/en/mysqld-options-full-list/) and many of them can be changed at runtime.

There are a few ways to see the full list of server system variables:

- While in the mysql client, run:

```sql
SHOW VARIABLES;
```

- See [SHOW VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables/) for instructions on using this command.

- From your shell, run mysqld like so:

```sql
mysqld --verbose --help
```

- View the Information Schema [GLOBAL_VARIABLES](/kb/en/information-schema-global_variables-and-session_variables-tables/), [SESSION_VARIABLES](/kb/en/information-schema-global_variables-and-session_variables-tables/), and [SYSTEM_VARIABLES](/kb/en/information-schema-system_variables-table/) tables.

## Setting Server System Variables

There are several ways to set server system variables:

- Specify them on the command line:

```sql
shell> ./mysqld_safe --aria_group_commit="hard"
```

- Specify them in your my.cnf file (see [Configuring MariaDB with my.cnf](/kb/en/configuring-mariadb-with-mycnf/) for more information):

```sql
aria_group_commit = "hard"
```

- Set them from the mysql client using the [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/) command. Only variables that are dynamic can be set at runtime in this way. Note that variables set in this way will not persist after a restart.

```sql
SET GLOBAL aria_group_commit="hard";
```

By convention, server variables have usually been specified with an underscore in the configuration files, and a dash on the command line.  You can however specify underscores as dashes - they are interchangeable.

Variables that take a numeric size can either be specified in full, or with a suffix for easier readability. Valid suffixes are:

<table><tbody><tr><th>Suffix</th><th>Description</th><th>Value</th></tr>
<tr><td>K</td><td>kilobytes</td><td>1024</td></tr>
<tr><td>M</td><td>megabytes</td><td>1024<sup>2</sup></td></tr>
<tr><td>G</td><td>gigabytes</td><td>1024<sup>3</sup></td></tr>
<tr><td>T</td><td>terabytes</td><td>1024<sup>4</sup> (from <a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a>)</td></tr>
<tr><td>P</td><td>petabytes</td><td>1024<sup>5</sup> (from <a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a>)</td></tr>
<tr><td>E</td><td>exabytes</td><td>1024<sup>6</sup> (from <a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a>)</td></tr>
</tbody></table>

The suffix can be upper or lower-case.

## List of Server System Variables

#### `alter_algorithm`

- <strong>Description:</strong> The implied `ALGORITHM` for [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) if no `ALGORITHM` clause is specified. The deprecated variable [old_alter_table](#old_alter_table) is an alias for this.
<ul start="1"><li>`COPY` corresponds to the pre-MySQL 5.1 approach of creating an intermediate table, copying data one row at a time, and renaming and dropping tables. 
</li><li>`INPLACE` requests that the operation be refused if it cannot be done natively inside a the storage engine. 
</li><li>`DEFAULT` (the default) chooses `INPLACE` if available, and falls back to `COPY`. 
</li><li>`NOCOPY` refuses to copy a table. 
</li><li>`INSTANT` refuses an operation that would involve any other than metadata changes.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--alter-algorithm=default</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `DEFAULT`
- <strong>Valid Values:</strong> `DEFAULT`, `COPY`, `INPLACE`, `NOCOPY`, `INSTANT`
- <strong>Introduced:</strong> [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

#### `analyze_sample_percentage`

- <strong>Description:</strong> Percentage of rows from the table [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) will sample to collect table statistics. Set to 0 to let MariaDB decide what percentage of rows to sample.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--analyze-sample-percentage=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100.000000`
- <strong>Range:</strong> `0` to `100`
- <strong>Introduced:</strong> [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)

#### `autocommit`

- <strong>Description:</strong> If set to 1, the default, all queries are committed immediately. The [LOCK IN SHARE MODE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/lock-in-share-mode/) and [FOR UPDATE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/for-update/) clauses therefore have no effect. If set to 0, they are only committed upon a [COMMIT](/kb/en/transactions-commit-statement/) statement, or rolled back with a [ROLLBACK](/kb/en/rollback-statement/) statement. If autocommit is set to 0, and then changed to 1, all open transactions are immediately committed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--autocommit[=#]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`

---

#### `automatic_sp_privileges`

- <strong>Description:</strong> When set to 1, the default, when a stored routine is created, the creator is automatically granted permission to [ALTER](/programming-customizing-mariadb/stored-routines/stored-procedures/alter-procedure/) (which includes dropping) and to EXECUTE the routine. If set to 0, the creator is not automatically granted these privileges.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--automatic-sp-privileges</code>, <code class="fixed" style="white-space:pre-wrap">--skip-automatic-sp-privileges</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`

---

#### `back_log`

- <strong>Description:</strong> Connections take a small amount of time to start, and this setting determines the number of outstanding connection requests MariaDB can have, or the size of the listen queue for incoming TCP/IP requests. Requests beyond this will be refused. Increase if you expect short bursts of connections. Cannot be set higher than the operating system limit (see the Unix listen() man page). If not set, set to `0`, or the `--autoset-back-log` option is used, will be autoset to the lower of `900` and (50 + [max_connections](#max_connections)/5).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--back-log=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Default Value:</strong> 
<ul start="1"><li>The lower of `900` and (50 + [max_connections](#max_connections)/5)
</li></ul>

---

#### `basedir`

- <strong>Description:</strong> Path to the MariaDB installation directory. Other paths are usually resolved relative to this base directory.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--basedir=path</code> or <code class="fixed" style="white-space:pre-wrap">-b path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> directory name

---

#### `big_tables`

- <strong>Description:</strong> If this system variable is set to 1, then temporary tables will be saved to disk intead of memory.
<ul start="1"><li>This system variable's original intention was to allow result sets that were too big for memory-based temporary tables and to avoid the resulting 'table full' errors. 
</li><li>This system variable is no longer needed, because the server can automatically convert large memory-based temporary tables into disk-based temporary tables when they exceed the value of the <a undefined>tmp_memory_table_size</a> system variable.
</li><li>To prevent memory-based temporary tables from being used at all, set the <a undefined>tmp_memory_table_size</a> system variable to `0`.
</li><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and earlier, <a undefined>sql_big_tables</a> is a synonym.
</li><li>In [MariaDB 10.5](/kb/en/what-is-mariadb-105/), this system variable is deprecated.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--big-tables</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`
- <strong>Deprecated:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `bind_address`

- <strong>Description:</strong> By default, the MariaDB server listens for TCP/IP connections on a network socket bound to a single address, 0.0.0.0. You can specify an alternative when the server starts using this option; either a host name, an IPv4 or an IPv6 address. In Debian and Ubuntu, the default bind_address is 127.0.0.1, which binds the server to listen on localhost only. `bind_address` has always been available as a [mysqld option](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/), from [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/) its also available as a system variable.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--bind-address=addr</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Valid Values:</strong> Host name, IPv4, IPv6
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/) (as a system variable)

---

#### `bulk_insert_buffer_size`

- <strong>Description:</strong> Size in bytes of the per-thread cache tree used to speed up bulk inserts into [MyISAM](/kb/en/myisam/) and [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) tables. A value of 0 disables the cache tree.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--bulk-insert-buffer-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8388608`
- <strong>Range - 32 bit:</strong> `0` to `4294967295`
- <strong>Range - 64 bit:</strong> `0` to `18446744073709547520`

---

#### `character_set_client`

- <strong>Description:</strong> Determines the [character set](/kb/en/data-types-character-sets-and-collations/) for queries arriving from the client. It can be set per session by the client, although the server can be configured to ignore client requests with the `--skip-character-set-client-handshake` option. If the client does not request a character set, or requests a character set that the server does not support, the global value will be used. utf16, utf32 and ucs2 cannot be used as client character sets.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `utf8`

---

#### `character_set_connection`

- <strong>Description:</strong>  [Character set](/kb/en/data-types-character-sets-and-collations/) used for number to string conversion, as well as for literals that don't have a character set introducer.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `utf8`

---

#### `character_set_database`

- <strong>Description:</strong> [Character set](/kb/en/data-types-character-sets-and-collations/) used by the default database, and set by the server whenever the default database is changed. If there's no default database, character_set_database contains the same value as [character_set_server](#character_set_server). This variable is dynamic, but should not be set manually, only by the server.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `latin1`

---

#### `character_set_filesystem`

- <strong>Description:</strong> The [character set](/kb/en/data-types-character-sets-and-collations/) for the filesystem. Used for converting file names specified as a string literal from [character_set_client](#character_set_client) to character_set_filesystem before opening the file. By default set to `binary`, so no conversion takes place. This could be useful for statements such as [LOAD_FILE()](/built-in-functions/string-functions/load_file/) or [LOAD DATA INFILE](/kb/en/load-data-infile/) on system where multi-byte file names are use.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--character-set-filesystem=name</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `binary`

---

#### `character_set_results`

- <strong>Description:</strong> [Character set](/kb/en/data-types-character-sets-and-collations/) used for results and error messages returned to the client.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `utf8`

---

#### `character_set_server`

- <strong>Description:</strong> Default [character set](/kb/en/data-types-character-sets-and-collations/) used by the server. See [character_set_database](#character_set_database) for character sets used by the default database. Defaults may be different on some systems, see for example [Differences in MariaDB in Debian](/kb/en/differences-in-mariadb-in-debian/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--character-set-server</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `latin1`

---

#### `character_set_system`

- <strong>Description:</strong> [Character set](/kb/en/data-types-character-sets-and-collations/) used by the server to store identifiers, always set to utf8.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `utf8`

---

#### `character_sets_dir`

- <strong>Description:</strong> Directory where the [character sets](/kb/en/data-types-character-sets-and-collations/) are installed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--character-sets-dir=path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> directory name

---

#### `check_constraint_checks`

- <strong>Description:</strong> If set to `0`, will disable [constraint checks](/sql-statements-structure/sql-statements/data-definition/constraint/), for example when loading a table that violates some constraints that you plan to fix later.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--check-constraint-checks=[0|1]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> boolean
- <strong>Default:</strong> ON
- <strong>Introduced:</strong> [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/)

---

#### `collation_connection`

- <strong>Description:</strong> Collation used for the connection [character set](/kb/en/data-types-character-sets-and-collations/).
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`

---

#### `collation_database`

- <strong>Description:</strong> [Collation used](/kb/en/data-types-character-sets-and-collations/) for the default database. Set by the server if the default database changes, if there is no default database the value from the `collation_server` variable is used. This variable is dynamic, but should not be set manually, only by the server.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`

---

#### `collation_server`

- <strong>Description:</strong> Default [collation](/kb/en/data-types-character-sets-and-collations/) used by the server. This is set to the default collation for a given character set automatically when [character_set_server](#character_set_server) is changed, but it can also be set manually. Defaults may be different on some systems, see for example [Differences in MariaDB in Debian](/kb/en/differences-in-mariadb-in-debian/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--collation-server=name</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `latin1_swedish_ci`

---

#### `completion_type`

- <strong>Description:</strong> The transaction completion type. If set to `NO_CHAIN` or `0` (the default), there is no effect on commits and rollbacks. If set to `CHAIN` or `1`, a [COMMIT](/kb/en/transactions-commit-statement/) statement is equivalent to COMMIT AND CHAIN, while a [ROLLBACK](/kb/en/rollback-statement/) is equivalent to ROLLBACK AND CHAIN, so a new transaction starts straight away with the same isolation level as transaction that's just finished. If set to `RELEASE` or `2`, a [COMMIT](/kb/en/transactions-commit-statement/) statement is equivalent to COMMIT RELEASE, while a [ROLLBACK](/kb/en/rollback-statement/) is equivalent to ROLLBACK RELEASE, so the server will disconnect after the transaction completes. Note that the transaction completion type only applies to explicit commits, not implicit commits.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--completion-type=name</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `NO_CHAIN`
- <strong>Valid Values:</strong> `0`, `1`, `2`, `NO_CHAIN`, `CHAIN`, `RELEASE`

---

#### `concurrent_insert`

- <strong>Description:</strong> If set to `AUTO` or `1`, the default, MariaDB allows [concurrent INSERTs](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts/) and SELECTs for [MyISAM](/kb/en/myisam/) tables with no free blocks in the data (deleted rows in the middle). If set to `NEVER` or `0`, concurrent inserts are disabled. If set to `ALWAYS` or `2`, concurrent inserts are permitted for all MyISAM tables, even those with holes, in which case new rows are added at the end of a table if the table is being used by another thread. <br><br>If the [--skip-new](/kb/en/mysqld-options/#-skip-new) option is used when starting the server, concurrent_insert is set to `NEVER`. <br><br>Changing the variable only affects new opened tables. Use [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) If you want it to also affect cached tables. <br><br>See  [Concurrent Inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts/) for more.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--concurrent-insert[=value]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `AUTO`
- <strong>Valid Values:</strong> `0`, `1`, `2`, `AUTO`, `NEVER`, `ALWAYS`

---

#### `connect_timeout`

- <strong>Description:</strong> Time in seconds that the server waits for a connect packet before returning a 'Bad handshake'. Increasing may help if clients regularly encounter 'Lost connection to MySQL server at 'X', system error: error_number' type-errors.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--connect-timeout=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> numeric
- <strong>Default Value:</strong> `10`

---

#### `core_file`

- <strong>Description:</strong> Write a core-file on crashes. The file name and location are system dependent. On Linux it is usually called `core.${PID}`, and it is usually written to the data directory. However, this can be changed.
<ul start="1"><li>See [Enabling Core Dumps](/kb/en/enabling-core-dumps/) for more information.
</li><li>Previously this system variable existed only as an [option](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/), but it was also made into a read-only system variable starting with [MariaDB 10.3.9](/kb/en/mariadb-1039-release-notes/), [MariaDB 10.2.17](/kb/en/mariadb-10217-release-notes/) and [MariaDB 10.1.35](/kb/en/mariadb-10135-release-notes/).
</li><li>On Windows &gt;= [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/), this option is set by default. 
</li><li>Note that the option accepts no arguments; specifying `--core-file` sets the value to `ON`. It cannot be disabled in the case of Windows &gt;= [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/).
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--core-file</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> boolean
- <strong>Default Value:</strong> 
<ul start="1"><li>Windows &gt;= [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/): `ON`
</li><li>All other systems: `OFF`
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.3.9](/kb/en/mariadb-1039-release-notes/), [MariaDB 10.2.17](/kb/en/mariadb-10217-release-notes/), [MariaDB 10.1.35](/kb/en/mariadb-10135-release-notes/)

---

#### `datadir`

- <strong>Description:</strong> Directory where the data is stored.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--datadir=path</code> or <code class="fixed" style="white-space:pre-wrap">-h path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> directory name

---

#### `date_format`

- <strong>Description:</strong> Unused.

---

#### `datetime_format`

- <strong>Description:</strong> Unused.

---

#### `debug`

- <strong>Description:</strong> Available in debug builds only (built with -DWITH_DEBUG=1). Used in debugging through the DBUG library to write to a trace file. Just using <code class="fixed" style="white-space:pre-wrap">--debug</code> will write a trace of what mysqld is doing to the default trace file.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">-#</code>, <code class="fixed" style="white-space:pre-wrap">--debug[=debug_options]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> 
<ul start="1"><li>&lt;= [MariaDB 10.4](/kb/en/what-is-mariadb-104/): `d:t:i:o,/tmp/mysqld.trace` (Unix) or `d:t:i:O,\mysqld.trace` (Windows)
</li><li>&gt;= [MariaDB 10.5](/kb/en/what-is-mariadb-105/): `d:t:i:o,/tmp/mariadbd.trace` (Unix) or `d:t:i:O,\mariadbd.trace` (Windows)
</li></ul>
- <strong>Debug Options:</strong> See the option flags on the [mysql_debug](/kb/en/mysql_debug/) page

---

#### `debug_no_thread_alarm`

- <strong>Description:</strong> Disable system thread alarm calls. Disabling it may be useful in debugging or testing, never do it in production.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--debug-no-thead-alarm=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> MariaDB

---

#### `debug_sync`

- <strong>Description:</strong> Used in debugging to show the interface to the [Debug Sync facility](/clients-utilities/mysqltest/the-debug-sync-facility/). MariaDB needs to be configured with -DENABLE_DEBUG_SYNC=1 for this variable to be available.
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `OFF` or `ON - current signal <em>signal name</em>`

---

#### `default_password_lifetime`

- <strong>Description:</strong> This defines the global [password expiration policy](/mariadb-administration/user-server-security/user-account-management/user-password-expiry/). 0 means automatic password expiration is disabled. If the value is a positive integer N, the passwords must be changed every N days. This behavior can be overridden using the password expiration options in [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--default-password-lifetime=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> numeric
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)

---

#### `default_regex_flags`

- <strong>Description:</strong> Introduced to address remaining incompatibilities between [PCRE](/kb/en/pcre-regular-expressions/) and the old regex library. Accepts a comma-separated list of zero or more of the following values:

<table><tbody><tr><td>Value</td><td>Pattern equivalent</td><td>Meaning</td></tr>
<tr><td>DOTALL</td><td>(?s)</td><td>. matches anything including NL</td></tr>
<tr><td>DUPNAMES</td><td>(?J)</td><td>Allow duplicate names for subpatterns</td></tr>
<tr><td>EXTENDED</td><td>(?x)</td><td>Ignore white space and # comments</td></tr>
<tr><td>EXTRA</td><td>(?X)</td><td>extra features (e.g. error on unknown escape character)</td></tr>
<tr><td>MULTILINE</td><td>(?m)</td><td>^ and $ match newlines within data</td></tr>
<tr><td>UNGREEDY</td><td>(?U)</td><td>Invert greediness of quantifiers</td></tr>
</tbody></table>

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--default-regex-flags=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> enumeration
- <strong>Default Value:</strong> empty
- <strong>Valid Values:</strong> `DOTALL`, `DUPNAMES`, `EXTENDED`, `EXTRA`, `MULTILINE`, `UNGREEDY`
- <strong>Introduced:</strong> [MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/)

---

#### `default_storage_engine`

- <strong>Description:</strong> The default [storage engine](/kb/en/mariadb-storage-engines/). The default storage engine must be enabled at server startup or the server won't start.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--default-storage-engine=name</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> enumeration
- <strong>Default Value:</strong> `InnoDB`

---

#### `default_table_type`

- <strong>Description:</strong> A synonym for [default_storage_engine](#default_storage_engine). Removed in [MariaDB 5.5](/kb/en/what-is-mariadb-55/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--default-table-type=name</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Removed:</strong> MariaDB/MySQL 5.5

---

#### `default_tmp_storage_engine`

- <strong>Description:</strong>  Default storage engine that will be used for tables created with [CREATE TEMPORARY TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) where no engine is specified. For internal temporary tables see [aria_used_for_temp_tables](/kb/en/aria-system-variables/#aria_used_for_temp_tables)). The storage engine used must be active or the server will not start. See [default_storage_engine](#default_storage_engine) for the default for non-temporary tables. Defaults to NULL, in which case the value from [default_storage_engine](#default_storage_engine) is used.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--default-tmp-storage-engine=name</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> NULL
- <strong>Introduced:</strong> [MariaDB 10.1.0](/kb/en/mariadb-1010-release-notes/)

---

#### `default_week_format`

- <strong>Description:</strong> Default mode for the [WEEK()](/built-in-functions/date-time-functions/week/) function. See that page for details on the different modes
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--default-week-format=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `7`

---

#### `delay_key_write`

- <strong>Description:</strong> Specifies how MyISAM tables handles [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) DELAY_KEY_WRITE. If set to `ON`, the default, any DELAY KEY WRITEs are honored. The key buffer is then flushed only when the table closes, speeding up writes. MyISAM tables should be automatically checked upon startup in this case, and --external locking should not be used, as it can lead to index corruption. If set to `OFF`, DELAY KEY WRITEs are ignored, while if set to `ALL`, all new opened tables are treated as if created with DELAY KEY WRITEs enabled.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--delay-key-write[=name]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `ON`, `OFF`, `ALL`

---

#### `delayed_insert_limit`

- <strong>Description:</strong> After this many rows have been inserted with [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/), the handler will check for and execute any waiting [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statements.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--delayed-insert-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100`
- <strong>Range:</strong> `1` to `4294967295`

---

#### `delayed_insert_timeout`

- <strong>Description:</strong> Time in seconds that the [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/) handler will wait for INSERTs before terminating.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--delayed-insert-timeout=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `300`

---

#### `delayed_queue_size`

- <strong>Description:</strong> Number of rows, per table, that can be queued when performing [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/) statements. If the queue becomes full, clients attempting to perform INSERT DELAYED's will wait until the queue has room available again.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--delayed-queue-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> numeric
- <strong>Default Value:</strong> `1000`
- <strong>Range:</strong> `1 to 4294967295`

---

#### `disconnect_on_expired_password`

- <strong>Description:</strong> When a user password has expired (see [User Password Expiry](/mariadb-administration/user-server-security/user-account-management/user-password-expiry/)), this variable controls how the server handles clients that are not aware of the sandbox mode. If enabled, the client is not permitted to connect, otherwise the server puts the client in a sandbox mode.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--disconnect-on-expired-password[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> boolean
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)

---

#### `div_precision_increment`

- <strong>Description:</strong> The precision of the result of the decimal division will be the larger than the precision of the dividend by that number. By default it's `4`, so `SELECT 2/15` would return 0.1333 and `SELECT 2.0/15` would return 0.13333. After setting div_precision_increment to `6`, for example, the same operation would return  0.133333 and 0.1333333 respectively.

From [MariaDB 10.1.46](/kb/en/mariadb-10146-release-notes/), [MariaDB 10.2.33](/kb/en/mariadb-10233-release-notes/), [MariaDB 10.3.24](/kb/en/mariadb-10324-release-notes/), [MariaDB 10.4.14](/kb/en/mariadb-10414-release-notes/) and [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/), `div_precision_increment` is taken into account in intermediate calculations. Previous versions did not, and the results were dependent on the optimizer, and therefore unpredictable.

In [MariaDB 10.1.46](/kb/en/mariadb-10146-release-notes/), [MariaDB 10.1.47](/kb/en/mariadb-10147-release-notes/), [MariaDB 10.2.33](/kb/en/mariadb-10233-release-notes/), [MariaDB 10.2.34](/kb/en/mariadb-10234-release-notes/), [MariaDB 10.2.35](/kb/en/mariadb-10235-release-notes/), [MariaDB 10.3.24](/kb/en/mariadb-10324-release-notes/), [MariaDB 10.3.25](/kb/en/mariadb-10325-release-notes/), [MariaDB 10.4.14](/kb/en/mariadb-10414-release-notes/), [MariaDB 10.4.15](/kb/en/mariadb-10415-release-notes/), [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/) and [MariaDB 10.5.6](/kb/en/mariadb-1056-release-notes/) only, the fix truncated decimal values after every division, resulting in lower precision in some cases for those versions only.

From [MariaDB 10.1.48](/kb/en/mariadb-10148-release-notes/), [MariaDB 10.2.35](/kb/en/mariadb-10235-release-notes/), [MariaDB 10.3.26](/kb/en/mariadb-10326-release-notes/), [MariaDB 10.4.16](/kb/en/mariadb-10416-release-notes/) and [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/), a different fix was implemented. Instead of truncating decimal values after every division, they are instead truncated for comparison purposes only.

For example

Versions other than [MariaDB 10.1.46](/kb/en/mariadb-10146-release-notes/), [MariaDB 10.1.47](/kb/en/mariadb-10147-release-notes/), [MariaDB 10.2.33](/kb/en/mariadb-10233-release-notes/), [MariaDB 10.2.34](/kb/en/mariadb-10234-release-notes/), [MariaDB 10.2.35](/kb/en/mariadb-10235-release-notes/), [MariaDB 10.3.24](/kb/en/mariadb-10324-release-notes/), [MariaDB 10.3.25](/kb/en/mariadb-10325-release-notes/), [MariaDB 10.4.14](/kb/en/mariadb-10414-release-notes/), [MariaDB 10.4.15](/kb/en/mariadb-10415-release-notes/), [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/) and [MariaDB 10.5.6](/kb/en/mariadb-1056-release-notes/):

```sql
SELECT (55/23244*1000);
+-----------------+
| (55/23244*1000) |
+-----------------+
|          2.3662 |
+-----------------
```

[MariaDB 10.1.46](/kb/en/mariadb-10146-release-notes/), [MariaDB 10.1.47](/kb/en/mariadb-10147-release-notes/), [MariaDB 10.2.33](/kb/en/mariadb-10233-release-notes/), [MariaDB 10.2.34](/kb/en/mariadb-10234-release-notes/), [MariaDB 10.2.35](/kb/en/mariadb-10235-release-notes/), [MariaDB 10.3.24](/kb/en/mariadb-10324-release-notes/), [MariaDB 10.3.25](/kb/en/mariadb-10325-release-notes/), [MariaDB 10.4.14](/kb/en/mariadb-10414-release-notes/), [MariaDB 10.4.15](/kb/en/mariadb-10415-release-notes/), [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/) and [MariaDB 10.5.6](/kb/en/mariadb-1056-release-notes/) only:

```sql
SELECT (55/23244*1000);
+-----------------+
| (55/23244*1000) |
+-----------------+
|          2.4000 |
+-----------------+
```

This is because the intermediate result, `SELECT 55/23244` takes into account `div_precision_increment` and results were truncated after every division in those versions only.

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--div-precision-increment=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4`
- <strong>Range:</strong> `0` to `30`

---

#### `encrypt_tmp_disk_tables`

- <strong>Description:</strong> Enables automatic encryption of all internal on-disk temporary tables that are created during query execution if <a undefined>aria_used_for_temp_tables=ON</a> is set. See [Data at Rest Encryption](/kb/en/data-at-rest-encryption/) and [Enabling Encryption for Internal On-disk Temporary Tables](/kb/en/encrypting-data-for-aria/#enabling-encryption-for-internal-on-disk-temporary-tables).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--encrypt-tmp-disk-tables[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> 10.1.3

---

#### `encrypt_tmp_files`

- <strong>Description:</strong> Enables automatic encryption of temporary files, such as those created for filesort operations, binary log file caches, etc. See [Data at Rest Encryption](/kb/en/data-at-rest-encryption/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--encrypt-tmp-files[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF` (&gt;= [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)), `ON` (&lt;= [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/))
- <strong>Introduced:</strong> [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/)

---

#### `encryption_algorithm`

- <strong>Description:</strong> Which encryption algorithm to use for table encryption. `aes_cbc` is the recommended one. See [Table and Tablespace Encryption](/kb/en/table-and-tablespace-encryption/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--encryption-algorithm=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `none`
- <strong>Valid Values:</strong> `none`, `aes_ecb`, `aes_cbc`, `aes_ctr`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `enforce_storage_engine`

- <strong>Description:</strong> Force the use of a particular storage engine for new tables. Used to avoid unwanted creation of tables using another engine. For example, setting to [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) will prevent any [MyISAM](/kb/en/myisam/) tables from being created. If another engine is specified in a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement, the outcome depends on whether the `NO_ENGINE_SUBSTITUTION` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) has been set or not. If set (the default from [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)), the query will fail, while if not set, a warning will be returned and the table created according to the engine specified by this variable. The variable has a session scope, but is only modifiable by a user with the SUPER privilege.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `none`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `engine_condition_pushdown`

- <strong>Description:</strong> Deprecated in [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and removed and replaced by the [optimizer_switch](#optimizer_switch) `engine_condition_pushdown={on|off}` flag in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).. Specifies whether the engine condition pushdown optimization is enabled. Since [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), engine condition pushdown is enabled for all engines that support it.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--engine-condition-pushdown</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Deprecated:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `eq_range_index_dive_limit`

- <strong>Description:</strong> Limit used for speeding up queries listed by long nested INs. The optimizer will use existing index statistics instead of doing index dives for equality ranges if the number of equality ranges for the index is larger than or equal to this number. If set to `0` (unlimited, the default), index dives are always used.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--eq-range-index-dive-limit=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `200` (&gt;= [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)), `0` (&lt;= [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/))
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/)

---

#### `error_count`

- <strong>Description:</strong> Read-only variable denoting the number of errors from the most recent statement in the current session that generated errors. See [SHOW_ERRORS()](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-errors/).
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`

---

#### `event_scheduler`

- <strong>Description:</strong> Status of the [Event](/programming-customizing-mariadb/triggers-events/event-scheduler/events/) Scheduler. Can be set to `ON` or `OFF`, while `DISABLED` means it cannot be set at runtime. Setting the variable will cause a load of events if they were not loaded at startup.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--event-scheduler[=value]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `OFF`
- <strong>Valid Values:</strong> `ON` (or `1`), `OFF` (or `0`), `DISABLED`

---

#### `expensive_subquery_limit`

- <strong>Description:</strong> Number of rows to be examined for a query to be considered expensive, that is, maximum number of rows a subquery may examine in order to be executed during optimization and used for constant optimization.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--expensive-subquery-limit=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100`
- <strong>Range:</strong> `0` upwards

---

#### `explicit_defaults_for_timestamp`

- <strong>Description:</strong> This option causes [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) to create all [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp/) columns as [NULL](/columns-storage-engines-and-plugins/data-types/null-values/) with the DEFAULT NULL attribute, Without this option, TIMESTAMP columns are NOT NULL and have implicit DEFAULT clauses.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--explicit-defaults-for-timestamp=[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `bolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.8](/kb/en/mariadb-1018-release-notes/)

---

#### `external_user`

- <strong>Description:</strong> External user name set by the plugin used to authenticate the client. `NULL` if native MariaDB authentication is used.
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `NULL`

---

#### `flush`

- <strong>Description:</strong> Usually, MariaDB writes changes to disk after each SQL statement, and the operating system handles synchronizing (flushing) it to disk. If set to `ON`, the server will synchronize all changes to disk after each statement.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--flush</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `flush_time`

- <strong>Description:</strong> Interval in seconds that tables are closed to synchronize (flush) data to disk and free up resources. If set to 0, the default, there is no automatic synchronizing tables and closing of tables. This option should not be necessary on systems with sufficient resources.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--flush_time=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`

---

#### `foreign_key_checks`

- <strong>Description:</strong> If set to 1 (the default) [foreign key constraints](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys/) (including ON UPDATE and ON DELETE behavior) [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables are checked, while if set to 0, they are not checked. `0` is not recommended for normal use, though it can be useful in situations where you know the data is consistent, but want to reload data in a different order from that that specified by parent/child relationships. Setting this variable to 1 does not retrospectively check for inconsistencies introduced while set to 0.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`

---

#### `ft_boolean_syntax`

- <strong>Description:</strong> List of operators supported by an IN BOOLEAN MODE [full-text search](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/). If you wish to change, note that each character must be ASCII and non-alphanumeric, the full string must be 14 characters and the first or second character must be a space. Positions 10, 13 and 14 are reserved for future extensions. Also, no duplicates are permitted except for the phrase quoting characters in positions 11 and 12, which may be the same.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--ft-boolean-syntax=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `+ -&gt;&lt;()*:""&amp;|`

---

#### `ft_max_word_len`

- <strong>Description:</strong> Maximum length for a word to be included in the [MyISAM](/kb/en/myisam/) [full-text index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/). If this variable is changed, the full-text index must be rebuilt. The quickest way to do this is by issuing a <code class="fixed" style="white-space:pre-wrap">REPAIR TABLE table_name QUICK</code> statement. See [innodb_ft_max_token_size](/kb/en/innodb-system-variables/#innodb_ft_max_token_size) for the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) equivalent.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--ft-max-word-len=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `84`
- <strong>Minimum Value:</strong> `10`

---

#### `ft_min_word_len`

- <strong>Description:</strong> Minimum length for a word to be included in the [MyISAM](/kb/en/myisam/) [full-text index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/). If this variable is changed, the full-text index must be rebuilt. The quickest way to do this is by issuing a <code class="fixed" style="white-space:pre-wrap">REPAIR TABLE table_name QUICK</code> statement. See [innodb_ft_min_token_size](/kb/en/innodb-system-variables/#innodb_ft_min_token_size) for the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) equivalent.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--ft-min-word-len=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4`
- <strong>Minimum Value:</strong> `1`

---

#### `ft_query_expansion_limit`

- <strong>Description:</strong> For [full-text searches](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/), denotes the numer of top matches when using WITH QUERY EXPANSION.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--ft-query-expansion-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `20`
- <strong>Range:</strong> `0` to `1000`

---

#### `ft_stopword_file`

- <strong>Description:</strong> File containing a list of [stopwords](/kb/en/stopwords/) for use in [MyISAM](/kb/en/myisam/) [full-text searches](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/). Unless an absolute path is specified the file will be looked for in the data directory. The file is not parsed for comments, so all words found become stopwords. By default, a built-in list of words (built from `storage/myisam/ft_static.c file`) is used. Stopwords can be disabled by setting this variable to `''` (an empty string). If this variable is changed, the full-text index must be rebuilt. The quickest way to do this is by issuing a <code class="fixed" style="white-space:pre-wrap">REPAIR TABLE table_name QUICK</code> statement. See [innodb_ft_server_stopword_table](/kb/en/innodb-system-variables/#innodb_ft_server_stopword_table) for the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) equivalent.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--ft-stopword-file=file_name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `file name`
- <strong>Default Value:</strong> `(built-in)`

---

#### `general_log`

- <strong>Description:</strong> If set to 0, the default unless the --general-log option is used, the [general query log](/mariadb-administration/server-monitoring-logs/general-query-log/) is disabled, while if set to 1, the general query log is enabled. See [log_output](#log_output) for how log files are written. If that variable is set to `NONE`, no logs will be written even if general_query_log is set to `1`.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--general-log</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `general_log_file`

- <strong>Description:</strong> Name of the [general query log](/mariadb-administration/server-monitoring-logs/general-query-log/) file. If this is not specified, the name is taken from the [log-basename](/kb/en/mysqld-options/#-log-basename) setting or from your system hostname with <code class="highlight fixed" style="white-space:pre-wrap">.log</code> as a suffix.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--general-log-file=file_name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `file name`
- <strong>Default Value:</strong> `<em>host_name</em>.log`

---

#### `group_concat_max_len`

- <strong>Description:</strong> Maximum length in bytes of the returned result for a [GROUP_CONCAT()](/built-in-functions/aggregate-functions/group_concat/) function.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--group-concat-max-len=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>
<ul start="1"><li>`1048576` (1M) &gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)
</li><li>`1024` (1K) &lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)
</li></ul>
- <strong>Range - 32-bit:</strong> `4` to `4294967295`
- <strong>Range - 64-bit:</strong> `4` to `18446744073709547520`

---

.

#### `have_compress`

- <strong>Description:</strong> If the zlib compression library is accessible to the server, this will be set to `YES`, otherwise it will be `NO`. The [COMPRESS()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/compress/) and [UNCOMPRESS()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/uncompress/) functions will only be available if set to `YES`.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No

---

#### `have_crypt`

- <strong>Description:</strong> If the crypt() system call is available this variable will be set to `YES`, otherwise it will be set to `NO`.  If set to `NO`, the [ENCRYPT()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/encrypt/) function cannot be used.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No

---

#### `have_csv`

- <strong>Description:</strong> If the server supports [CSV tables](/columns-storage-engines-and-plugins/storage-engines/csv/), will be set to `YES`, otherwise will be set to `NO`. Removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/), use the [Information Schema PLUGINS](/kb/en/information-schema-plugins-table/) table or [SHOW ENGINES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engines/) instead.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `have_dynamic_loading`

- <strong>Description:</strong> If the server supports dynamic loading of [plugins](/kb/en/mariadb-plugins/), will be set to `YES`, otherwise will be set to `NO`.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No

---

#### `have_geometry`

- <strong>Description:</strong> If the server supports spatial data types, will be set to `YES`, otherwise will be set to `NO`.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No

---

#### `have_ndbcluster`

- <strong>Description:</strong> If the server supports NDBCluster ([disabled in MariaDB](/kb/en/ndb-disabled-in-mariadb/)).
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `have_partitioning`

- <strong>Description:</strong> If the server supports partitioning, will be set to `YES`, unless the `--skip-partition` option is used, in which case will be set to `DISABLED`. Will be set to `NO` otherwise. Removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/) - [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins/) should be used instead.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `have_profiling`

- <strong>Description:</strong> If statement profiling is available, will be set to `YES`, otherwise will be set to `NO`.  See [SHOW PROFILES()](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-profiles/) and [SHOW PROFILE()](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-profile/).
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No

---

#### `have_query_cache`

- <strong>Description:</strong> If the server supports the [query cache](/kb/en/the-query-cache/), will be set to `YES`, otherwise will be set to `NO`.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No

---

#### `have_rtree_keys`

- <strong>Description:</strong> If RTREE indexes (used for [spatial indexes](/sql-statements-structure/geographic-geometric-features/spatial-index/)) are available, will be set to `YES`, otherwise will be set to `NO`.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No

---

#### `have_symlink`

- <strong>Description:</strong> This system variable can be used to determine whether the server supports symbolic links (note that it has no meaning on Windows).
<ul start="1"><li>If symbolic links are supported, then the value will be `YES`.
</li><li>If symbolic links are not supported, then the value will be `NO`.
</li><li>If symbolic links are disabled with the [--symbolic-links](/kb/en/mysqld-options/#-symbolic-links) option and the `skip` [option prefix](/kb/en/mysqld-options/#option-prefixes) (i.e. --skip-symbolic-links), then the value will be `DISABLED`.
</li><li>Symbolic link support is required for the [INDEX DIRECTORY](/kb/en/create-table/#data-directoryindex-directory) and [DATA DIRECTORY](/kb/en/create-table/#data-directoryindex-directory) table options.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No

---

#### `histogram_size`

- <strong>Description:</strong> Number of bytes used for a [histogram](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/histogram-based-statistics/). If set to 0, no histograms are created by [ANALYZE](/sql-statements-structure/sql-statements/table-statements/analyze-table/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--histogram-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `254` (&gt;= [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)), `0` (&lt;= [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/))
- <strong>Range:</strong> `0` to `255`
- <strong>Introduced:</strong> [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/)

---

#### `histogram_type`

- <strong>Description:</strong> Specifies the type of [histograms](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/histogram-based-statistics/) created by [ANALYZE](/sql-statements-structure/sql-statements/table-statements/analyze-table/). 
<ul start="1"><li>`SINGLE_PREC_HB` - single precision height-balanced.
</li><li>`DOUBLE_PREC_HB` - double precision height-balanced.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--histogram-type=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `DOUBLE_PREC_HB` (&gt;= [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)), `SINGLE_PREC_HB`(&lt;= [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/))
- <strong>Valid Values:</strong> `SINGLE_PREC_HB`, `DOUBLE_PREC_HB`
- <strong>Introduced:</strong> [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/)

---

#### `host_cache_size`

- <strong>Description:</strong> Number of host names that will be cached to avoid resolving. Setting to `0` disables the cache. Changing the value while the server is running causes an implicit [FLUSH HOSTS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/), clearing the host cache and truncating the [performance_schema.host_cache](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-host_cache-table/) table. If you are connecting from a lot of different machines you should consider increasing.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--host-cache-size=#</code>.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `128`
- <strong>Range:</strong> `0` to `65536`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `hostname`

- <strong>Description:</strong> When the server starts, this variable is set to the server host name.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---

#### `identity`

- <strong>Description:</strong> A synonym for [last_insert_id](#last_insert_id) variable.

---

#### `idle_readonly_transaction_timeout`

- <strong>Description:</strong> Time in seconds that the server waits for idle read-only transactions before killing the connection. If set to `0`, the default, connections are never killed. See also [idle_transaction_timeout](#idle_transaction_timeout), [idle_write_transaction_timeout](#idle_write_transaction_timeout) and [Transaction Timeouts](/sql-statements-structure/sql-statements/transactions/transaction-timeouts/).
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `31536000`
- <strong>Introduced:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `idle_transaction_timeout`

- <strong>Description:</strong> Time in seconds that the server waits for idle transactions before killing the connection. If set to `0`, the default, connections are never killed. See also [idle_readonly_transaction_timeout](#idle_readonly_transaction_timeout), [idle_write_transaction_timeout](#idle_write_transaction_timeout) and [Transaction Timeouts](/sql-statements-structure/sql-statements/transactions/transaction-timeouts/).
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `31536000`
- <strong>Introduced:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `idle_write_transaction_timeout`

- <strong>Description:</strong> Time in seconds that the server waits for idle read-write transactions before killing the connection. If set to `0`, the default, connections are never killed. See also [idle_transaction_timeout](#idle_transaction_timeout), [idle_readonly_transaction_timeout](#idle_readonly_transaction_timeout) and  [Transaction Timeouts](/sql-statements-structure/sql-statements/transactions/transaction-timeouts/). Called `idle_readwrite_transaction_timeout` until [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/).
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `31536000`
- <strong>Introduced:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `ignore_db_dirs`

- <strong>Description:</strong> Tells the server that this directory can never be a database. That means two things - firstly it is ignored by the [SHOW DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases/) command and [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) tables. And secondly, USE, CREATE DATABASE and SELECT statements will return an error if the database from the ignored list specified. Use this option several times if you need to ignore more than one directory. To make the list empty set the void value to the option as --ignore-db-dir=. If the option or configuration is specified multiple times, viewing this value will list the ignore directories separated by a period.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--ignore-db-dirs=dir</code>.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---

#### `in_predicate_conversion_threshold`

- <strong>Description:</strong> The minimum number of scalar elements in the value list of an IN predicate that triggers its conversion to an IN subquery. Set to 0 to disable the conversion. See [Conversion of Big IN Predicates Into Subqueries](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/conversion-of-big-in-predicates-into-subqueries/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--in-predicate-conversion-threshold=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1000`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.3.18](/kb/en/mariadb-10318-release-notes/) (previously debug builds only)

---

#### `in_transaction`

- <strong>Description:</strong> Session-only and read-only variable that is set to `1` if a transaction is in progress, `0` if not.
- <strong>Commandline:</strong> No
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `init_connect`

- <strong>Description:</strong> String containing one or more SQL statements, separated by semicolons, that will be executed by the server for each client connecting. If there's a syntax error in the one of the statements, the client will fail to connect. For this reason, the statements are not executed for users with the [SUPER](/kb/en/grant/#super) privilege or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [CONNECTION ADMIN](/kb/en/grant/#connection-admin) privilege, who can then still connect and correct the error. See also [init_file](#init_file).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--init-connect=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`

---

#### `init_file`

- <strong>Description:</strong> Name of a file containing SQL statements that will be executed by the server on startup. Each statement should be on a new line, and end with a semicolon. See also [init_connect](#init_connect).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">init-file=file_name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `file name`

---

#### `insert_id`

- <strong>Description:</strong> Value to be used for the next statement inserting a new [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) value.
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`

---

#### `interactive_timeout`

- <strong>Description:</strong> Time in seconds that the server waits for an interactive connection (one that connects with the mysql_real_connect() CLIENT_INTERACTIVE option) to become active before closing it. See also [wait_timeout](#wait_timeout).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--interactive-timeout=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `28800`
- <strong>Range: (Windows): `1` to `2147483`</strong>
- <strong>Range: (Other): `1` to `31536000`</strong>

---

#### `join_buffer_size`

- <strong>Description:</strong> Minimum size in bytes of the buffer used for queries that cannot use an index, and instead perform a full table scan. Increase to get faster full joins when adding indexes is not possible, although be aware of memory issues, since joins will always allocate the minimum size. Best left low globally and set high in sessions that require large full joins. In 64-bit platforms, Windows truncates values above 4GB to 4GB with a warning. See also [Block-Based Join Algorithms - Size of Join Buffers](/kb/en/block-based-join-algorithms/#size-of-join-buffers).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--join-buffer-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `262144` (256kB) (&gt;=[MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)), `131072` (128kB) (&lt;=[MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/))
- <strong>Range (&gt;=MariaDB/MySQL 5.5):</strong> `128` to `18446744073709547520`
- <strong>Range (&lt;=MariaDB/MySQL 5.3, Windows):</strong> `8228` to `18446744073709547520`

---

#### `join_buffer_space_limit`

- <strong>Description:</strong> Maximum size in bytes of the query buffer, By default 1024*128*10. See [Block-based join algorithms](/kb/en/block-based-join-algorithms/#size-of-join-buffers).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--join-buffer-space-limit=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `2097152`
- <strong>Range:</strong> `2048` to `99999999997952`

---

#### `join_cache_level`

- <strong>Description:</strong> Controls which of the eight block-based algorithms can be used for join operations. See [Block-based join algorithms](/replication/optimization-and-tuning/query-optimizations/block-based-join-algorithms/) for more information.
<ul><li>1 – flat (Block Nested Loop) BNL
</li><li>2 – incremental BNL
</li><li>3 – flat Block Nested Loop Hash (BNLH)
</li><li>4 – incremental BNLH
</li><li>5 – flat Batch Key Access (BKA)
</li><li>6 – incremental BKA
</li><li>7 – flat Batch Key Access Hash (BKAH)
</li><li>8 – incremental BKAH 
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--join-cache-level=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `2`
- <strong>Range:</strong> `0` to `8`

---

#### `keep_files_on_create`

- <strong>Description:</strong> If a [MyISAM](/kb/en/myisam/) table is created with no DATA DIRECTORY option, the .MYD file is stored in the database directory. When set to `0`, the default, if MariaDB finds another .MYD file in the database directory it will overwrite it. Setting this variable to `1` means that MariaDB will return an error instead, just as it usually does in the same situation outside of the database directory. The same applies for .MYI files and no INDEX DIRECTORY option.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--keep-files-on-create=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `large_files_support`

- <strong>Description:</strong> ON if the server if was compiled with large file support or not, else OFF
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No

---

#### `large_page_size`

- <strong>Description:</strong> Indicates the size of memory page if large page support (Linux only) is enabled. The page size is determined from the Hugepagesize setting in `/proc/meminfo`. See [large_pages](#large_pages). Deprecated and unused in [MariaDB 10.5.3](/kb/en/mariadb-1053-release-notes/) since multiple page size support was added.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> Autosized (see description)
- <strong>Deprecated:</strong> [MariaDB 10.5.3](/kb/en/mariadb-1053-release-notes/)

---

#### `large_pages`

- <strong>Description:</strong> Indicates whether large page support (Linux only - called huge pages) is used.  This is set with `--large-pages` or disabled with `--skip-large-pages`. Large pages are used for the [innodb buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/) and for online DDL (of size 3* [innodb_sort_buffer_size](/kb/en/innodb-system-variables/#innodb_sort_buffer_size) (or 6 when encryption is used)). To use large pages, the Linux `sysctl` variable `kernel.shmmax` must be large than the llocation. Also the `sysctl` variable `vm.nr_hugepages` multipled by [large-page](#large_page_size)) must be larger than the usage. The ulimit for locked memory must be sufficient to cover the amount used (`ulimit -l` and equalivent in /etc/security/limits.conf / or in systemd [LimitMEMLOCK](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/)). If these operating system controls or insufficient free huge pages are available, the allocation of large pages will fall back to conventional memory allocation and a warning will appear in the logs. Only allocations of the default `Hugepagesize` currently occur (see `/proc/meminfo`).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--large-pages</code>, <code class="fixed" style="white-space:pre-wrap">--skip-large-pages</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `last_insert_id`

- <strong>Description:</strong> Contains the same value as that returned by [LAST_INSERT_ID()](/built-in-functions/secondary-functions/information-functions/last_insert_id/). Note that setting this variable doen't update the value returned by the underlying function.
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`

---

#### `lc_messages`

- <strong>Description:</strong> This system variable can be specified as a [locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale/) name. The language of the associated [locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale/) will be used for error messages. See [Server Locales](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale/) for a list of supported locales and their associated languages.
<ul start="1"><li>This system variable is set to `en_US` by default, which means that error messages are in English by default.
</li><li>If this system variable is set to a valid [locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale/) name, but the server can't find an [error message file](/kb/en/error-log/#error-messages-file) for the language associated with the [locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale/), then the default language will be used instead.
</li><li>This system variable is used along with the <a undefined>lc_messages_dir</a> system variable to construct the path to the [error messages file](/kb/en/error-log/#error-messages-file).
</li><li>See [Setting the Language for Error Messages](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/setting-the-language-for-error-messages/) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--lc-messages=name</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `en_us`

---

#### `lc_messages_dir`

- <strong>Description:</strong> This system variable can be specified either as the path to the directory storing the server's [error message files](/kb/en/error-log/#error-messages-file) or as the path to the directory storing the specific language's [error message file](/kb/en/error-log/#error-messages-file). See [Server Locales](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale/) for a list of available locales and their related languages.
<ul start="1"><li>The server initially tries to interpret the value of this system variable as a path to the directory storing the server's [error message files](/kb/en/error-log/#error-messages-file). Therefore, it constructs the path to the language's [error message file](/kb/en/error-log/#error-messages-file) by concatenating the value of this system variable with the language name of the [locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale/) specified by the <a undefined>lc_messages</a> system variable .
</li><li>If the server does not find the [error message file](/kb/en/error-log/#error-messages-file) for the language, then it tries to interpret the value of this system variable as a direct path to the directory storing the specific language's [error message file](/kb/en/error-log/#error-messages-file).
</li><li>See [Setting the Language for Error Messages](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/setting-the-language-for-error-messages/) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--lc-messages-dir=path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `directory name`

---

#### `lc_time_names`

- <strong>Description:</strong> The locale that determines the language used for the date and time functions [DAYNAME()](/built-in-functions/date-time-functions/dayname/), [MONTHNAME()](/built-in-functions/date-time-functions/monthname/) and [DATE_FORMAT()](date-format). Locale names are language and region subtags, for example 'en_ZA' (English - South Africa) or 'es_US: Spanish - United States'. The default is always 'en-US' regardless of the system's locale setting. See [server locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale/) for a full list of supported locales.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--lc-time-names=name</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `en_US`

---

#### `license`

- <strong>Description:</strong> Server license, for example `GPL`.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---

#### `local_infile`

- <strong>Description:</strong> If set to `1`, LOCAL is supported for [LOAD DATA INFILE](/kb/en/load-data-infile/) statements. If set to `0`, usually for security reasons, attempts to perform a LOAD DATA LOCAL will fail with an error message.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--local-infile=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`

---

#### `lock_wait_timeout`

- <strong>Description:</strong> Timeout in seconds for attempts to acquire [metadata locks](/sql-statements-structure/sql-statements/transactions/metadata-locking/). Statements using metadata locks include [FLUSH TABLES WITH READ LOCK](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/), [LOCK TABLES](/kb/en/transactions-lock/), HANDLER and DML and DDL operations on tables, [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/) and [functions](/programming-customizing-mariadb/stored-routines/stored-functions/), and [views](/programming-customizing-mariadb/views/). The timeout is separate for each attempt, of which there may be multiple in a single statement. `0` (from [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)) means no wait. See [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--lock-wait-timeout=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>
<ul start="1"><li>`86400` (1 day) &gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)
</li><li>`31536000` (1 year) &lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)
</li></ul>
- <strong>Range:</strong> 
<ul start="1"><li>`0` to `31536000` (&gt;= [MariaDB 10.3](/kb/en/what-is-mariadb-103/))
</li><li>`1` to `31536000` (&lt;= [MariaDB 10.2](/kb/en/what-is-mariadb-102/))
</li></ul>

---

#### `locked_in_memory`

- <strong>Description:</strong> Indicates whether --memlock was used to lock mysqld in memory.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--memlock</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `log`

- <strong>Description:</strong> Deprecated and removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/), use [general_log](#general_log) instead.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">-l [filename]</code> or <code class="fixed" style="white-space:pre-wrap">--log[=filename]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `OFF`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `log_disabled_statements`

- <strong>Description:</strong> If set, the specified type of statements (slave or stored procedure statements) will not be logged to the [general log](/mariadb-administration/server-monitoring-logs/general-query-log/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-disabled_statements=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `set`
- <strong>Default Value:</strong> (empty string)
- <strong>Valid Vales:</strong> `slave` and/or `sp`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)

---

#### `log_error`

- <strong>Description:</strong> Specifies the name of the [error log](/mariadb-administration/server-monitoring-logs/error-log/). If [--console](/kb/en/mysqld-options/#-console) is specified later in the configuration (Windows only) or this option isn't specified, errors will be logged to stderr. If no name is provided, errors will still be logged to `hostname.err` in the `datadir` directory by default. If a configuration file sets `--log-error`, one can reset it with `--skip-log-error` (useful to override a system wide configuration file). MariaDB always writes its error log, but the destination is configurable. See [error log](/mariadb-administration/server-monitoring-logs/error-log/) for details.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-error[=name]</code>, <code class="fixed" style="white-space:pre-wrap">--skip-log-error</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `file name`
- <strong>Default Value:</strong> (empty string)

---

#### `log_output`

- <strong>Description:</strong> How the output for the [general query log](/mariadb-administration/server-monitoring-logs/general-query-log/) and the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) is stored. By default written to file (`FILE`), it can also be stored in the [general_log](/kb/en/mysqlgeneral_log-table/) and [slow_log](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlslow_log-table/) tables in the mysql database (`TABLE`), or not stored at all (`NONE`). More than one option can be chosen at the same time, with `NONE` taking precedence if present. Logs will not be written if logging is not enabled. See [Writing logs into tables](/mariadb-administration/server-monitoring-logs/writing-logs-into-tables/), and the [slow_query_log](#slow_query_log) and [general_log](#general_log) server system variables.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-output=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `set`
- <strong>Default Value:</strong> `FILE`
- <strong>Valid Values:</strong> `TABLE`, `FILE` or `NONE`

---

#### `log_queries_not_using_indexes`

- <strong>Description:</strong> Queries that don't use an index, or that perform a  full index scan where the index doesn't limit the number of rows, will be logged to the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) (regardless of time taken). The slow query log needs to be enabled for this to have an effect. Mapped to `log_slow_filter='not_using_index'` from [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-queries-not-using-indexes</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `log_slow_admin_statements`

- <strong>Description:</strong> Log slow [OPTIMIZE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/), [ANALYZE](/sql-statements-structure/sql-statements/table-statements/analyze-table/), [ALTER](/sql-statements-structure/sql-statements/data-definition/alter/) and other [administrative](/kb/en/slow-query-log-overview/#logging-slow-administrative-statements) statements to the [slow log](/mariadb-administration/server-monitoring-logs/slow-query-log/) if it is open. Before [MariaDB 10.1.13](/kb/en/mariadb-10113-release-notes/), this was only available as a mysqld option, not a server variable. See also [log_slow_disabled_statements](#log_slow_disabled_statements) and [log_slow_filter](#log_slow_filter).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-slow-admin-statements</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong>
<ul start="1"><li>`ON ` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/))
</li><li>`OFF` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))
</li></ul>
- <strong>Introduced:</strong> [MariaDB 10.1.13](/kb/en/mariadb-10113-release-notes/) (variable)

---

#### `log_slow_disabled_statements`

- <strong>Description:</strong> If set, the specified type of statements will not be logged to the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/). See also [log_slow_admin_statements](#log_slow_admin_statements) and [log_slow_filter](#log_slow_filter).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-slow-disabled_statements=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `set`
- <strong>Default Value:</strong> `sp`
- <strong>Valid Vales:</strong> `admin`, `call`, `slave` and/or `sp`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)

---

#### `log_slow_filter`

- <strong>Description:</strong> Comma-delimited string containing one or more settings for filtering what is logged to the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/). If a query matches one of the types listed in the filter, and takes longer than [long_query_time](#long_query_time), it will be logged(except for 'not_using_index' which is always logged if enabled, regardless of the time). Sets [log-slow-admin-statements](#log_slow_admin_statements) to ON. See also [log_slow_disabled_statements](#log_slow_disabled_statements).
<ul><li>`admin` log [administrative](/kb/en/slow-query-log-overview/#logging-slow-administrative-statements) queries (create, optimize, drop etc...) 
</li><li>`filesort` logs queries that use a filesort. 
</li><li>`filesort_on_disk` logs queries that perform a a filesort on disk.
</li><li>`filesort_priority_queue` (from [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/))
</li><li>`full_join` logs queries that perform a join without indexes.
</li><li>`full_scan` logs queries that perform full table scans.
</li><li>`not_using_index` logs queries that don't use an index, or that perform a full index scan where the index doesn't limit the number of rows. Disregards [long_query_time](#long_query_time), unlike other options. [log_queries_not_using_indexes](#log_queries_not_using_indexes) maps to this option. From [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/).
</li><li>`query_cache` log queries that are resolved by the query cache.
</li><li>`query_cache_miss` logs queries that are not found in the [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache/).
</li><li>`tmp_table` logs queries that create an implicit temporary table. 
</li><li>`tmp_table_on_disk` logs queries that create a temporary table on disk.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">log-slow-filter=value1[,value2...]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> 
<ul start="1"><li>`admin`, `filesort`, `filesort_on_disk`, `full_join`, `full_scan`, `query_cache`, `query_cache_miss`, `tmp_table`, `tmp_table_on_disk` (&lt;= [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/))
</li><li>`admin`, `filesort`, `filesort_on_disk`, `filesort_priority_queue`, `full_join`, `full_scan`, `query_cache`, `query_cache_miss`, `tmp_table`, `tmp_table_on_disk` (&gt;= [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/))
</li></ul>
- <strong>Valid Values:</strong> `admin`, `filesort`, `filesort_on_disk`, `filesort_priority_queue`, `full_join`, `full_scan`, `query_cache`, `query_cache_miss`, `tmp_table`, `tmp_table_on_disk`

---

#### `log_slow_queries`

- <strong>Description:</strong> Deprecated and removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/), use [slow_query_log](#slow_query_log) instead.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-slow-queries[=name]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `log_slow_rate_limit`

- <strong>Description:</strong> The [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) will log every this many queries. The default is `1`, or every query, while setting it to `20` would log every 20 queries, or five percent. Aims to reduce I/O usage and excessively large slow query logs. See also [Slow Query Log Extended Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/slow-query-log-extended-statistics/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">log-slow-rate-limit=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` upwards

---

#### `log_slow_verbosity`

- <strong>Description:</strong> Controls information to be added to the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/). Options are added in a comma-delimited string. See also [Slow Query Log Extended Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/slow-query-log-extended-statistics/). log_slow_verbosity is not supported when log_output='TABLE'.
<ul><li>`query_plan` logs query execution plan information
</li><li>`innodb` an unused Percona XtraDB option for logging XtraDB/InnoDB statistics.
</li><li>`explain` prints EXPLAIN output in the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/). See [EXPLAIN in the Slow Query Log](/mariadb-administration/server-monitoring-logs/slow-query-log/explain-in-the-slow-query-log/).
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">log-slow-verbosity=value1[,value2...]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> (Empty)
- <strong>Valid Values:</strong> (Empty), `query_plan`, `innodb`, `explain` (from [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/))

---

#### `log_tc_size`

- <strong>Description:</strong> Defines the size in bytes of the memory-mapped file-based transaction coordinator log, which is only used if the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is disabled. If you have two or more XA-capable storage engines enabled, then a transaction coordinator log must be available. This size is defined in multiples of 4096. This size could always be set as a commandline option, but it was made into a system variable in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/). See [Transaction Coordinator Log](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log/) for more information. Also see the <a undefined>--log-tc</a> server option and the <a undefined>--tc-heuristic-recover</a> option.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">log-tc-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `24576`
- <strong>Range:</strong> `12288` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/) (variable)

---

#### `log_warnings`

- <strong>Description:</strong> Determines which additional warnings are logged. Setting to `0` disables additional warning logging. Note that this does not prevent all warnings, there is a core set of warnings that will always be written to the error log. The additional warnings are as follows:
<ul start="1"><li>log_warnings &gt;= 1
<ul start="1"><li>[Event scheduler](/programming-customizing-mariadb/triggers-events/event-scheduler/) information.
</li><li>System signals
</li><li>Wrong usage of <code class="fixed" style="white-space:pre-wrap">--user</code>
</li><li>Failed setrlimit() and mlockall()
</li><li>Changed limits
</li><li>Wrong values of lower_case_table_names and stack_size
</li><li>Wrong values for command line options
</li><li>Start log position and some master information when starting slaves
</li><li>Slave reconnects
</li><li>Killed slaves
</li><li>Error reading relay logs
</li><li>[Unsafe statements for statement-based replication](/replication/standard-replication/unsafe-statements-for-statement-based-replication/). If this warning occurs frequently, it is throttled to prevent flooding the log.
</li><li>Disabled [plugins](/kb/en/mariadb-plugins/) that one tried to enable or use.
</li><li>UDF files that didn't include the required init functions.
</li><li>DNS lookup failures.
</li></ul>
</li><li>log_warnings &gt;= 2
<ul start="1"><li>Access denied errors.
</li><li>Connections aborted or closed due to errors or timeouts.
</li><li>Table handler errors
</li><li>Messages related to the files used to [persist replication state](/kb/en/change-master-to/#option-persistence):
<ul start="1"><li>Either the default `master.info` file or the file that is configured by the <a undefined>master_info_file</a> option.
</li><li>Either the default `relay-log.info` file or the file that is configured by the <a undefined>relay_log_info_file</a> system variable.
</li></ul>
</li><li>Information about a master's [binary log dump thread](/kb/en/replication-threads/#binary-log-dump-thread).
</li></ul>
</li><li>log_warnings &gt;= 3
<ul start="1"><li>All errors and warnings during [MyISAM](/kb/en/myisam/) repair and auto recover.
</li><li>Information about old-style language options.
</li><li>Information about [progress of InnoDB online DDL](https://mariadb.org/monitoring-progress-and-temporal-memory-usage-of-online-ddl-in-innodb/).
</li></ul>
</li><li>log_warnings &gt;=4
<ul start="1"><li>Connections aborted due to "Too many connections" errors.
</li><li>Connections closed normally.
</li><li>Connections aborted due to <a undefined>KILL</a>.
</li><li>Connections closed due to released connections, such as when <a undefined>completion_type</a> is set to `RELEASE`.
</li></ul>
</li><li>log_warnings &gt;=9
<ul start="1"><li>Information about initializing plugins.
</li></ul>
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">-W [level]</code> or <code class="fixed" style="white-space:pre-wrap">--log-warnings[=level]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>
<ul start="1"><li>`2` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/))
</li><li>`1` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))
</li></ul>
- <strong>Range:</strong> `0` to `4294967295` <strong>
</strong>

---

#### `long_query_time`

- <strong>Description:</strong> If a query takes longer than this many seconds to execute (microseconds can be specified too), the [Slow_queries](/kb/en/server-status-variables/#slow_queries) status variable is incremented and, if enabled, the query is logged to the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--long-query-time=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10.000000` &gt;= [MariaDB 10.1.13](/kb/en/mariadb-10113-release-notes/), `10` &lt;= [MariaDB 10.1.12](/kb/en/mariadb-10112-release-notes/)
- <strong>Range:</strong> `0` upwards

---

#### `low_priority_updates`

- <strong>Description:</strong> If set to 1 (0 is the default), for [storage engines](/columns-storage-engines-and-plugins/storage-engines/) that use only table-level locking ([Aria](/columns-storage-engines-and-plugins/storage-engines/aria/), [MyISAM](/kb/en/myisam/), [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) and [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge/)), all INSERTs, UPDATEs, DELETEs and LOCK TABLE WRITEs will wait until there are no more SELECTs or LOCK TABLE READs pending on the relevant tables. Set this to 1 if reads are prioritized over writes. 
<ul start="1"><li>In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and earlier, <a undefined>sql_low_priority_updates</a> is a synonym.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--low-priority-updates</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `lower_case_file_system`

- <strong>Description:</strong> Read-only variable describing whether the file system is case-sensitive. If set to `OFF`, file names are case-sensitive. If set to `ON`, they are not case-sensitive.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `##`

---

#### `lower_case_table_names`

- <strong>Description:</strong> If set to `0` (the default on Unix-based systems), table names and aliases and database names  are compared in a case-sensitive manner. If set to `1` (the default on Windows), names are stored in lowercase and not compared in a case-sensitive manner. If set to `2` (the default on Mac OS X), names are stored as declared, but compared in lowercase.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--lower-case-table-names[=#]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0` (Unix), `1` (Windows), `2` (Mac OS X)
- <strong>Range:</strong> `0` to `2`

---

#### `max_allowed_packet`

- <strong>Description:</strong> Maximum size in bytes of a packet or a generated/intermediate string. The packet message buffer is initialized with the value from [net_buffer_length](#net_buffer_length), but can grow up to max_allowed_packet bytes. Set as large as the largest BLOB, in multiples of 1024. If this value is changed, it should be changed on the client side as well. See [slave_max_allowed_packet](/kb/en/replication-and-binary-log-server-system-variables/#slave_max_allowed_packet) for a specific limit for replication purposes.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-allowed-packet=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes (Global), No (Session)
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>
<ul start="1"><li>`16777216` (16M) &gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)
</li><li>`4194304` (4M) &gt;= [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)
</li><li>`1048576` (1MB) &lt;= [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)
</li><li>`1073741824` (1GB) (client-side)
</li></ul>
- <strong>Range:</strong> `1024` to `1073741824`

---

#### `max_connect_errors`

- <strong>Description:</strong> Limit to the number of successive failed connects from a host before the host is blocked from making further connections. The count for a host is reset to zero if they successfully connect. To unblock, flush the host cache with a [FLUSH HOSTS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) statement or [mysqladmin flush-hosts](/clients-utilities/mysqladmin/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-connect-errors=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100` (from [MariaDB 10.0](/kb/en/what-is-mariadb-100/)), `10` (before [MariaDB 10.0](/kb/en/what-is-mariadb-100/))

---

#### `max_connections`

- <strong>Description:</strong> The maximum number of simultaneous client connections. See also [Handling Too Many Connections](/replication/optimization-and-tuning/system-variables/handling-too-many-connections/). Note that this value affects the number of file descriptors required on the operating system. Minimum was changed from `1` to `10` to avoid possible unexpected results for the user ([MDEV-18252](https://jira.mariadb.org/browse/MDEV-18252)).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-connections=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `151`
- <strong>Range:</strong> 
<ul start="1"><li>`10` to `100000` (&gt;= [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/), [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), [MariaDB 10.1.33](/kb/en/mariadb-10133-release-notes/))
</li><li>`1` to `100000` (&lt;= [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/), [MariaDB 10.2.14](/kb/en/mariadb-10214-release-notes/), [MariaDB 10.1.32](/kb/en/mariadb-10132-release-notes/))
</li></ul>

---

#### `max_delayed_threads`

- <strong>Description:</strong> Limits to the number of [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/) threads. Once this limit is reached, the insert is handled as if there was no DELAYED attribute. If set to `0`, DELAYED is ignored entirely. The session value can only be set to `0` or to the same as the global value.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-delayed-threads=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `20`
- <strong>Range:</strong> `0` to `16384`

---

#### `max_digest_length`

- <strong>Description:</strong> Maximum length considered for computing a statement digest, such as used by the [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/) and query rewrite plugins. Statements that differ after this many bytes produce the same digest, and are aggregated for statistics purposes. The variable is allocated per session. Increasing will allow longer statements to be distinguished from each other, but increase memory use, while decreasing will reduce memory use, but more statements may become indistinguishable.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-digest-length=#</code>
- <strong>Scope:</strong> Global,
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1024`
- <strong>Range:</strong> `0` to `1048576`
- <strong>Introduced:</strong> [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/)

---

#### `max_error_count`

- <strong>Description:</strong> Specifies the maximum number of messages stored for display by [SHOW ERRORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-errors/) and [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/) statements.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-error-count=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `64`
- <strong>Range:</strong> `0` to `65535`

---

#### `max_heap_table_size`

- <strong>Description:</strong> Maximum size in bytes for user-created [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) tables. Setting the variable while the server is active has no effect on existing tables unless they are recreated or altered. The smaller of max_heap_table_size and [tmp_table_size](#tmp_table_size) also limits internal in-memory tables. When the maximum size is reached, any further attempts to insert data will receive a "table ... is full" error. Temporary tables created with [CREATE TEMPORARY](/sql-statements-structure/sql-statements/data-definition/create/create-table/) will not be converted to Aria, as occurs with internal temporary tables, but will also receive a table full error.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-heap-table-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `16777216`
- <strong>Range :</strong> `16384` to `4294966272`

---

#### `max_insert_delayed_threads`

- <strong>Description:</strong> Synonym for [max_delayed_threads](#max_delayed_threads).

---

#### `max_join_size`

- <strong>Description:</strong> Statements will not be performed if they are likely to need to examine more than this number of rows, row combinations or do more disk seeks. Can prevent poorly-formatted queries from taking server resources. Changing this value to anything other the default will reset [sql_big_selects](#sql_big_selects) to 0. If sql_big_selects is set again, max_join_size will be ignored. This limit is also ignored if the query result is sitting in the [query cache](/kb/en/the-query-cache/). Previously named [sql_max_join_size](#sql_max_join_size), which is still a synonym.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-join-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `18446744073709551615`
- <strong>Range:</strong> `1` to `18446744073709551615`

---

#### `max_length_for_sort_data`

- <strong>Description:</strong> Used to decide which algorithm to choose when sorting rows. If the total size of the column data, not including columns that are part of the sort, is less than `max_length_for_sort_data`, then we add these to the sort key. This can speed up the sort as we don't have to re-read the same row again later. Setting the value too high can slow things down as there will be a higher disk activity for doing the sort.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-length-for-sort-data=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1024`
- <strong>Range:</strong> `4` to `8388608`

---

#### `max_long_data_size`

- <strong>Description:</strong> Maximum size for parameter values sent with mysql_stmt_send_long_data(). If not set, will default to the value of [max_allowed_packet](#max_allowed_packet). Deprecated in [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and removed in [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/); use [max_allowed_packet](#max_allowed_packet) instead.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-long-data-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>
<ul start="1"><li>`16777216` (16M) &gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)
</li><li>`4194304` (4M) &lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/), &gt;= [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)
</li><li>`1048576` (1M) &lt;= [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)
</li></ul>
- <strong>Range:</strong> `1024` to `4294967295`
- <strong>Deprecated:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- <strong>Removed:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `max_password_errors`

- <strong>Description:</strong> The maximum permitted number of failed connection attempts due to an invalid password before a user is blocked from further connections. [FLUSH_PRIVILEGES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) will permit the user to connect again. This limit is ignored for users with the [SUPER](/kb/en/grant/#super) privilege or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [CONNECTION ADMIN](/kb/en/grant/#connection-admin) privilege.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-password-errors=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4294967295`
- <strong>Range:</strong> `1` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/)

---

#### `max_prepared_stmt_count`

- <strong>Description:</strong> Maximum number of prepared statements on the server. Can help prevent certain forms of denial-of-service attacks. If set to `0`, no prepared statements are permitted on the server.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-prepared-stmt-count=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `16382`
- <strong>Range:</strong> `0` to `4294967295` (&gt;= [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/)), `0` to `1048576` (&lt;= [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/))

---

#### `max_recursive_iterations`

- <strong>Description:</strong> Maximum number of iterations when executing recursive queries, used to prevent infinite loops in [recursive CTEs](/kb/en/recursive-common-table-expressions-overview/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-recursive-iterations=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4294967295`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `max_rowid_filter_size`

- <strong>Description:</strong> The maximum size of the container of a rowid filter.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-rowid-filter-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `131072`
- <strong>Range:</strong> `1024` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)

---

#### `max_seeks_for_key`

- <strong>Description:</strong> The optimizer assumes that the number specified here is the most key seeks required when searching with an index, regardless of the actual index cardinality. If this value is set lower than its default and maximum, indexes will tend to be preferred over table scans.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-seeks-for-key=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4294967295`
- <strong>Range:</strong> `1` to `4294967295`

---

#### `max_session_mem_used`

- <strong>Description:</strong> Amount of memory a single user session is allowed to allocate. This limits the value of the session variable [Memory_used](/kb/en/server-status-variables/#memory_used).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-session-mem-used=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `9223372036854775807` (8192 PB)
- <strong>Range:</strong> `8192` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.1.21](/kb/en/mariadb-10121-release-notes/)

---

#### `max_sort_length`

- <strong>Description:</strong> Maximum size in bytes used for sorting data values - anything exceeding this is ignored. The server uses only the first `max_sort_length` bytes of each value and ignores the rest.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-sort-length=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1024`
- <strong>Range:</strong>
<ul><li>`4` to `8388608`  (&lt;= [MariaDB 10.1.45](/kb/en/mariadb-10145-release-notes/), [MariaDB 10.2.32](/kb/en/mariadb-10232-release-notes/), [MariaDB 10.3.23](/kb/en/mariadb-10323-release-notes/), [MariaDB 10.4.13](/kb/en/mariadb-10413-release-notes/), [MariaDB 10.5.3](/kb/en/mariadb-1053-release-notes/)) 
</li><li>`8` to `8388608`  (&gt;=  [MariaDB 10.1.46](/kb/en/mariadb-10146-release-notes/), [MariaDB 10.2.33](/kb/en/mariadb-10233-release-notes/), [MariaDB 10.3.24](/kb/en/mariadb-10324-release-notes/), [MariaDB 10.4.14](/kb/en/mariadb-10414-release-notes/), [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/))
</li></ul>

---

#### `max_sp_recursion_depth`

- <strong>Description:</strong> Permitted number of recursive calls for a [stored procedure](/programming-customizing-mariadb/stored-routines/stored-procedures/). `0`, the default, no recursion is permitted. Increasing this value increases the thread stack requirements, so you may need to increase [thread_stack](#thread_stack) as well. This limit doesn't apply to [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-sp-recursion-depth[=#]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `255`

---

#### `max_statement_time`

- <strong>Description:</strong> Maximum time in seconds that a query can execute before being aborted. This includes all queries, not just [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statements, but excludes statements in stored procedures. If set to 0, no limit is applied. See [Aborting statements that take longer than a certain time to execute](/kb/en/aborting-statements-that-take-longer-than-a-certain-time-to-execute/) for details and limitations. Useful when combined with [SET STATEMENT](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set-statement/) for limiting the execution times of individual queries.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-statement-time[=#]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0.000000` &gt;= [MariaDB 10.1.13](/kb/en/mariadb-10113-release-notes/), `0` &lt;= [MariaDB 10.1.12](/kb/en/mariadb-10112-release-notes/)
- <strong>Range:</strong> `0` upwards
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `max_tmp_tables`

- <strong>Description:</strong> Unused.

---

#### `max_user_connections`

- <strong>Description:</strong> 
Maximum simultaneous connections permitted for each user account.  When set to `0`, there is no per user limit.  Setting it to `-1` stops users without the [SUPER](/kb/en/grant/#super) privilege or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [CONNECTION ADMIN](/kb/en/grant/#connection-admin) privilege, from connecting to the server.  The session variable is always read-only and only privileged users can modify user limits.  The session variable defaults to the global `max_user_connections` variable, unless the user's specific <a undefined>MAX_USER_CONNECTIONS</a> resource option is non-zero.  When both global variable and the user resource option are set, the user's [MAX_USER_CONNECTIONS](/kb/en/create-user/#max_user_connections) is used.  Note: This variable does not affect users with the [SUPER](/kb/en/grant/#super) privilege or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [CONNECTION ADMIN](/kb/en/grant/#connection-admin) privilege.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-user-connections=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes, (except when globally set to `0` or `-1`)
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `-1` to `4294967295`

---

#### `max_write_lock_count`

- <strong>Description:</strong> Read lock requests will be permitted for processing after this many write locks. Applies only to storage engines that use table level locks (thr_lock), so no effect with [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) or [Archive](/columns-storage-engines-and-plugins/storage-engines/archive/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-write-lock-count=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4294967295`
- <strong>Range:</strong> `0-4294967295`

---

#### `metadata_locks_cache_size`

- <strong>Description:</strong> Size of the metadata locks cache, used for reducing the need to create and destroy synchronization objects. It is particularly helpful on systems where this process is inefficient, such as Windows XP.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--metadata-locks-cache-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1024`
- <strong>Range:</strong> `1` to `1048576`

---

#### `metadata_locks_hash_instances`

- <strong>Description:</strong> Number of hashes used by the set of metadata locks. The metadata locks are partitioned into separate hashes in order to reduce contention.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--metadata-locks-hash-instances=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8`
- <strong>Range:</strong> `1` to `1024`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `min_examined_row_limit`

- <strong>Description:</strong> If a query examines more than this number of rows, it is logged to the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/). If set to `0`, the default, no row limit is used.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--min-examined-row-limit=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0-4294967295`

---

#### `mrr_buffer_size`

- <strong>Description:</strong> Size of buffer to use when using multi-range read with range access. See [Multi Range Read optimization](/kb/en/multi-range-read-optimization/#range-access) for more information.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mrr-buffer-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `262144`
- <strong>Range</strong> `8192` to `2147483648`

---

#### `multi_range_count`

- <strong>Description:</strong> Ignored. Use [mrr_buffer_size](#mrr_buffer_size) instead.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--multi-range-count=#</code>
- <strong>Default Value:</strong> `256`
- <strong>Removed:</strong> [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)

---

#### `mysql56_temporal_format`

- <strong>Description:</strong> If set (the default), MariaDB uses the MySQL 5.6 low level formats for [TIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/time/), [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime/) and [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp/) instead of the [MariaDB 5.3](/kb/en/what-is-mariadb-53/) version. The version MySQL introduced in 5.6 requires more storage, but potentially allows negative dates and has some advantages in replication. There should be no reason to revert to the old [MariaDB 5.3](/kb/en/what-is-mariadb-53/) microsecond format. See also [MDEV-10723](https://jira.mariadb.org/browse/MDEV-10723).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--mysql56-temporal-format</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

#### `named_pipe`

- <strong>Description:</strong> On Windows systems, determines whether connections over named pipes are permitted.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--named-pipe</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `net_buffer_length`

- <strong>Description:</strong> The starting size, in bytes, for the connection and thread buffers for each client thread. The size can grow to [max_allowed_packet](#max_allowed_packet). This variable's session value is read-only. Can be set to the expected length of client statements if memory is a limitation.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--net-buffer-length=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `16384`
- <strong>Range:</strong> `1024` to `1048576`

---

#### `net_read_timeout`

- <strong>Description:</strong> Time in seconds the server will wait for a client connection to send more data before aborting the read. See also [net_write_timeout](#net_write_timeout) and [slave_net_timeout](/kb/en/replication-and-binary-log-server-system-variables/#slave_net_timeout)
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--net-read-timeout=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `30`
- <strong>Range:</strong> `1` upwards

---

#### `net_retry_count`

- <strong>Description:</strong> Permit this many retries before aborting when attempting to read or write on a communication port. On FreeBSD systems should be set higher as threads are sent internal interrupts..
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--net-retry-count=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `1` to `4294967295`

---

#### `net_write_timeout`

- <strong>Description:</strong> Time in seconds to wait on writing a block to a connection before aborting the write. See also [net_read_timeout](#net_read_timeout) and [slave_net_timeout](/kb/en/replication-and-binary-log-server-system-variables/#slave_net_timeout).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--net-write-timeout=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `60`
- <strong>Range:</strong> `1` upwards

---

#### `old`

- <strong>Description:</strong> Disabled by default, enabling it reverts index hints to those used before MySQL 5.1.17. Enabling may lead to replication errors. Being replaced by [old_mode](#old_mode).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--old</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `old_alter_table`

- <strong>Description:</strong> From [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/), an alias for [alter_algorithm](#alter_algorithm). Prior to that, if set to `1` (`0` is default), MariaDB reverts to the non-optimized, pre-MySQL 5.1, method of processing [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statements. A temporary table is created, the data is copied over, and then the temporary table is renamed to the original.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--old-alter-table</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumerated` (&gt;=[MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)), `boolean` (&lt;= [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/))
- <strong>Default Value:</strong> See [alter_algorithm](#alter_algorithm) (&gt;= [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)), `0` (&lt;= [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/))
- <strong>Valid Values:</strong> See [alter_algorithm](#alter_algorithm) for the full list.
- <strong>Deprecated:</strong> [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/) (superceded by [alter_algorithm](#alter_algorithm))

---

#### `old_mode`

- <strong>Description:</strong> Used for getting MariaDB to emulate behavior from an old version of MySQL or MariaDB. See [OLD Mode](/kb/en/old_mode/). Will be used to replace the [old](#old) variable over time.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--old-mode</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `(empty string)`
- <strong>Valid Values:</strong> See [OLD Mode](/kb/en/old_mode/) for the full list.

---

#### `old_passwords`

- <strong>Description:</strong> If set to `1` (`0` is default), MariaDB reverts to using the [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password/) authentication plugin by default for newly created users and passwords, instead of the [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password/) authentication plugin.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `open_files_limit`

- <strong>Description:</strong> The number of file descriptors available to MariaDB. If you are getting the `Too many open files` error, then you should increase this limit. If set to 0, then MariaDB will calculate a limit based on the following: <br>
<br>
MAX([max_connections](#max_connections)*5, [max_connections](#max_connections) +[table_open_cache](#table_open_cache)*2) <br>
<br>
MariaDB sets the limit with <a undefined>setrlimit</a>. MariaDB cannot set this to exceed the hard limit imposed by the operating system. Therefore, you may also need to change the hard limit. There are a few ways to do so. 
<ul start="1"><li>If you are using [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/) to start `mysqld`, then see the instructions at [mysqld_safe: Configuring the Open Files Limit](/kb/en/mysqld_safe/#configuring-the-open-files-limit). 
</li><li>If you are using [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) to start `mysqld`, then see the instructions at [systemd: Configuring the Open Files Limit](/kb/en/systemd/#configuring-the-open-files-limit). 
</li><li>Otherwise, you can change the hard limit for the `mysql` user account by modifying <a undefined>/etc/security/limits.conf</a>. See [Configuring Linux for MariaDB: Configuring the Open Files Limit](/kb/en/configuring-linux-for-mariadb/#configuring-the-open-files-limit) for more details.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--open-files-limit=count</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> Autosized (see description)
- <strong>Range:</strong> `0` to `4294967295`

---

#### `optimizer_max_sel_arg_weight`

- <strong>Description:</strong> The maximum weight of the SEL_ARG graph. Set to 0 for no limit.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--optimizer-max-sel-arg-weight=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `32000`
- <strong>Range:</strong> `0` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/)

---

#### `optimizer_prune_level`

- <strong>Description:</strong> If set to `1`, the default, the optimizer will use heuristics to prune less-promising partial plans from the optimizer search space. If set to `0`, heuristics are disabled and an exhaustive search is performed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--optimizer-prune-level[=#]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`

---

#### `optimizer_search_depth`

- <strong>Description:</strong> Maximum search depth by the query optimizer. Smaller values lead to less time spent on execution plans, but potentially less optimal results. If set to `0`, MariaDB will automatically choose a reasonable value. Since the better results from more optimal planning usually offset the longer time spent on planning, this is set as high as possible by default. `63` is a valid value, but its effects (switching to the original find_best search) are deprecated.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--optimizer-search-depth[=#]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `62`
- <strong>Range:</strong> `0` to `63`

---

#### `optimizer_selectivity_sampling_limit`

- <strong>Description:</strong> Controls number of record samples to check condition selectivity
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">optimizer-selectivity-sampling-limit[=#]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100`
- <strong>Range:</strong> `10` upwards
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `optimizer_switch`

- <strong>Description:</strong> A series of flags for controlling the query optimizer. See [Optimizer Switch](/replication/optimization-and-tuning/query-optimizations/optimizer-switch/) for defaults, and a comparison to MySQL.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--optimizer-switch=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Valid Values:</strong> 
<ul start="1"><li>`condition_pushdown_for_derived={on|off}` (&gt;=[MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)) 
</li><li>`condition_pushdown_for_subquery={on|off}` (&gt;=[MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/)) 
</li><li>`default` - set all optimizations to their default values.
</li><li>`derived_merge={on|off}` - see [Derived table merge optimization](/replication/optimization-and-tuning/query-optimizations/optimizations-for-derived-tables/derived-table-merge-optimization/)
</li><li>`derived_with_keys={on|off}` - see [Derived table with key optimization](/replication/optimization-and-tuning/query-optimizations/optimizations-for-derived-tables/derived-table-with-key-optimization/)
</li><li>`engine_condition_pushdown={on|off}`. Deprecated in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/) as engine condition pushdown is now automatically enabled for all engines that support it.
</li><li>`exists_to_in={on|off}` - see [EXISTS-to-IN optimization](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/exists-to-in-optimization/)
</li><li>`extended_keys={on|off}`  - see [Extended Keys](/replication/optimization-and-tuning/query-optimizations/extended-keys/)
</li><li>`firstmatch={on|off}` - see [First Match Strategy](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/firstmatch-strategy/)
</li><li>`index_condition_pushdown={on|off}` - see [Index Condition Pushdown](/replication/optimization-and-tuning/query-optimizations/index-condition-pushdown/)
</li><li>`index_merge={on|off}`
</li><li>`index_merge_intersection={on|off}`
</li><li>`index_merge_sort_intersection={on|off}` - [more details](/kb/en/index_merge_sort_intersection/)
</li><li>`index_merge_sort_union={on|off}`
</li><li>`index_merge_union={on|off}`
</li><li>`in_to_exists={on|off}` - see [IN-TO-EXISTS transformation](/kb/en/non-semi-join-subquery-optimizations/#the-in-to-exists-transformation)
</li><li>`join_cache_bka={on|off}` - see [Block-Based Join Algorithms](/replication/optimization-and-tuning/query-optimizations/block-based-join-algorithms/)
</li><li>`join_cache_hashed={on|off}` - see [Block-Based Join Algorithms](/replication/optimization-and-tuning/query-optimizations/block-based-join-algorithms/)
</li><li>`join_cache_incremental={on|off}` - see [Block-Based Join Algorithms](/replication/optimization-and-tuning/query-optimizations/block-based-join-algorithms/)
</li><li>`loosescan={on|off}` - see [LooseScan strategy](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/loosescan-strategy/)
</li><li>`materialization={on|off}` - [Semi-join](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/semi-join-materialization-strategy/) and [non semi-join](/kb/en/non-semi-join-subquery-optimizations/#materialization-for-non-correlated-in-subqueries) materialization.
</li><li>`mrr={on|off}` - see [Multi Range Read optimization](/replication/optimization-and-tuning/mariadb-internal-optimizations/multi-range-read-optimization/)
</li><li>`mrr_cost_based={on|off}` - see [Multi Range Read optimization](/replication/optimization-and-tuning/mariadb-internal-optimizations/multi-range-read-optimization/)
</li><li>`mrr_sort_keys={on|off}` - see [Multi Range Read optimization](/replication/optimization-and-tuning/mariadb-internal-optimizations/multi-range-read-optimization/)
</li><li>`optimize_join_buffer_size={on|off}` - see [Block-Based Join Algorithms](/replication/optimization-and-tuning/query-optimizations/block-based-join-algorithms/)
</li><li>`orderby_uses_equalities={on|off}` (&gt;= [MariaDB 10.1.15](/kb/en/mariadb-10115-release-notes/), [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/)) - if not set, the optimizer ignores equality propagation. See [MDEV-8989](https://jira.mariadb.org/browse/MDEV-8989).
</li><li>`outer_join_with_cache={on|off}` - see [Block-Based Join Algorithms](/replication/optimization-and-tuning/query-optimizations/block-based-join-algorithms/)
</li><li>`partial_match_rowid_merge={on|off}` - see [Non-semi-join subquery optimizations](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/non-semi-join-subquery-optimizations/)
</li><li>`partial_match_table_scan={on|off}` - see [Non-semi-join subquery optimizations](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/non-semi-join-subquery-optimizations/)
</li><li>`semijoin={on|off}` - see [Semi-join subquery optimizations](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/semi-join-subquery-optimizations/)
</li><li>`semijoin_with_cache={on|off}` - see [Block-Based Join Algorithms](/replication/optimization-and-tuning/query-optimizations/block-based-join-algorithms/)
</li><li>`subquery_cache={on|off}` - see [subquery cache](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/subquery-cache/).
</li><li>`table_elimination={on|off}` - see [Table Elimination User Interface](/replication/optimization-and-tuning/query-optimizations/table-elimination/table-elimination-user-interface/)
</li></ul>

---

#### `optimizer_trace`

- <strong>Description:</strong> Controls [tracing of the optimizer](/kb/en/optimizer-trace/): optimizer_trace=option=val[,option=val...], where option is one of {enabled} and val is one of {on, off, default}
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--optimizer-trace=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `enabled=off`
- <strong>Valid Values:</strong> `enabled={on|off|default}`
- <strong>Introduced:</strong> [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)

---

#### `optimizer_trace_max_mem_size`

- <strong>Description:</strong> Limits the memory used while tracing a query by specifying the maximum allowed cumulated size, in bytes, of stored [optimizer traces](/kb/en/optimizer-trace/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--optimizer-trace-max-mem-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1048576`
- <strong>Range:</strong> `1` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)

---

#### `optimizer_use_condition_selectivity`

- <strong>Description:</strong> Controls which statistics can be used by the optimizer when looking for
the best query execution plan.
<ul start="1"><li>`1` Use selectivity of predicates as in [MariaDB 5.5](/kb/en/what-is-mariadb-55/).
</li><li>`2` Use selectivity of all range predicates supported by indexes.
</li><li>`3` Use selectivity of all range predicates estimated without [histogram](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/histogram-based-statistics/).
</li><li>`4` Use selectivity of all range predicates estimated with [histogram](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/histogram-based-statistics/).
</li><li>`5` Additionally use selectivity of certain non-range predicates calculated on record sample.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--optimizer-use-condition-selectivity=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4` (&gt;= [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/)), `1` (&lt;= [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/))
- <strong>Range:</strong> `1` to `5`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `pid_file`

- <strong>Description:</strong> Full path of the process ID file.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pid-file=file_name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `file name`

---

#### `plugin_dir`

- <strong>Description:</strong> Path to the [plugin](/kb/en/mariadb-plugins/) directory. For security reasons, either make sure this directory can only be read by the server, or set [secure_file_priv](#secure_file_priv).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--plugin-dir=path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `directory name`
- <strong>Default Value:</strong> `BASEDIR/lib/plugin`

---

#### `plugin_maturity`

- <strong>Description:</strong> The lowest acceptable [plugin](/kb/en/mariadb-plugins/) maturity. MariaDB will not load plugins less mature than the specified level.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--plugin-maturity=level</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> enum
- <strong>Default Value:</strong> One less than the server maturity (&gt;= [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)), `unknown` (&lt;= [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/))
- <strong>Valid Values:</strong> <code class="fixed" style="white-space:pre-wrap">unknown</code>, <code class="fixed" style="white-space:pre-wrap">experimental</code>, <code class="fixed" style="white-space:pre-wrap">alpha</code>, <code class="fixed" style="white-space:pre-wrap">beta</code>, <code class="fixed" style="white-space:pre-wrap">gamma</code>, <code class="fixed" style="white-space:pre-wrap">stable</code>

---

#### `port`

- <strong>Description:</strong> Port to listen for TCP/IP connections. If set to `0`, will default to, in order of preference, my.cnf, the MYSQL_TCP_PORT [environment variable](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-environment-variables/), /etc/services, built-in default (3306).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--port=#</code>, <code class="fixed" style="white-space:pre-wrap">-P</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `3306`
- <strong>Range:</strong> `0` to `65535`

---

#### `preload_buffer_size`

- <strong>Description:</strong> Size in bytes of the buffer allocated when indexes are preloaded.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--preload-buffer-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `32768`
- <strong>Range:</strong> `1024` to `1073741824`

---

#### `profiling`

- <strong>Description:</strong> If set to `1` (`0` is default), statement profiling will be enabled. See [SHOW PROFILES()](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-profiles/) and [SHOW PROFILE()](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-profile/).
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `profiling_history_size`

- <strong>Description:</strong> Number of statements about which profiling information is maintained. If set to `0`, no profiles are stored. See [SHOW PROFILES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-profiles/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--profiling-history-size=# </code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `15`
- <strong>Range:</strong> `0` to `100`

---

#### `progress_report_time`

- <strong>Description:</strong> Time in seconds between sending [progress reports](/kb/en/progress-reporting/) to the client for time-consuming statements. If set to `0`, progress reporting will be disabled.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--progress-report-time=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `5`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `protocol_version`

- <strong>Description:</strong> The version of the client/server protocol used by the MariaDB server.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `proxy_protocol_networks`

- <strong>Description:</strong> Enable [proxy protocol](/kb/en/proxy-protocol-support/) for these source networks. The syntax is a comma separated list of IPv4 and IPv6 networks. If the network doesn't contain a mask, it is considered to be a single host. "*" represents all networks and must be the only directive on the line. String "localhost" represents non-TCP local connections (Unix domain socket, Windows named pipe or shared memory). See [Proxy Protocol Support](/kb/en/proxy-protocol-support/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--proxy-protocol-networks=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes (&gt;= [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/)), No (&lt;= [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/))
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> (empty)
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)

---

#### `proxy_user`

- <strong>Description:</strong> Set to the proxy user account name if the current client is a proxy, else  `NULL`.
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---

#### `pseudo_slave_mode`

- <strong>Description:</strong> For internal use by the server.
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `OFF`

---

#### `pseudo_thread_id`

- <strong>Description:</strong> For internal use only.
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`

---

#### `query_alloc_block_size`

- <strong>Description:</strong> Size in bytes of the extra blocks allocated during query parsing and execution (after [query_prealloc_size](#query_prealloc_size) is used up).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--query-alloc-block-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `16384` (from [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)), `8192` (before [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/))
- <strong>Range:</strong> `1024` to `4294967295`

---

#### `query_cache_limit`

- <strong>Description:</strong> Size in bytes for which results larger than this are not stored in the [query cache](/kb/en/the-query-cache/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--query-cache-limit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1048576` (1MB)
- <strong>Range:</strong> `0` to `4294967295`

---

#### `query_cache_min_res_unit`

- <strong>Description:</strong> Minimum size in bytes of the blocks allocated for [query cache](/kb/en/the-query-cache/) results.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--query-cache-min-res-unit=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4096` (4KB)
- <strong>Range:</strong> `0` to `4294967295`

---

#### `query_cache_size`

- <strong>Description:</strong> Size in bytes available to the [query cache](/kb/en/the-query-cache/). About 40KB is needed for query cache structures, so setting a size lower than this will result in a warning. `0`, the default before [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/), effectively disables the query cache.

<strong>Warning:</strong> Starting from [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/), [query_cache_type](#query_cache_type) is automatically set to ON if the server is started with the query_cache_size set to a non-zero (and non-default) value. This will happen even if [query_cache_type](#query_cache_type) is explicitly set to OFF in the configuration.

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--query-cache-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1M` (&gt;= [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)), `0` (&lt;= [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)) (although frequently given a default value in some setups)
- <strong>Valid Values:</strong> `0` upwards in units of 1024.

---

#### `query_cache_strip_comments`

- <strong>Description:</strong> If set to `1` (`0` is default), the server will strip any comments from the query before searching to see if it exists in the [query cache](/kb/en/the-query-cache/).  Multiple space, line feeds, tab and other white space characters will also be removed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">query-cache-strip-comments</code>
- <strong>Scope:</strong> Session, Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `query_cache_type`

- <strong>Description:</strong> If set to `0`, the [query cache](/kb/en/the-query-cache/) is disabled (although a buffer of  [query_cache_size](#query_cache_size) bytes is still allocated). If set to `1` all SELECT queries will be cached unless SQL_NO_CACHE is specified. If set to `2` (or `DEMAND`), only queries with the SQL CACHE clause will be cached. Note that if the server is started with the query cache disabled, it cannot be enabled at runtime.

<strong>Warning:</strong> Starting from [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/), query_cache_type is automatically set to ON if the server is started with the [query_cache_size](#query_cache_size) set to a non-zero (and non-default) value. This will happen even if [query_cache_type](#query_cache_type) is explicitly set to OFF in the configuration.

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--query-cache-type=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enumeration`
- <strong>Default Value:</strong> `OFF` (&gt;= [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)), `ON` (&lt;= [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/))
- <strong>Valid Values:</strong> `0` or `OFF`, `1` or `ON`, `2` or `DEMAND`

---

#### `query_cache_wlock_invalidate`

- <strong>Description:</strong> If set to `0`, the default, results present in the [query cache](/kb/en/the-query-cache/) will be returned even if there's a write lock on the table. If set to `1`, the client will first have to wait for the lock to be released.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--query-cache-wlock-invalidate</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `query_prealloc_size`

- <strong>Description:</strong> Size in bytes of the persistent buffer for query parsing and execution, allocated on connect and freed on disconnect. Increasing may be useful if complex queries are being run, as this will reduce the need for more memory allocations during query operation. See also [query_alloc_block_size](#query_alloc_block_size).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--query-prealloc-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `24576` (from [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)) `8192` (before [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/))
- <strong>Range:</strong> `1024` to `4294967295` (from [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)), `8192` to `4294967295` (before [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/))

---

#### `rand_seed1`

- <strong>Description:</strong> `rand_seed1` and `rand_seed2` facilitate replication of the [RAND()](/built-in-functions/numeric-functions/rand/) function. The master passes the value of these to the slaves so that the random number generator is seeded in the same way, and generates the same value, on the slave as on the master. Until [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/), the variable value could not be viewed, with the [SHOW VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables/) output always displaying zero.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> Varies
- <strong>Range:</strong> `0` to `18446744073709551615`

---

#### `rand_seed2`

- <strong>Description:</strong> See [rand_seed1](#rand_seed1).

---

#### `range_alloc_block_size`

- <strong>Description:</strong> Size in bytes of blocks allocated during range optimization. The unit size in 1024.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--range-alloc-block-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `4096`
- <strong>Range - 32 bit:</strong> `4096` to `4294967295`

---

#### `read_buffer_size`

- <strong>Description:</strong> Each thread performing a sequential scan (for MyISAM, Aria and MERGE tables) allocates a buffer of this size in bytes for each table scanned. Increase if you perform many sequential scans. If not in a multiple of 4KB, will be rounded down to the nearest multiple. Also used in ORDER BY's for caching indexes in a temporary file (not temporary table), for caching results of nested queries, for bulk inserts into partitions, and to determine the memory block size of [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) tables.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--read-buffer-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `131072`
- <strong>Range:</strong> <code class="fixed" style="white-space:pre-wrap">8200 to 2147479552</code>

---

#### `read_only`

- <strong>Description:</strong> When set to `1` (`0` is default), no updates are permitted except from users with the [SUPER](/kb/en/grant/#super) privilege or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the  [READ ONLY ADMIN](/kb/en/grant/#read_only-admin) privilege, or replica servers updating from a primary. The `read_only` variable is useful for replica servers to ensure no updates are accidentally made outside of what are performed on the primary. Inserting rows to log tables, updates to temporary tables and [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/) or [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statements are excluded from this limitation. If `read_only` is set to `1`, then the [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password/) statement is limited only to users with the [SUPER](/kb/en/grant/#super) privilege (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)) or [READ ONLY ADMIN](/kb/en/grant/#read_only-admin) privilege (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)). Attempting to set this variable to `1` will fail if the current session has table locks or transactions pending, while if other sessions hold table locks, the statement will wait until these locks are released before completing. While the attempt to set `read_only` is waiting, other requests for table locks or transactions will also wait until `read_only` has been set. See [Read-Only Replicas](/replication/standard-replication/read-only-replicas/) for more.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--read-only</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `read_rnd_buffer_size`

- <strong>Description:</strong> Size in bytes of the buffer used when reading rows from a [MyISAM](/kb/en/myisam/) table in sorted order after a key sort. Larger values improve ORDER BY performance, although rather increase the size by SESSION where the need arises to avoid excessive memory use.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--read-rnd-buffer-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `262144`
- <strong>Range:</strong> `8200` to `2147483647`

---

-

-

#### `require_secure_transport`

- <strong>Description:</strong> When this option is enabled, connections attempted using insecure transport will be rejected. Secure transports are SSL/TLS, Unix sockets or named pipes. Note that [per-account requirements](/kb/en/securing-connections-for-client-and-server/#requiring-tls) take precedence.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--require-secure-transport[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `rowid_merge_buff_size`

- <strong>Description:</strong> The maximum size in bytes of the memory available to the Rowid-merge strategy. See [Non-semi-join subquery optimizations](/kb/en/non-semi-join-subquery-optimizations/#optimizer-control) for more information.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--rowid-merge-buff-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8388608`
- <strong>Range:</strong> `0` to `2147483647`

---

#### `rpl_recovery_rank`

- <strong>Description:</strong> Unused.
- <strong>Removed:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

#### `safe_show_database`

- <strong>Description:</strong> This variable was removed in [MariaDB 5.5](/kb/en/what-is-mariadb-55/), and has been replaced by the more flexible [SHOW DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases/) privilege.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--safe-show-database</code> (until MySQL 4.1.1)
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Removed:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `secure_auth`

- <strong>Description:</strong> Connections will be blocked if they use the the [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password/) authentication plugin. The server will also fail to start if the privilege tables are in the old, pre-MySQL 4.1 format.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--secure-auth</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON` (&gt;= [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)), `OFF` (&lt;= [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/))

---

#### `secure_file_priv`

- <strong>Description:</strong> [LOAD DATA](/kb/en/load-data-infile/), [SELECT ... INTO](/kb/en/select/#into) and [LOAD FILE()](/built-in-functions/string-functions/load_file/) will only work with files in the specified path. If not set, the default, or set to empty string, the statements will work with any files that can be accessed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--secure-file-priv=path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `path name`
- <strong>Default Value:</strong> None

---

#### `secure_timestamp`

- <strong>Description:</strong> Restricts direct setting of a session timestamp. Possible levels are: 
<ul start="1"><li>YES - timestamp cannot deviate from the system clock
</li><li>REPLICATION - replication thread can adjust timestamp to match the master's
</li><li>SUPER - a user with this privilege and a replication thread can adjust timestamp
</li><li>NO - historical behavior, anyone can modify session timestamp
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--secure-timestamp=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `NO`
- <strong>Introduced:</strong> [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `session_track_schema`

- <strong>Description:</strong> Whether to track changes to the default schema within the current session.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--session-track-schema={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `session_track_state_change`

- <strong>Description:</strong> Whether to track changes to the session state.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--session-track-state-change={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `session_track_system_variables`

- <strong>Description:</strong> Comma-separated list of session system variables for which to track changes. In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), by default no variables are tracked. For compatibility with MySQL defaults, this variable should be set to "autocommit, character_set_client, character_set_connection, character_set_results, time_zone" (the default from [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)). The `*` character tracks all session variables.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--session-track-system-variables=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `autocommit, character_set_client, character_set_connection, character_set_results, time_zone` (&gt;= [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)), empty string (&lt;= [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/))
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `session_track_transaction_info`

- <strong>Description:</strong> Track changes to the transaction attributes. OFF to disable; STATE to track just transaction state (Is there an active transaction? Does it have any data? etc.); CHARACTERISTICS to track transaction state and report all statements needed to start a transaction with the same characteristics (isolation level, read only/read write,snapshot - but not any work done / data modified within the transaction).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--session-track-transaction-info=value</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `OFF`
- <strong>Valid Values:</strong> `OFF`, `STATE`, `CHARACTERISTICS`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `shared_memory`

- <strong>Description:</strong> Windows only, determines whether the server permits shared memory connections. See also [shared_memory_base_name](#shared_memory_base_name).
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No

---

#### `shared_memory_base_name`

- <strong>Description:</strong> Windows only, specifies the name of the shared memory to use for shared memory connection. Mainly used when running more than one instance on the same physical machine. By default the name is `MYSQL` and is case sensitive. See also [shared_memory](#shared_memory).
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `MYSQL`

---

#### `skip_external_locking`

- <strong>Description:</strong> If this system variable is set, then some kinds of external table locks will be disabled for some [storage engines](/columns-storage-engines-and-plugins/storage-engines/).
<ul start="1"><li>If this system variable is set, then the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine will not use file-based locks. Otherwise, it will use the <a undefined>fcntl()</a> function with the `F_SETLK` option to get file-based locks on Unix, and it will use the <a undefined>LockFileEx()</a> function to get file-based locks on Windows.
</li><li>If this system variable is set, then the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine will not lock a table when it decrements the table's in-file counter that keeps track of how many connections currently have the table open. See [MDEV-19393](https://jira.mariadb.org/browse/MDEV-19393) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--skip-external-locking</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`

---

#### `skip_name_resolve`

- <strong>Description:</strong> If set to 1 (0 is the default), only IP addresses are used for connections. Host names are not resolved. All host values in the GRANT tables must be IP addresses (or localhost).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--skip-name-resolve</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `skip_networking`

- <strong>Description:</strong> If set to 1, (0 is the default), the server does not listen for TCP/IP connections. All interaction with the server by be through socket files (Unix) or named pipes or shared memory (Windows). It's recommended to use this option if only local clients are permitted to connect to the server. Enabling this option also prevents a server from functioning as a replication client.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--skip-networking</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `skip_show_database`

- <strong>Description:</strong> If set to 1, (0 is the default), only users with the [SHOW DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases/) privilege can use the SHOW DATABASES statement to see all database names.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--skip-show-database</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `slow_launch_time`

- <strong>Description:</strong> Time in seconds. If a thread takes longer than this to launch, the `slow_launch_threads` [server status variable](/replication/optimization-and-tuning/system-variables/server-status-variables/) is incremented.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slow-launch-time=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `2`

---

#### `slow_query_log`

- <strong>Description:</strong> If set to 0, the default unless the --slow-query-log option is used, the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) is disabled, while if set to 1 (both global and session variables), the slow query log is enabled. [MariaDB 10.1](/kb/en/what-is-mariadb-101/) added support for session variables.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slow-query-log</code>
- <strong>Scope:</strong> Global, Session ([MariaDB 10.1](/kb/en/what-is-mariadb-101/))
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`
- <strong>See also:</strong>  See [log_output](#log_output) to see how log files are written. If that variable is set to `NONE`, no logs will be written even if slow_query_log is set to `1`.

---

#### `slow_query_log_file`

- <strong>Description:</strong> Name of the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) file.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slow-query-log-file=file_name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `file name`
- <strong>Default Value:</strong> `<em>host_name</em>-slow.log`

---

#### `socket`

- <strong>Description:</strong> On Unix-like systems, this is the name of the socket file used for local client connections, by default `/tmp/mysql.sock`, often changed by the distribution, for example `/var/lib/mysql/mysql.sock`. On Windows, this is the name of the named pipe used for local client connections, by default `MySQL`. On Windows, this is not case-sensitive.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--socket=name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `file name`
- <strong>Default Value:</strong> `/tmp/mysql.sock` (Unix), `MySQL` (Windows)

---

#### `sort_buffer_size`

- <strong>Description:</strong> Each session performing a sort allocates a buffer with this amount of memory. Not specific to any storage engine. If the status variable [sort_merge_passes](/kb/en/server-status-variables/#sort_merge_passes) is too high, you may need to look at improving your query indexes, or increasing this. Consider reducing where there are many small sorts, such as OLTP, and increasing where needed by session. 16k is a suggested minimum.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sort-buffer-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `number`
- <strong>Default Value:</strong> `2M (2097152)` (some distributions increase the default)

---

#### `sql_auto_is_null`

- <strong>Description:</strong> If set to 1, the query <code class="fixed" style="white-space:pre-wrap">SELECT * FROM table_name WHERE auto_increment_column IS NULL</code> will return an auto-increment that has just been successfully inserted, the same as the LAST_INSERT_ID() function. Some ODBC programs make use of this IS NULL comparison.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `sql_big_selects`

- <strong>Description:</strong> If set to 0, MariaDB will not perform large SELECTs. See [max_join_size](#max_join_size) for details. If max_join_size is set to anything but DEFAULT, sql_big_selects is automatically set to 0. If sql_big_selects is again set, max_join_size will be ignored.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`

---

#### `sql_big_tables`

- <strong>Description:</strong> Old variable, which if set to 1, allows large result sets by saving all temporary sets to disk, avoiding 'table full' errors. No longer needed, as the server now handles this automatically.
<ul start="1"><li>This is a synonym for <a undefined>big_tables</a>.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-big-tables</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `sql_buffer_result`

- <strong>Description:</strong> If set to 1 (0 is default), results from SELECT statements are always placed into temporary tables. This can help the server when it takes a long time to send the results to the client by allowing the table locks to be freed early.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `sql_if_exists`

- <strong>Description:</strong> If set to 1, adds an implicit IF EXISTS to ALTER, RENAME and DROP of TABLES, VIEWS, FUNCTIONS and PACKAGES. This variable is mainly used in replication to tag DDLs that can be ignored on the slave if the target table doesn't exist.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-if-exists[={0|1}]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

---

#### `sql_log_off`

- <strong>Description:</strong> If set to 1 (0 is the default), no logging to the [general query log](/mariadb-administration/server-monitoring-logs/general-query-log/) is done for the client. Only clients with the SUPER privilege can update this variable.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`

---

#### `sql_log_update`

- <strong>Description:</strong> Removed. Use [sql_log_bin](#sql_log_bin) instead.
- <strong>Removed:</strong> MariaDB/MySQL 5.5

---

#### `sql_low_priority_updates`

- <strong>Description:</strong> If set to 1 (0 is the default), for [storage engines](/columns-storage-engines-and-plugins/storage-engines/) that use only table-level locking ([Aria](/columns-storage-engines-and-plugins/storage-engines/aria/), [MyISAM](/kb/en/myisam/), [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) and [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge/)), all INSERTs, UPDATEs, DELETEs and LOCK TABLE WRITEs will wait until there are no more SELECTs or LOCK TABLE READs pending on the relevant tables. Set this to 1 if reads are prioritized over writes.
<ul start="1"><li>This is a synonym for <a undefined>low_priority_updates</a>.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-low-priority-updates</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `0`
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `sql_max_join_size`

- <strong>Description:</strong> Synonym for [max_join_size](#max_join_size), the preferred name.
- <strong>Deprecated:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `sql_mode`

- <strong>Description:</strong> Sets the [SQL Mode](/mariadb-administration/variables-and-modes/sql-mode/). Multiple modes can be set, separated by a comma.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-mode=value[,value[,value...]]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong>
<ul start="1"><li>`STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/))
</li><li>`NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION` (&gt;= [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/))
</li><li>`(empty string)` (&lt;= [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/))
</li></ul>
- <strong>Valid Values:</strong> See [SQL Mode](/mariadb-administration/variables-and-modes/sql-mode/) for the full list.

---

#### `sql_notes`

- <strong>Description:</strong> If set to 1, the default, [warning_count](#warning_count) is incremented each time a Note warning is encountered. If set to 0, Note warnings are not recorded. [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) has outputs to set this variable to 0 so that no unnecessary increments occur when data is reloaded.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`

---

#### `sql_quote_show_create`

- <strong>Description:</strong> If set to 1, the default, the server will quote identifiers for [SHOW CREATE DATABASE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-database/), [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) and [SHOW CREATE VIEW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-view/) statements. Quoting is disabled if set to 0. Enable to ensure replications works when identifiers require quoting.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`

---

#### `sql_safe_updates`

- <strong>Description:</strong> If set to 1, UPDATEs and DELETEs need either a key in the WHERE clause, or a LIMIT clause, or else they will aborted. Prevents the common mistake of accidentally deleting or updating every row in a table. Until [MariaDB 10.3.11](/kb/en/mariadb-10311-release-notes/), could not be set as a command-line option or in my.cnf.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-safe-updates[={0|1}]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

#### `sql_select_limit`

- <strong>Description:</strong> Maximum number of rows that can be returned from a SELECT query. Default is the maximum number of rows permitted per table by the server, usually 2<sup>32</sup>-1 or 2<sup>64</sup>-1. Can be restored to the default value after being changed by assigning it a value of DEFAULT.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `18446744073709551615`

---

#### `sql_warnings`

- <strong>Description:</strong> If set to 1, single-row INSERTs will produce a string containing warning information if a warning occurs.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF (0)`

---

#### `storage_engine`

- <strong>Description:</strong> See [default_storage_engine](#default_storage_engine).
- <strong>Deprecated:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `standard_compliant_cte`

- <strong>Description:</strong> Allow only standard-compliant [common table expressions](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/). Prior to [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/), this variable was named `standards_compliant_cte`.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--standard-compliant-cte={0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `stored_program_cache`

- <strong>Description:</strong> Limit to the number of [stored routines](/kb/en/stored-programs-and-views/) held in the stored procedures and stored functions caches. Each time a stored routine is executed, this limit is first checked, and if the number held in the cache exceeds this, that cache is flushed and memory freed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--stored-program-cache=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `256`
- <strong>Range:</strong> `256` to `524288`

---

#### `strict_password_validation`

- <strong>Description:</strong> When [password validation](/kb/en/password-validation/) plugins are enabled, reject passwords that cannot be validated (passwords specified as a hash). This excludes direct updates to the privilege tables.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--strict-password-validation</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

#### `sync_frm`

- <strong>Description:</strong> If set to 1, the default, each time a non-temporary table is created, its .frm definition file is synced to disk. Fractionally slower, but safer in case of a crash.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sync-frm</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `TRUE`

---

#### `system_time_zone`

- <strong>Description:</strong> The system time zone is determined when the server starts. The system [time zone](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones/) is usually read from the operating system's environment. See [Time Zones: System Time Zone](/kb/en/time-zones/#system-time-zone) for the various ways to change the system time zone. This variable is not the same as the <a undefined>time_zone</a> system variable, which is the variable that actually controls a session's active time zone. The system time zone is used for a session when `time_zone` is set to the special value `SYSTEM`.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`

---

#### `table_definition_cache`

- <strong>Description:</strong> Number of table definitions that can be cached. Table definitions are taken from the .frm files, and if there are a large number of tables increasing the cache size can speed up table opening. Unlike the [table_open_cache](#table_open_cache), as the table_definition_cache doesn't use file descriptors, and is much smaller.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--table-definition-cache=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `400`
- <strong>Range:</strong> 
<ul start="1"><li>`400` to `2097152` (&gt;= [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/), [MariaDB 10.3.13](/kb/en/mariadb-10313-release-notes/), [MariaDB 10.2.22](/kb/en/mariadb-10222-release-notes/), [MariaDB 10.1.38](/kb/en/mariadb-10138-release-notes/), [MariaDB 10.0.38](/kb/en/mariadb-10038-release-notes/)) 
</li><li>`400` to `524288` (&lt;= [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/), [MariaDB 10.3.12](/kb/en/mariadb-10312-release-notes/), [MariaDB 10.2.21](/kb/en/mariadb-10221-release-notes/), [MariaDB 10.1.37](/kb/en/mariadb-10137-release-notes/), [MariaDB 10.0.37](/kb/en/mariadb-10037-release-notes/)) 
</li></ul>

---

#### `table_lock_wait_timeout`

- <strong>Description:</strong> Unused, and removed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--table-lock-wait-timeout=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `50`
- <strong>Range:</strong> `1` to `1073741824`
- <strong>Removed:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `table_open_cache`

- <strong>Description:</strong> Maximum number of open tables cached in one table cache instance. See [Optimizing table_open_cache](/replication/optimization-and-tuning/system-variables/optimizing-table_open_cache/) for suggestions on optimizing. Increasing table_open_cache increases the number of file descriptors required.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--table-open-cache=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `2000` (&gt;= [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/)), `400` (&lt;= [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/))
- <strong>Range:</strong> 
<ul start="1"><li>`1` to `1048576` (1024K) (&gt;= [MariaDB 10.1.20](/kb/en/mariadb-10120-release-notes/), [MariaDB 10.0.35](/kb/en/mariadb-10035-release-notes/))
</li><li>`1` to `524288` (512K) (&lt;= [MariaDB 10.1.19](/kb/en/mariadb-10119-release-notes/), [MariaDB 10.0.34](/kb/en/mariadb-10034-release-notes/))
</li></ul>

---

#### `table_open_cache_instances`

- <strong>Description:</strong> This system variable specifies the maximum number of table cache instances. MariaDB Server initially creates just a single instance. However, whenever it detects contention on the existing instances, it will automatically create a new instance. When the number of instances has been increased due to contention, it does not decrease again. The default value of this system variable is `8`, which is expected to handle up to 100 CPU cores. If your system is larger than this, then you may benefit from increasing the value of this system variable.
<ul start="1"><li>Depending on the ratio of actual available file handles, and <a undefined>table_open_cache</a> size, the max. instance count may be auto adjusted to a lower value on server startup.
</li><li>The implementation and behavior of this feature is different than the same feature in MySQL 5.6.
</li><li>See [Optimizing table_open_cache: Automatic Creation of New Table Open Cache Instances](/kb/en/optimizing-table_open_cache/#automatic-creation-of-new-table-open-cache-instances) for more information.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8` (&gt;= [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/))
- <strong>Range:</strong> `1` to `64`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `table_type`

- <strong>Description:</strong> Removed and replaced by [storage_engine](#storage_engine). Use [default_storage_engine](#default_storage_engine) instead.

---

#### `tcp_keepalive_interval`

- <strong>Description:</strong> The interval, in seconds, between when successive keep-alive packets are sent if no acknowledgement is received. If set to 0, the system dependent default is used.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--tcp-keepalive-interval=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `2147483`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `tcp_keepalive_probes`

- <strong>Description:</strong> The number of unacknowledged probes to send before considering the connection dead and notifying the application layer. If set to 0, a system dependent default is used.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--tcp-keepalive-probes=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `2147483`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `tcp_keepalive_time`

- <strong>Description:</strong> Timeout, in seconds, with no activity until the first TCP keep-alive packet is sent. If set to 0, a system dependent default is used.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--tcp-keepalive-time=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `2147483`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `tcp_nodelay`

- <strong>Description:</strong> Set the TCP_NODELAY option (disable Nagle's algorithm) on socket.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--tcp-nodelay={0|1}</code>
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `1`
- <strong>Introduced:</strong> [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/)

---

#### `thread_cache_size`

- <strong>Description:</strong> Number of threads server caches for re-use. If this limit hasn't been reached, when a client disconnects, its threads are put into the cache, and re-used where possible. In [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/) and newer the threads are freed after 5 minutes of idle time. Normally this setting has little effect, as the other aspects of the thread implementation are more important, but increasing it can help servers with high volumes of connections per second so that most can use a cached, rather than a new, thread. The cache miss rate can be calculated as the [server status variables](/replication/optimization-and-tuning/system-variables/server-status-variables/) threads_created/connections. If the [thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/) is active, `thread_cache_size` is ignored. If `thread_cache_size` is set to greater than the value of [max_connections](#max_connections), `thread_cache_size` will be set to the [max_connections](#max_connections) value.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--thread-cache-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0` (&lt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/)), 256 (from [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/))
- <strong>Range:</strong> `0` to `16384`

---

#### `thread_concurrency`

- <strong>Description:</strong> Allows applications to give the system a hint about the desired number of threads. Specific to Solaris only, invokes thr_setconcurrency(). Deprecated and has no effect from [MariaDB 5.5](/kb/en/what-is-mariadb-55/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--thread-concurrency=# </code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `1` to `512`
- <strong>Deprecated:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- <strong>Removed:</strong> [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)

---

#### `thread_stack`

- <strong>Description:</strong> Stack size for each thread. If set too small, limits recursion depth of stored procedures and complexity of SQL statements the server can handle in memory. Also affects limits in the crash-me test.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--thread-stack=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong>
<ul start="1"><li>`299008` ([MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/))
</li><li>`297984` ([MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/))
</li><li>`296960` ([MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/))
</li><li>`295936` ([MariaDB 10.1](/kb/en/what-is-mariadb-101/))
</li><li>`294912` (&lt;= [MariaDB 10.0](/kb/en/what-is-mariadb-100/))
</li></ul>
- <strong>Range:</strong> `131072` to `18446744073709551615`

---

#### `time_format`

- <strong>Description:</strong> Unused.

---

#### `time_zone`

- <strong>Description:</strong> The global value determines the default [time zone](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones/) for sessions that connect. The session value determines the session's active [time zone](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones/). When it is set to `SYSTEM`, the session's time zone is determined by the <a undefined>system_time_zone</a> system variable.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--default-time-zone=string</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> `SYSTEM`

---

#### `timed_mutexes`

- <strong>Description:</strong> Determines whether [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) mutexes are timed. `OFF`, the default, disables mutex timing, while `ON` enables it. See also [SHOW ENGINE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine/) for more on mutex statistics. Deprecated and has no effect.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--timed-mutexes</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Deprecated:</strong> [MariaDB 5.5.39](/kb/en/mariadb-5539-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)

---

#### `timestamp`

- <strong>Description:</strong> Sets the time for the client. This will affect the result returned by the [NOW()](/built-in-functions/date-time-functions/now/) function, not the [SYSDATE()](/built-in-functions/date-time-functions/sysdate/) function, unless the server is started with the [--sysdate-is-now](/kb/en/mysqld-options/#-sysdate-is-now) option, in which case SYSDATE becomes an alias of NOW, and will also be affected. Also used to get the original timestamp when restoring rows from the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/).
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Valid Values:</strong> `timestamp_value` (Unix epoch timestamp, not MariaDB timestamp), `DEFAULT`

---

#### `tmp_disk_table_size`

- <strong>Description:</strong> Max size for data for an internal temporary on-disk [MyISAM](/kb/en/myisam/) or [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) table.  These tables are created as part of complex queries when the result doesn't fit into the memory engine. You can set this variable if you want to limit the size of temporary tables created in your temporary directory [tmpdir](#tmpdir).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--tmp-disk-table-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `18446744073709551615` (max unsigned integer, no limit)
- <strong>Range:</strong> `1024` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.2.7](/kb/en/mariadb-1027-release-notes/)

---

#### `tmp_memory_table_size`

- <strong>Description:</strong> An alias for [tmp_table_size](#tmp_table_size).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--tmp-memory-table-size=#</code>
- <strong>Introduced:</strong> [MariaDB 10.2.7](/kb/en/mariadb-1027-release-notes/)

---

#### `tmp_table_size`

- <strong>Description:</strong> The largest size for temporary tables in memory (not [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) tables) although if [max_heap_table_size](#max_heap_table_size) is smaller the lower limit will apply. If a table exceeds the limit, MariaDB converts it to a MyISAM or Aria table. You can see if it's necessary to increase by comparing the [status variables](/replication/optimization-and-tuning/system-variables/server-status-variables/) `Created_tmp_disk_tables` and `Created_tmp_tables` to see how many temporary tables out of the total created needed to be converted to disk. Often complex GROUP BY queries are responsible for exceeding the limit. Defaults may be different on some systems, see for example [Differences in MariaDB in Debian](/kb/en/differences-in-mariadb-in-debian/). From [MariaDB 10.2.7](/kb/en/mariadb-1027-release-notes/), [tmp_memory_table_size](#tmp_memory_table_size) is an alias.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--tmp-table-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `16777216` (16MB)
- <strong>Range:</strong>
<ul start="1"><li>`1024` to `4294967295` (&lt; [MariaDB 10.5](/kb/en/what-is-mariadb-105/))
</li><li>`0` to `4294967295` (&gt;= [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/))
</li></ul>

---

#### `tmpdir`

- <strong>Description:</strong> Directory for storing temporary tables and files. Can specify a list (separated by semicolons in Windows, and colons in Unix) that will then be used in round-robin fashion. This can be used for load balancing across several disks. Note that if the server is a [replication](/replication/) replica, and [slave_load_tmpdir](/kb/en/replication-and-binary-log-system-variables/#slave_load_tmpdir), which overrides `tmpdir` for replicas, is not set, you should not set `tmpdir` to a directory that is cleared when the machine restarts, or else replication may fail.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--tmpdir=path</code> or <code class="fixed" style="white-space:pre-wrap">-t path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> directory name/s
- <strong>Default:</strong>
<ul><li>`$TMPDIR` (environment variable) if set
</li><li>otherwise `$TEMP` if set and on Windows
</li><li>otherwise `$TMP` if set and on Windows
</li><li>otherwise `P_tmpdir` (`"/tmp"`) or `C:\TEMP` (unless overridden during buid time)
</li></ul>

---

#### `transaction_alloc_block_size`

- <strong>Description:</strong> Size in bytes to increase the memory pool available to each transaction when the available pool is not large enough. See [transaction_prealloc_size](#transaction_prealloc_size).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--transaction-alloc-block-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> numeric
- <strong>Default Value:</strong> `8192`
- <strong>Range:</strong> `1024` to `4294967295`
- <strong>Block Size:</strong> `1024`

---

#### `transaction_prealloc_size`

- <strong>Description:</strong> Initial size of a memory pool available to each transaction for various memory allocations. If the memory pool is not large enough for an allocation, it is increased by [transaction_alloc_block_size](#transaction_alloc_block_size) bytes, and truncated back to transaction_prealloc_size bytes when the transaction is completed. If set large enough to contain all statements in a transaction, extra malloc() calls are avoided.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--transaction-prealloc-size=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> numeric
- <strong>Default Value:</strong> `4096`
- <strong>Range:</strong> `1024` to `4294967295`
- <strong>Block Size:</strong> `1024`

---

#### `tx_isolation`

- <strong>Description:</strong> The transaction [isolation level](/kb/en/isolation-level/). See also [SET TRANSACTION ISOLATION LEVEL](/kb/en/set-transaction-isolation-level/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--transaction-isolation=name</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> enumeration
- <strong>Default Value:</strong> `REPEATABLE-READ`
- <strong>Valid Values:</strong> `READ-UNCOMMITTED`, `READ-COMMITTED`, `REPEATABLE-READ`, `SERIALIZABLE`

---

#### `tx_read_only`

- <strong>Description:</strong> Default transaction access mode. If set to `OFF`, the default, access is read/write. If set to `ON`, access is read-only. The `SET TRANSACTION` statement can also change the value of this variable. See [SET TRANSACTION](/sql-statements-structure/sql-statements/transactions/set-transaction/) and [START TRANSACTION](/sql-statements-structure/sql-statements/transactions/start-transaction/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--transaction-read-only=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> boolean
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `unique_checks`

- <strong>Description:</strong> If set to 1, the default, secondary indexes in InnoDB tables are performed. If set to 0, storage engines can (but are not required to) assume that duplicate keys are not present in input data. Set to 0 to speed up imports of large tables to InnoDB. The storage engine will still issue a duplicate key error if it detects one, even if set to 0.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> boolean
- <strong>Default Value:</strong> `1`

---

#### `updatable_views_with_limit`

- <strong>Description:</strong> Determines whether view updates can be made with an UPDATE or DELETE statement with a LIMIT clause if the view does not contain all primary or not null unique key columns from the underlying table. `0` prohibits this, while `1` permits it while issuing a warning (the default).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--updatable-views-with-limit=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> boolean
- <strong>Default Value:</strong> `1`

---

#### `use_stat_tables`

- <strong>Description:</strong> Controls the use of [engine-independent table statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics/). 
<ul start="1"><li>`never`: The optimizer will not use data from statistics tables. 
</li><li>`complementary`: The optimizer uses data from statistics tables if the same kind of data is not provided by the storage engine.
</li><li>`preferably`: Prefer the data from statistics tables, if it's not available there, use the data from the storage engine.
</li><li>`complementary_for_queries`: Same as `complementary`, but for queries only (to avoid needlessly collecting for [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/)). From [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/).
</li><li>`preferably_for_queries`: Same as `preferably`, but for queries only (to avoid needlessly collecting for [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/)). From [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/).
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--use-stat-tables=mode</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `preferably_for_queries` (&gt;= [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/)), `never` (&lt;= [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/))
- <strong>Introduced:</strong> [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/)

---

#### `version`

- <strong>Description:</strong> Server version number. It may also include a suffix with configuration or build information. `-debug` indicates debugging support was enabled on the server, and `-log` indicates at least one of the binary log, general log or [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) are enabled, for example `10.0.1-MariaDB-mariadb1precise-log`. From [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/), this variable can be set at startup in order to fake the server version.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap"> -V</code>, <code class="fixed" style="white-space:pre-wrap">--version[=name]</code> (&gt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/)), <code class="fixed" style="white-space:pre-wrap">--version</code> (&lt;= [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/))
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> string

---

#### `version_comment`

- <strong>Description:</strong> Value of the COMPILATION_COMMENT option specified by CMake when building MariaDB, for example `mariadb.org binary distribution`.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> string

---

#### `version_compile_machine`

- <strong>Description:</strong> The machine type or architecture MariaDB was built on, for example `i686`.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> string

---

#### `version_compile_os`

- <strong>Description:</strong> Operating system that MariaDB was built on, for example `debian-linux-gnu`.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> string

---

#### `version_malloc_library`

- <strong>Description:</strong> Version of the used malloc library.
- <strong>Commandline:</strong> No
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> string
- <strong>Introduced:</strong> [MariaDB 10.0.8](/kb/en/mariadb-1008-release-notes/)

---

#### `version_source_revision`

- <strong>Description:</strong> Source control revision id for MariaDB source code, enabling one to see exactly which version of the source was used for a build.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> string
- <strong>Introduced:</strong> [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/)

---

#### `wait_timeout`

- <strong>Description:</strong> Time in seconds that the server waits for a connection to become active before closing it. The session value is initialized when a thread starts up from either the global value, if the connection is non-interactive, or from the [interactive_timeout](#interactive_timeout) value, if the connection is interactive.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wait-timeout=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> numeric
- <strong>Default Value:</strong> `28800`
- <strong>Range: (Windows): `1` to `2147483`</strong>
- <strong>Range: (Other): `1` to `31536000`</strong>

---

#### `warning_count`

- <strong>Description:</strong> Read-only variable indicating the number of warnings, errors and notes resulting from the most recent statement that generated messages. See [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/) for more. Note warnings will only be recorded if [sql_notes](#sql_notes) is true (the default).
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> numeric

---