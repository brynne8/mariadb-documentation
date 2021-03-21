# GRANT

## Syntax

```sql
GRANT
    priv_type [(column_list)]
      [, priv_type [(column_list)]] ...
    ON [object_type] priv_level
    TO user_specification [ user_options ...]

user_specification:
  username [authentication_option]

authentication_option:
  IDENTIFIED BY 'password' 
  | IDENTIFIED BY PASSWORD 'password_hash'
  | IDENTIFIED {VIA|WITH} authentication_rule [OR authentication_rule  ...]

authentication_rule:
    authentication_plugin
  | authentication_plugin {USING|AS} 'authentication_string'
  | authentication_plugin {USING|AS} PASSWORD('password')

GRANT PROXY ON username
    TO username [, username] ...
    [WITH GRANT OPTION]

user_options:
    [REQUIRE {NONE | tls_option [[AND] tls_option] ...}]
    [WITH with_option [with_option] ...]

object_type:
    TABLE
  | FUNCTION
  | PROCEDURE

priv_level:
    *
  | *.*
  | db_name.*
  | db_name.tbl_name
  | tbl_name
  | db_name.routine_name

with_option:
    GRANT OPTION
  | resource_option

resource_option:
  MAX_QUERIES_PER_HOUR count
  | MAX_UPDATES_PER_HOUR count
  | MAX_CONNECTIONS_PER_HOUR count
  | MAX_USER_CONNECTIONS count
  | MAX_STATEMENT_TIME time

tls_option:
  SSL 
  | X509
  | CIPHER 'cipher'
  | ISSUER 'issuer'
  | SUBJECT 'subject'
```

## Description

The `GRANT` statement allows you to grant privileges or [roles](#roles) to accounts. To use `GRANT`, you must have the `GRANT OPTION` privilege, and you must have the privileges that you are granting.

Use the [REVOKE](/sql-statements-structure/sql-statements/account-management-sql-commands/revoke/) statement to revoke privileges granted with the `GRANT` statement.

Use the [SHOW GRANTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-grants/) statement to determine what privileges an account has.

## Account Names

For `GRANT` statements, account names are specified as the `username` argument in the same way as they are for [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/) statements. See [account names](/kb/en/create-user/#account-names) from the `CREATE USER` page for details on how account names are specified.

## Implicit Account Creation

The `GRANT` statement also allows you to implicitly create accounts in some cases.

If the account does not yet exist, then `GRANT` can implicitly create it. To implicitly create an account with `GRANT`, a user is required to have the same privileges that would be required to explicitly create the account with the `CREATE USER` statement.

If the `NO_AUTO_CREATE_USER` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) is set, then accounts can only be created if authentication information is specified, or with a [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/) statement. If no authentication information is provided, `GRANT` will produce an error when the specified account does not exist, for example:

```sql
show variables like '%sql_mode%' ;
+---------------+--------------------------------------------+
| Variable_name | Value                                      |
+---------------+--------------------------------------------+
| sql_mode      | NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION |
+---------------+--------------------------------------------+

GRANT USAGE ON *.* TO 'user123'@'%' IDENTIFIED BY '';
ERROR 1133 (28000): Can't find any matching row in the user table

GRANT USAGE ON *.* TO 'user123'@'%' IDENTIFIED VIA PAM using 'mariadb' require ssl ;
Query OK, 0 rows affected (0.00 sec)
 
select host, user from mysql.user where user='user123' ;

+------+----------+
| host | user     |
+------+----------+
| %    | user123 |
+------+----------+
```

## Privilege Levels

Privileges can be set globally, for an entire database, for a table or routine,
or for individual columns in a table. Certain privileges can only be set at
certain levels.

- [Global privileges <em>priv_type</em>](#global-privileges) are granted using `*.*` for
<em>priv_level</em>. Global privileges include privileges to administer the database
and manage user accounts, as well as privileges for all tables, functions, and
procedures. Global privileges are stored in the [mysql.user table](/kb/en/mysqluser-table/).
- [Database privileges <em>priv_type</em>](#database-privileges) are granted using `db_name.*`
for <em>priv_level</em>, or using just `*` to use default database. Database
privileges include privileges to create tables and functions, as well as
privileges for all tables, functions, and procedures in the database. Database privileges are stored in the [mysql.db table](/kb/en/mysqldb-table/).
- [Table privileges <em>priv_type</em>](#table-privileges) are granted using `db_name.tbl_name`
for <em>priv_level</em>, or using just `tbl_name` to specify a table in the default
database. The `TABLE` keyword is optional. Table privileges include the
ability to select and change data in the table. Certain table privileges can
be granted for individual columns.
- [Column privileges <em>priv_type</em>](#column-privileges) are granted by specifying a table for
<em>priv_level</em> and providing a column list after the privilege type. They allow
you to control exactly which columns in a table users can select and change.
- [Function privileges <em>priv_type</em>](#function-privileges) are granted using `FUNCTION db_name.routine_name`
for <em>priv_level</em>, or using just  `FUNCTION routine_name` to specify a function
in the default database.
- [Procedure privileges <em>priv_type</em>](#procedure-privileges) are granted using `PROCEDURE db_name.routine_name`
for <em>priv_level</em>, or using just `PROCEDURE routine_name` to specify a procedure
in the default database.

### The `USAGE` Privilege

The `USAGE` privilege grants no real privileges. The [SHOW GRANTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-grants/)
statement will show a global `USAGE` privilege for a newly-created user. You
can use `USAGE` with the `GRANT` statement to change options like `GRANT OPTION`
and `MAX_USER_CONNECTIONS` without changing any account privileges.

### The `ALL PRIVILEGES` Privilege

The `ALL PRIVILEGES` privilege grants all available privileges. Granting all
privileges only affects the given privilege level. For example, granting all
privileges on a table does not grant any privileges on the database or globally.

Using `ALL PRIVILEGES` does not grant the special `GRANT OPTION` privilege.

You can use `ALL` instead of `ALL PRIVILEGES`.

### The `GRANT OPTION` Privilege

Use the `WITH GRANT OPTION` clause to give users the ability to grant privileges
to other users at the given privilege level. Users with the `GRANT OPTION` privilege can
only grant privileges they have. They cannot grant privileges at a higher privilege level than
they have the `GRANT OPTION` privilege.

The `GRANT OPTION` privilege cannot be set for individual columns.
If you use `WITH GRANT OPTION` when specifying [column privileges](#column-privileges),
the `GRANT OPTION` privilege will be granted for the entire table.

Using the `WITH GRANT OPTION` clause is equivalent to listing `GRANT OPTION`
as a privilege.

### Global Privileges

The following table lists the privileges that can be granted globally. You can
also grant all database, table, and function privileges globally. When granted
globally, these privileges apply to all databases, tables, or functions,
including those created later.

To set a global privilege, use <code class="fixed" style="white-space:pre-wrap">*.*</code> for <em>priv_level</em>.

#### BINLOG ADMIN

Enables administration of the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/), including the [PURGE BINARY LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/purge-binary-logs/) statement and setting the [binlog_annotate_row_events](/kb/en/replication-and-binary-log-system-variables/#binlog_annotate_row_events), [binlog_cache_size](/kb/en/replication-and-binary-log-system-variables/#binlog_cache_size), [binlog_commit_wait_count](/kb/en/replication-and-binary-log-system-variables/#binlog_commit_wait_count), [binlog_commit_wait_usec](/kb/en/replication-and-binary-log-system-variables/#binlog_commit_wait_usec), [binlog_direct_non_transactional_updates](/kb/en/replication-and-binary-log-system-variables/#binlog_direct_non_transactional_updates), [binlog_file_cache_size](/kb/en/replication-and-binary-log-system-variables/#binlog_file_cache_size), [binlog_format](/kb/en/replication-and-binary-log-system-variables/#binlog_format), [binlog_row_image](/kb/en/replication-and-binary-log-system-variables/#binlog_row_image), [binlog_row_metadata](/kb/en/replication-and-binary-log-system-variables/#binlog_row_metadata), [binlog_stmt_cache_size](/kb/en/replication-and-binary-log-system-variables/#binlog_stmt_cache_size), [expire_logs_days](/kb/en/replication-and-binary-log-system-variables/#expire_logs_days), [log_bin_compress](/kb/en/replication-and-binary-log-system-variables/#log_bin_compress), [log_bin_compress_min_len](/kb/en/replication-and-binary-log-system-variables/#log_bin_compress_min_len), [log_bin_trust_function_creators](/kb/en/replication-and-binary-log-system-variables/#log_bin_trust_function_creators), [max_binlog_cache_size](/kb/en/replication-and-binary-log-system-variables/#max_binlog_cache_size), [max_binlog_size](/kb/en/replication-and-binary-log-system-variables/#max_binlog_size), [max_binlog_stmt_cache_size](/kb/en/replication-and-binary-log-system-variables/#max_binlog_stmt_cache_size), [sql_log_bin](/kb/en/replication-and-binary-log-system-variables/#sql_log_bin) and [sync_binlog](/kb/en/replication-and-binary-log-system-variables/#sync_binlog) system variables. Added in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

#### BINLOG MONITOR

New name for [REPLICATION CLIENT](#replication-client) from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), (`REPLICATION CLIENT` still supported as an alias for compatibility purposes). Permits running SHOW commands related to the [binary log](binary_log), in particular the [SHOW BINLOG STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-binlog-status/), [SHOW REPLICA STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-replica-status/) and [SHOW BINARY LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-binary-logs/) statements.

#### BINLOG REPLAY

Enables replaying the binary log with the [BINLOG](/sql-statements-structure/sql-statements/administrative-sql-statements/binlog/) statement (generated by [mariadb-binlog](/clients-utilities/mysqlbinlog/)), executing [SET timestamp](/kb/en/server-system-variables/#timestamp) when [secure_timestamp](/kb/en/server-system-variables/#secure_timestamp) is set to `replication`, and setting the session values of system variables usually included in BINLOG output, in particular [gtid_domain_id](/kb/en/gtid/#gtid_domain_id), [gtid_seq_no](/kb/en/gtid/#gtid_seq_no), [pseudo_thread_id](/kb/en/server-system-variables/#pseudo_thread_id) and [server_id](/kb/en/replication-and-binary-log-system-variables/#server_id). Added in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)

#### CONNECTION ADMIN

Enables administering connection resource limit options. This includes ignoring the limits specified by [max_connections](/kb/en/server-system-variables/#max_connections), [max_user_connections](/kb/en/server-system-variables/#max_user_connections) and [max_password_errors](/kb/en/server-system-variables/#max_password_errors), not executing the statements specified in [init_connect](/kb/en/server-system-variables/#init_connect), [killing connections and queries](/sql-statements-structure/sql-statements/administrative-sql-statements/kill/) owned by other users as well as setting the following connection-related system variables: [connect_timeout](/kb/en/server-system-variables/#connect_timeout), [disconnect_on_expired_password](/kb/en/server-system-variables/#disconnect_on_expired_password), [extra_max_connections](/kb/en/thread-pool-system-status-variables/#extra_max_connections), [init_connect](/kb/en/server-system-variables/#init_connect), [max_connections](/kb/en/server-system-variables/#max_connections), 
[max_connect_errors](/kb/en/server-system-variables/#max_connect_errors), [max_password_errors](/kb/en/server-system-variables/#max_password_errors), [proxy_protocol_networks](/kb/en/server-system-variables/#proxy_protocol_networks), [secure_auth](/kb/en/server-system-variables/#secure_auth), [slow_launch_time](/kb/en/server-system-variables/#slow_launch_time), [thread_pool_exact_stats](/kb/en/thread-pool-system-status-variables/#thread_pool_exact_stats), [thread_pool_dedicated_listener](/kb/en/thread-pool-system-status-variables/#thread_pool_dedicated_listener), [thread_pool_idle_timeout](/kb/en/thread-pool-system-status-variables/#thread_pool_idle_timeout), [thread_pool_max_threads](/kb/en/thread-pool-system-status-variables/#thread_pool_max_threads), [thread_pool_min_threads](/kb/en/thread-pool-system-status-variables/#thread_pool_min_threads), thread_pool_mode, [thread_pool_oversubscribe](/kb/en/thread-pool-system-status-variables/#thread_pool_oversubscribe), [thread_pool_prio_kickup_timer](/kb/en/thread-pool-system-status-variables/#thread_pool_prio_kickup_timer), [thread_pool_priority](/kb/en/thread-pool-system-status-variables/#thread_pool_priority), [thread_pool_size](/kb/en/thread-pool-system-status-variables/#thread_pool_size), [thread_pool_stall_limit](/kb/en/thread-pool-system-status-variables/#thread_pool_stall_limit). Added in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

#### CREATE USER

Create a user using the [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/) statement, or implicitly create a user with the `GRANT` statement.

#### FEDERATED ADMIN

Execute [CREATE SERVER](/sql-statements-structure/sql-statements/data-definition/create/create-server/), [ALTER SERVER](/sql-statements-structure/sql-statements/data-definition/alter/alter-server/), and [DROP SERVER](/sql-statements-structure/sql-statements/data-definition/drop/drop-server/) statements. Added in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

#### FILE

Read and write files on the server, using statements like [LOAD DATA INFILE](/kb/en/load-data-infile/) or functions like [LOAD_FILE()](/built-in-functions/string-functions/load_file/). Also needed to create [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) outward tables. MariaDB server must have the permissions to access those files.

#### GRANT OPTION

Grant global privileges. You can only grant privileges that you have.

#### PROCESS

Show information about the active processes, for example via [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) or [mysqladmin processlist](/clients-utilities/mysqladmin/).

#### READ_ONLY ADMIN

User can set the [read_only](/kb/en/server-system-variables/#read_only) system variable and allows the user to perform write operations, even when the `read_only` option is active. Added in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

#### RELOAD

Execute [FLUSH](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) statements or equivalent [mariadb-admin/mysqladmin](/clients-utilities/mysqladmin/) commands.

#### REPLICATION CLIENT

Execute [SHOW MASTER STATUS](/kb/en/show-master-status/), [SHOW SLAVE STATUS](/kb/en/show-slave-status/) and [SHOW BINARY LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-binary-logs/) informative statements. Renamed to [BINLOG MONITOR](binlog-admin) in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/) (but still supported as an alias for compatibility reasons).

#### REPLICATION MASTER ADMIN

Permits administration of primary servers, including the [SHOW REPLICA HOSTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-replica-hosts/) statement, and setting the [gtid_binlog_state](/kb/en/gtid/#gtid_binlog_state), [gtid_domain_id](/kb/en/gtid/#gtid_domain_id), [master_verify_checksum](/kb/en/replication-and-binary-log-system-variables/#master_verify_checksum) and [server_id](/kb/en/replication-and-binary-log-system-variables/#server_id) system variables. Added in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

#### REPLICA MONITOR

Permit [SHOW REPLICA STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-replica-status/) and [SHOW RELAYLOG EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-relaylog-events/). From [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/).

When a user would upgrade from an older major release to a [MariaDB 10.5](/kb/en/what-is-mariadb-105/) minor release prior to [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/), certain user accounts would lose capabilities. For example, a user account that had the REPLICATION CLIENT privilege in older major releases could run [SHOW REPLICA STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-replica-status/), but after upgrading to a [MariaDB 10.5](/kb/en/what-is-mariadb-105/) minor release prior to [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/), they could no longer run [SHOW REPLICA STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-replica-status/), because that statement was changed to require the REPLICATION REPLICA ADMIN privilege.

This issue is fixed in [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/) with this new privilege, which now grants the user the ability to execute SHOW [ALL] (SLAVE | REPLICA) STATUS.

When a database is upgraded from an older major release to MariaDB Server 10.5.9 or later, any user accounts with the REPLICATION CLIENT or REPLICATION SLAVE privileges will automatically be granted the new REPLICA MONITOR privilege. The privilege fix occurs when the server is started up, not when mariadb-upgrade is performed.

However, when a database is upgraded from an early 10.5 minor release to 10.5.9 and later, the user will have to fix any user account privileges manually.

#### REPLICATION REPLICA

Synonym for [REPLICATION SLAVE](#replication-slave). From [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/).

#### REPLICATION SLAVE

Accounts used by replica servers on the primary need this privilege. This is needed to get the updates made on the master. From [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/), [REPLICATION REPLICA](#replication-replica) is an alias for `REPLICATION SLAVE`.

#### REPLICATION SLAVE ADMIN

Permits administering replica servers, including [START REPLICA/SLAVE](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/start-replica/), [STOP REPLICA/SLAVE](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/stop-replica/), [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/), [SHOW REPLICA/SLAVE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-replica-status/), [SHOW RELAYLOG EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-relaylog-events/) statements, replaying the binary log with the [BINLOG](/sql-statements-structure/sql-statements/administrative-sql-statements/binlog/) statement (generated by [mariadb-binlog](/clients-utilities/mysqlbinlog/)), and setting the [gtid_cleanup_batch_size](/kb/en/gtid/#gtid_cleanup_batch_size), [gtid_ignore_duplicates](/kb/en/gtid/#gtid_ignore_duplicates), [gtid_pos_auto_engines](/kb/en/gtid/#gtid_pos_auto_engines), [gtid_slave_pos](/kb/en/gtid/#gtid_slave_pos), [gtid_strict_mode](/kb/en/gtid/#gtid_strict_mode), [init_slave](/kb/en/replication-and-binary-log-system-variables/#init_slave), [read_binlog_speed_limit](/kb/en/replication-and-binary-log-system-variables/#read_binlog_speed_limit), [relay_log_purge](/kb/en/replication-and-binary-log-system-variables/#relay_log_purge), [relay_log_recovery](/kb/en/replication-and-binary-log-system-variables/#relay_log_recovery), [replicate_do_db](/kb/en/replication-and-binary-log-system-variables/#replicate_do_db), [replicate_do_table](/kb/en/replication-and-binary-log-system-variables/#replicate_do_table), [replicate_events_marked_for_skip](/kb/en/replication-and-binary-log-system-variables/#replicate_events_marked_for_skip), [replicate_ignore_db](/kb/en/replication-and-binary-log-system-variables/#replicate_ignore_db), [replicate_ignore_table](/kb/en/replication-and-binary-log-system-variables/#replicate_ignore_table), [replicate_wild_do_table](/kb/en/replication-and-binary-log-system-variables/#replicate_wild_do_table), [replicate_wild_ignore_table](/kb/en/replication-and-binary-log-system-variables/#replicate_wild_ignore_table), [slave_compressed_protocol](/kb/en/replication-and-binary-log-system-variables/#slave_compressed_protocol), [slave_ddl_exec_mode](/kb/en/replication-and-binary-log-system-variables/#slave_ddl_exec_mode), [slave_domain_parallel_threads](/kb/en/replication-and-binary-log-system-variables/#slave_domain_parallel_threads), [slave_exec_mode](/kb/en/replication-and-binary-log-system-variables/#slave_exec_mode), [slave_max_allowed_packet](/kb/en/replication-and-binary-log-system-variables/#slave_max_allowed_packet), [slave_net_timeout](/kb/en/replication-and-binary-log-system-variables/#slave_net_timeout), [slave_parallel_max_queued](/kb/en/replication-and-binary-log-system-variables/#slave_parallel_max_queued), [slave_parallel_mode](/kb/en/replication-and-binary-log-system-variables/#slave_parallel_mode), [slave_parallel_threads](/kb/en/replication-and-binary-log-system-variables/#slave_parallel_threads), [slave_parallel_workers](/kb/en/replication-and-binary-log-system-variables/#slave_parallel_workers), [slave_run_triggers_for_rbr](/kb/en/replication-and-binary-log-system-variables/#slave_run_triggers_for_rbr), [slave_sql_verify_checksum](/kb/en/replication-and-binary-log-system-variables/#slave_sql_verify_checksum), [slave_transaction_retry_interval](/kb/en/replication-and-binary-log-system-variables/#slave_transaction_retry_interval), [slave_type_conversions](/kb/en/replication-and-binary-log-system-variables/#slave_type_conversions), [sync_master_info](/kb/en/replication-and-binary-log-system-variables/#sync_master_info), [sync_relay_log](/kb/en/replication-and-binary-log-system-variables/#sync_relay_log) and [sync_relay_log_info](/kb/en/replication-and-binary-log-system-variables/#sync_relay_log_info) system variables. Added in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

#### SET USER

Enables setting the `DEFINER` when creating [triggers](/programming-customizing-mariadb/triggers-events/triggers/), [views](/programming-customizing-mariadb/views/), [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions/) and [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/). Added in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

#### SHOW DATABASES

List all databases using the [SHOW DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases/) statement. Without the `SHOW DATABASES` privilege, you can still issue the `SHOW DATABASES` statement, but it will only list databases containing tables on which you have privileges.

#### SHUTDOWN

Shut down the server using [SHUTDOWN](/sql-statements-structure/sql-statements/administrative-sql-statements/shutdown/) or the [mysqladmin shutdown](/clients-utilities/mysqladmin/) command.

#### SUPER

Execute superuser statements: [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/), [KILL](/kb/en/data-manipulation-kill-connection-query/) (users who do not have this privilege can only `KILL` their own threads), [PURGE LOGS](/kb/en/sql-commands-purge-logs/), [SET global system variables](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/), or the [mysqladmin debug](/clients-utilities/mysqladmin/) command. Also, this permission allows the user to write data even if the [read_only](/kb/en/server-system-variables/#read_only) startup option is set, enable or disable logging, enable or disable replication on replica, specify a `DEFINER` for statements that support that clause, connect once after reaching the `MAX_CONNECTIONS`. If a statement has been specified for the [init-connect](/kb/en/server-system-variables/#init_connect) `mysqld` option, that command will not be executed when a user with `SUPER` privileges connects to the server.

The SUPER privilege has been split into multiple smaller privileges from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/) to allow for more fine-grained privileges, although it remains an alias for these smaller privileges.

### Database Privileges

The following table lists the privileges that can be granted at the database
level. You can also grant all table and function privileges at the database
level. Table and function privileges on a database apply to all tables or
functions in that database, including those created later.

To set a privilege for a database, specify the database using
<code class="fixed" style="white-space:pre-wrap">db_name.*</code> for <em>priv_level</em>, or just use <code class="fixed" style="white-space:pre-wrap">*</code>
to specify the default database.

<table><tbody><tr><th>Privilege</th><th>Description</th></tr>
<tr><td><code>CREATE</code></td><td>Create a database using the <a href="/kb/en/create-database/">CREATE DATABASE</a> statement, when the privilege is granted for a database. You can grant the <code>CREATE</code> privilege on databases that do not yet exist. This also grants the <code>CREATE</code> privilege on all tables in the database.</td></tr>
<tr><td><code>CREATE ROUTINE</code></td><td>Create Stored Programs using the <a href="/kb/en/create-procedure/">CREATE PROCEDURE</a> and <a href="/kb/en/create-function/">CREATE FUNCTION</a> statements.</td></tr>
<tr><td><code>CREATE TEMPORARY TABLES</code></td><td>Create temporary tables with the <a href="/kb/en/create-table/">CREATE TEMPORARY TABLE</a> statement. This privilege enable writing and dropping those temporary tables</td></tr>
<tr><td><code>DROP</code></td><td>Drop a database using the <a href="/kb/en/drop-database/">DROP DATABASE</a> statement, when the privilege is granted for a database. This also grants the <code>DROP</code> privilege on all tables in the database.</td></tr>
<tr><td><code>EVENT</code></td><td>Create, drop and alter <code>EVENT</code>s.</td></tr>
<tr><td><code>GRANT OPTION</code></td><td>Grant database privileges. You can only grant privileges that you have.</td></tr>
<tr><td><code>LOCK TABLES</code></td><td>Acquire explicit locks using the <a href="/kb/en/transactions-lock/">LOCK TABLES</a> statement; you also need to have the <code>SELECT</code> privilege on a table, in order to lock it.</td></tr>
</tbody></table>

### Table Privileges

<table><tbody><tr><th>Privilege</th><th>Description</th></tr>
<tr><td><code>ALTER</code></td><td>Change the structure of an existing table using the <a href="/kb/en/alter-table/">ALTER TABLE</a> statement.</td></tr>
<tr><td><code>CREATE</code></td><td>Create a table using the <a href="/kb/en/create-table/">CREATE TABLE</a> statement.  You can grant the <code>CREATE</code> privilege on tables that do not yet exist.</td></tr>
<tr><td><code>CREATE VIEW</code></td><td>Create a view using the <a href="/kb/en/create-view/">CREATE_VIEW</a> statement.</td></tr>
<tr><td><code>DELETE</code></td><td>Remove rows from a table using the <a href="/kb/en/delete/">DELETE</a> statement.</td></tr>
<tr><td><code>DELETE HISTORY</code></td><td>Remove <a href="/kb/en/system-versioned-tables/">historical rows</a> from a table using the <a href="/kb/en/delete/">DELETE HISTORY</a> statement. Displays as <code>DELETE VERSIONING ROWS</code> when running <a href="/kb/en/show-grants/">SHOW GRANTS</a> until <a href="/kb/en/mariadb-10315-release-notes/">MariaDB 10.3.15</a> and until <a href="/kb/en/mariadb-1045-release-notes/">MariaDB 10.4.5</a> (<a href="https://jira.mariadb.org/browse/MDEV-17655">MDEV-17655</a>), or when running SHOW PRIVILEGES until <a href="/kb/en/mariadb-1052-release-notes/">MariaDB 10.5.2</a>, <a href="/kb/en/mariadb-10413-release-notes/">MariaDB 10.4.13</a> and <a href="/kb/en/mariadb-10323-release-notes/">MariaDB 10.3.23</a> (<a href="https://jira.mariadb.org/browse/MDEV-20382">MDEV-20382</a>). From <a href="/kb/en/mariadb-1034-release-notes/">MariaDB 10.3.4</a>. From <a href="/kb/en/mariadb-1035-release-notes/">MariaDB 10.3.5</a>, if a user has the <code>SUPER</code> privilege but not this privilege, running <a href="/kb/en/mysql_upgrade/">mysql_upgrade</a> will grant this privilege as well.</td></tr>
<tr><td><code>DROP</code></td><td>Drop a table using the <a href="/kb/en/drop-table/">DROP TABLE</a> statement or a view using the <a href="/kb/en/drop-view/">DROP VIEW</a> statement. Also required to execute the <a href="/kb/en/truncate-table/">TRUNCATE TABLE</a> statement.</td></tr>
<tr><td><code>GRANT OPTION</code></td><td>Grant table privileges. You can only grant privileges that you have.</td></tr>
<tr><td><code>INDEX</code></td><td>Create an index on a table using the <a href="/kb/en/create-index/">CREATE INDEX</a> statement. Without the <code>INDEX</code> privilege, you can still create indexes when creating a table using the <a href="/kb/en/create-table/">CREATE TABLE</a> statement if the you have the <code>CREATE</code> privilege, and you can create indexes using the <a href="/kb/en/alter-table/">ALTER TABLE</a> statement if you have the <code>ALTER</code> privilege.</td></tr>
<tr><td><code>INSERT</code></td><td>Add rows to a table using the <a href="/kb/en/insert/">INSERT</a> statement.  The <code>INSERT</code> privilege can also be set on individual columns; see <a href="#column-privileges">Column Privileges</a> below for details.</td></tr>
<tr><td><code>REFERENCES</code></td><td>Unused.</td></tr>
<tr><td><code>SELECT</code></td><td>Read data from a table using the <a href="/kb/en/select/">SELECT</a> statement. The <code>SELECT</code> privilege can also be set on individual columns; see <a href="#column-privileges">Column Privileges</a> below for details.</td></tr>
<tr><td><code>SHOW VIEW</code></td><td>Show the <a href="/kb/en/create-view/">CREATE VIEW</a> statement to create a view using the <a href="/kb/en/show-create-view/">SHOW CREATE VIEW</a> statement.</td></tr>
<tr><td><code>TRIGGER</code></td><td>Execute triggers associated to tables you update, execute the <a href="/kb/en/create-trigger/">CREATE TRIGGER</a> and <a href="/kb/en/drop-trigger/">DROP TRIGGER</a> statements. You will still be able to see triggers.</td></tr>
<tr><td><code>UPDATE</code></td><td>Update existing rows in a table using the <a href="/kb/en/update/">UPDATE</a> statement. <code>UPDATE</code> statements usually include a <code>WHERE</code> clause to update only certain rows. You must have <code>SELECT</code> privileges on the table or the appropriate columns for the <code>WHERE</code> clause. The <code>UPDATE</code> privilege can also be set on individual columns; see <a href="#column-privileges">Column Privileges</a> below for details.</td></tr>
</tbody></table>

### Column Privileges

Some table privileges can be set for individual columns of a table. To use
column privileges, specify the table explicitly and provide a list of column
names after the privilege type. For example, the following statement would allow
the user to read the names and positions of employees, but not other information
from the same table, such as salaries.

```sql
GRANT SELECT (name, position) on Employee to 'jeffrey'@'localhost';
```

<table><tbody><tr><th>Privilege</th><th>Description</th></tr>
<tr><td><code>INSERT (column_list)</code></td><td>Add rows specifying values in columns using the <a href="/kb/en/insert/">INSERT</a> statement. If you only have column-level <code>INSERT</code> privileges, you must specify the columns you are setting in the <code>INSERT</code> statement. All other columns will be set to their default values, or <code>NULL</code>.</td></tr>
<tr><td><code>REFERENCES (column_list)</code></td><td>Unused.</td></tr>
<tr><td><code>SELECT (column_list)</code></td><td>Read values in columns using the <a href="/kb/en/select/">SELECT</a> statement. You cannot access or query any columns for which you do not have <code>SELECT</code> privileges, including in <code>WHERE</code>, <code>ON</code>, <code>GROUP BY</code>, and <code>ORDER BY</code> clauses.</td></tr>
<tr><td><code>UPDATE (column_list)</code></td><td>Update values in columns of existing rows using the <a href="/kb/en/update/">UPDATE</a> statement. <code>UPDATE</code> statements usually include a <code>WHERE</code> clause to update only certain rows. You must have <code>SELECT</code> privileges on the table or the appropriate columns for the <code>WHERE</code> clause.</td></tr>
</tbody></table>

### Function Privileges

<table><tbody><tr><th>Privilege</th><th>Description</th></tr>
<tr><td><code>ALTER ROUTINE</code></td><td>Change the characteristics of a stored function using the <a href="/kb/en/alter-function/">ALTER FUNCTION</a> statement.</td></tr>
<tr><td><code>EXECUTE</code></td><td>Use a stored function. You need <code>SELECT</code> privileges for any tables or columns accessed by the function.</td></tr>
<tr><td><code>GRANT OPTION</code></td><td>Grant function privileges. You can only grant privileges that you have.</td></tr>
</tbody></table>

### Procedure Privileges

<table><tbody><tr><th>Privilege</th><th>Description</th></tr>
<tr><td><code>ALTER ROUTINE</code></td><td>Change the characteristics of a stored procedure using the <a href="/kb/en/alter-procedure/">ALTER PROCEDURE</a> statement.</td></tr>
<tr><td><code>EXECUTE</code></td><td>Execute a <a href="/kb/en/stored-procedures/">stored procedure</a> using the <a href="/kb/en/call/">CALL</a> statement. The privilege to call a procedure may allow you to perform actions you wouldn't otherwise be able to do, such as insert rows into a table.</td></tr>
<tr><td><code>GRANT OPTION</code></td><td>Grant procedure privileges. You can only grant privileges that you have.</td></tr>
</tbody></table>

### Proxy Privileges

<table><tbody><tr><th>Privilege</th><th>Description</th></tr>
<tr><td><code>PROXY</code></td><td>Permits one user to be a proxy for another.</td></tr>
</tbody></table>

The `PROXY` privilege allows one user to proxy as another user, which means their privileges change to that of the proxy user, and the [CURRENT_USER()](/built-in-functions/secondary-functions/information-functions/current_user/) function returns the user name of the proxy user.

The `PROXY` privilege only works with authentication plugins that support it. The default [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password/) authentication plugin does not support proxy users.

The [pam](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam/) authentication plugin is the only plugin included with MariaDB that currently supports proxy users. The `PROXY` privilege is commonly used with the [pam](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam/) authentication plugin to enable [user and group mapping with PAM](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/user-and-group-mapping-with-pam/).

For example, to grant the `PROXY` privilege to an [anonymous account](/kb/en/create-user/#anonymous-accounts) that authenticates with the [pam](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam/) authentication plugin, you could execute the following:

```sql
CREATE USER 'dba'@'%' IDENTIFIED BY 'strongpassword';
GRANT ALL PRIVILEGES ON *.* TO 'dba'@'%' ;

CREATE USER ''@'%' IDENTIFIED VIA pam USING 'mariadb';
GRANT PROXY ON 'dba'@'%' TO ''@'%';
```

A user account can only grant the `PROXY` privilege for a specific user account if the granter also has the `PROXY` privilege for that specific user account, and if that privilege is defined `WITH GRANT OPTION`. For example, the following example fails because the granter does not have the `PROXY` privilege for that specific user account at all:

```sql
SELECT USER(), CURRENT_USER();
+-----------------+-----------------+
| USER()          | CURRENT_USER()  |
+-----------------+-----------------+
| alice@localhost | alice@localhost |
+-----------------+-----------------+

SHOW GRANTS;
+-----------------------------------------------------------------------------------------------------------------------+
| Grants for alice@localhost                                                                                            |
+-----------------------------------------------------------------------------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO 'alice'@'localhost' IDENTIFIED BY PASSWORD '*2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19' |
+-----------------------------------------------------------------------------------------------------------------------+

GRANT PROXY ON 'dba'@'localhost' TO 'bob'@'localhost';
ERROR 1698 (28000): Access denied for user 'alice'@'localhost'
```

And the following example fails because the granter does have the `PROXY` privilege for that specific user account, but it is not defined `WITH GRANT OPTION`:

```sql
SELECT USER(), CURRENT_USER();
+-----------------+-----------------+
| USER()          | CURRENT_USER()  |
+-----------------+-----------------+
| alice@localhost | alice@localhost |
+-----------------+-----------------+

SHOW GRANTS;
+-----------------------------------------------------------------------------------------------------------------------+
| Grants for alice@localhost                                                                                            |
+-----------------------------------------------------------------------------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO 'alice'@'localhost' IDENTIFIED BY PASSWORD '*2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19' |
| GRANT PROXY ON 'dba'@'localhost' TO 'alice'@'localhost'                                                               |
+-----------------------------------------------------------------------------------------------------------------------+

GRANT PROXY ON 'dba'@'localhost' TO 'bob'@'localhost';
ERROR 1698 (28000): Access denied for user 'alice'@'localhost'
```

But the following example succeeds because the granter does have the `PROXY` privilege for that specific user account, and it is defined `WITH GRANT OPTION`:

```sql
SELECT USER(), CURRENT_USER();
+-----------------+-----------------+
| USER()          | CURRENT_USER()  |
+-----------------+-----------------+
| alice@localhost | alice@localhost |
+-----------------+-----------------+

SHOW GRANTS;
+-----------------------------------------------------------------------------------------------------------------------------------------+
| Grants for alice@localhost                                                                                                              |
+-----------------------------------------------------------------------------------------------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO 'alice'@'localhost' IDENTIFIED BY PASSWORD '*2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19' WITH GRANT OPTION |
| GRANT PROXY ON 'dba'@'localhost' TO 'alice'@'localhost' WITH GRANT OPTION                                                               |
+-----------------------------------------------------------------------------------------------------------------------------------------+

GRANT PROXY ON 'dba'@'localhost' TO 'bob'@'localhost';
```

A user account can grant the `PROXY` privilege for any other user account if the granter has the `PROXY` privilege for the `''@'%'` anonymous user account, like this:

```sql
GRANT PROXY ON ''@'%' TO 'dba'@'localhost' WITH GRANT OPTION;
```

For example, the following example succeeds because the user can grant the `PROXY` privilege for any other user account:

```sql
SELECT USER(), CURRENT_USER();
+-----------------+-----------------+
| USER()          | CURRENT_USER()  |
+-----------------+-----------------+
| alice@localhost | alice@localhost |
+-----------------+-----------------+

SHOW GRANTS;
+-----------------------------------------------------------------------------------------------------------------------------------------+
| Grants for alice@localhost                                                                                                              |
+-----------------------------------------------------------------------------------------------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO 'alice'@'localhost' IDENTIFIED BY PASSWORD '*2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19' WITH GRANT OPTION |
| GRANT PROXY ON ''@'%' TO 'alice'@'localhost' WITH GRANT OPTION                                                                          |
+-----------------------------------------------------------------------------------------------------------------------------------------+

GRANT PROXY ON 'app1_dba'@'localhost' TO 'bob'@'localhost';
Query OK, 0 rows affected (0.004 sec)

GRANT PROXY ON 'app2_dba'@'localhost' TO 'carol'@'localhost';
Query OK, 0 rows affected (0.004 sec)
```

The default `root` user accounts created by [mysql_install_db](/clients-utilities/mysql_install_db/) have this privilege. For example:

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
GRANT PROXY ON ''@'%' TO 'root'@'localhost' WITH GRANT OPTION;
```

This allows the default `root` user accounts to grant the `PROXY` privilege for any other user account, and it also allows the default `root` user accounts to grant others the privilege to do the same.

## Authentication Options

The authentication options for the `GRANT` statement are the same as those for the [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/) statement.

### IDENTIFIED BY 'password'

The optional `IDENTIFIED BY` clause can be used to provide an account with a password. The password should be specified in plain text. It will be hashed by the [PASSWORD](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/) function prior to being stored to the [mysql.user](/kb/en/mysqluser-table/) table.

For example, if our password is `mariadb`, then we can create the user with:

```sql
GRANT USAGE ON *.* TO foo2@test IDENTIFIED BY 'mariadb';
```

If you do not specify a password with the `IDENTIFIED BY` clause, the user
will be able to connect without a password. A blank password is not a wildcard
to match any password. The user must connect without providing a password if no
password is set.

If the user account already exists and if you provide the `IDENTIFIED BY` clause, then the user's password will be changed. You must have the privileges needed for the [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password/)
statement to change a user's password with `GRANT`.

The only [authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins/) that this clause supports are [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password/) and [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password/).

### IDENTIFIED BY PASSWORD 'password_hash'

The optional `IDENTIFIED BY PASSWORD` clause can be used to provide an account with a password that has already been hashed. The password should be specified as a hash that was provided by the [PASSWORD](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/) function. It will be stored to the [mysql.user](/kb/en/mysqluser-table/) table as-is.

For example, if our password is `mariadb`, then we can find the hash with:

```sql
SELECT PASSWORD('mariadb');
+-------------------------------------------+
| PASSWORD('mariadb')                       |
+-------------------------------------------+
| *54958E764CE10E50764C2EECBB71D01F08549980 |
+-------------------------------------------+
1 row in set (0.00 sec)
```

And then we can create a user with the hash:

```sql
GRANT USAGE ON *.* TO foo2@test IDENTIFIED BY PASSWORD '*54958E764CE10E50764C2EECBB71D01F08549980';
```

If you do not specify a password with the `IDENTIFIED BY` clause, the user
will be able to connect without a password. A blank password is not a wildcard
to match any password. The user must connect without providing a password if no
password is set.

If the user account already exists and if you provide the `IDENTIFIED BY` clause, then the user's password will be changed. You must have the privileges needed for the [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password/)
statement to change a user's password with `GRANT`.

The only [authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins/) that this clause supports are [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password/) and [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password/).

### IDENTIFIED {VIA|WITH} authentication_plugin

The optional `IDENTIFIED VIA authentication_plugin` allows you to specify that the account should be authenticated by a specific [authentication plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins/). The plugin name must be an active authentication plugin as per [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins/). If it doesn't show up in that output, then you will need to install it with [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/) or [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/).

For example, this could be used with the [PAM authentication plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam/):

```sql
GRANT USAGE ON *.* TO foo2@test IDENTIFIED VIA pam;
```

Some authentication plugins allow additional arguments to be specified after a `USING` or `AS` keyword. For example, the [PAM authentication plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam/) accepts a [service name](/kb/en/authentication-plugin-pam/#configuring-the-pam-service):

```sql
GRANT USAGE ON *.* TO foo2@test IDENTIFIED VIA pam USING 'mariadb';
```

The exact meaning of the additional argument would depend on the specific authentication plugin.

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

The `USING` or `AS` keyword can also be used to provide a plain-text password to a plugin if it's provided as an argument to the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/) function. This is only valid for [authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins/) that have implemented a hook for the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/) function. For example, the [ed25519](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-ed25519/) authentication plugin supports this:

```sql
CREATE USER safe@'%' IDENTIFIED VIA ed25519 USING PASSWORD('secret');
```

##### MariaDB starting with [10.4.3](/kb/en/mariadb-1043-release-notes/)

One can specify many authentication plugins, they all works as alternatives ways of authenticating a user:

```sql
CREATE USER safe@'%' IDENTIFIED VIA ed25519 USING PASSWORD('secret') OR unix_socket;
```

## Resource Limit Options

##### MariaDB starting with [10.2.0](/kb/en/mariadb-1020-release-notes/)

[MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/) introduced a number of resource limit options.

It is possible to set per-account limits for certain server resources. The following table shows the values that can be set per account:

<table><tbody><tr><th>Limit Type</th><th>Decription</th></tr>
<tr><td><code>MAX_QUERIES_PER_HOUR</code></td><td>Number of statements that the account can issue per hour (including updates)</td></tr>
<tr><td><code>MAX_UPDATES_PER_HOUR</code></td><td>Number of updates (not queries) that the account can issue per hour</td></tr>
<tr><td><code>MAX_CONNECTIONS_PER_HOUR</code></td><td>Number of connections that the account can start per hour</td></tr>
<tr><td><code>MAX_USER_CONNECTIONS</code></td><td>Number of simultaneous connections that can be accepted from the same account; if it is 0, <code>max_connections</code> will be used instead; if <code>max_connections</code> is 0, there is no limit for this account's simultaneous connections.</td></tr>
<tr><td><code>MAX_STATEMENT_TIME</code></td><td>Timeout, in seconds, for statements executed by the user. See also <a href="/kb/en/aborting-statements/">Aborting Statements that Exceed a Certain Time to Execute</a>.</td></tr>
</tbody></table>

If any of these limits are set to `0`, then there is no limit for that resource for that user.

To set resource limits for an account, if you do not want to change that account's privileges, you can issue a `GRANT` statement with the `USAGE` privilege, which has no meaning. The statement can name some or all limit types, in any order.

Here is an example showing how to set resource limits:

```sql
GRANT USAGE ON *.* TO 'someone'@'localhost' WITH
    MAX_USER_CONNECTIONS 0
    MAX_QUERIES_PER_HOUR 200;
```

The resources are tracked per account, which means `'user'@'server'`; not per user name or per connection.

The count can be reset for all users using [FLUSH USER_RESOURCES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/), [FLUSH PRIVILEGES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) or [mysqladmin reload](/clients-utilities/mysqladmin/).

Users with the `CONNECTION ADMIN` privilege (in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/) and later) or the `SUPER` privilege are not restricted by `max_user_connections`, `max_connections`, or `max_password_errors`.

Per account resource limits are stored in the [user](/kb/en/mysqluser-table/) table, in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/) database. Columns used for resources limits are named `max_questions`, `max_updates`, `max_connections` (for `MAX_CONNECTIONS_PER_HOUR`), and `max_user_connections` (for `MAX_USER_CONNECTIONS`).

## TLS Options

By default, MariaDB transmits data between the server and clients without encrypting it. This is generally acceptable when the server and client run on the same host or in networks where security is guaranteed through other means. However, in cases where the server and client exist on separate networks or they are in a high-risk network, the lack of encryption does introduce security concerns as a malicious actor could potentially eavesdrop on the traffic as it is sent over the network between them.

To mitigate this concern, MariaDB allows you to encrypt data in transit between the server and clients using the Transport Layer Security (TLS) protocol. TLS was formerly known as Secure Socket Layer (SSL), but strictly speaking the SSL protocol is a predecessor to TLS and, that version of the protocol is now considered insecure. The documentation still uses the term SSL often and for compatibility reasons TLS-related server system and status variables still use the prefix ssl_, but internally, MariaDB only supports its secure successors.

See [Secure Connections Overview](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview/) for more information about how to determine whether your MariaDB server has TLS support.

You can set certain TLS-related restrictions for specific user accounts. For instance, you might use this with user accounts that require access to sensitive data while sending it across networks that you do not control. These restrictions can be enabled for a user account with the [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/), [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user/), or [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) statements. The following options are available:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>REQUIRE NONE</code></td><td>TLS is not required for this account, but can still be used.</td></tr>
<tr><td><code>REQUIRE SSL</code></td><td>The account must use TLS, but no valid X509 certificate is required. This option cannot be combined with other TLS options.</td></tr>
<tr><td><code>REQUIRE X509</code></td><td>The account must use TLS and must have a valid X509 certificate. This option implies <code>REQUIRE SSL</code>. This option cannot be combined with other TLS options.</td></tr>
<tr><td><code>REQUIRE ISSUER 'issuer'</code></td><td>The account must use TLS and must have a valid X509 certificate. Also, the Certificate Authority must be the one specified via the string <code>issuer</code>. This option implies <code>REQUIRE X509</code>. This option can be combined with the <code>SUBJECT</code>, and <code>CIPHER</code> options in any order.</td></tr>
<tr><td><code>REQUIRE SUBJECT 'subject'</code></td><td>The account must use TLS and must have a valid X509 certificate. Also, the certificate's Subject must be the one specified via the string <code>subject</code>. This option implies <code>REQUIRE X509</code>. This option can be combined with the <code>ISSUER</code>, and <code>CIPHER</code> options in any order.</td></tr>
<tr><td><code>REQUIRE CIPHER 'cipher'</code></td><td>The account must use TLS, but no valid X509 certificate is required. Also, the encryption used for the connection must use one of the methods specified in the string <code>cipher</code>. This option implies <code>REQUIRE SSL</code>. This option can be combined with the <code>ISSUER</code>, and <code>SUBJECT</code> options in any order.</td></tr>
</tbody></table>

The `REQUIRE` keyword must be used only once for all specified options, and the `AND` keyword can be used to separate individual options, but it is not required.

For example, you can create a user account that requires these TLS options with the following:

```sql
GRANT USAGE ON *.* TO 'alice'@'%'
    REQUIRE SUBJECT '/CN=alice/O=My Dom, Inc./C=US/ST=Oregon/L=Portland'
    AND ISSUER '/C=FI/ST=Somewhere/L=City/ O=Some Company/CN=Peter Parker/emailAddress=p.parker@marvel.com'
    AND CIPHER 'TLSv1.2';
```

If any of these options are set for a specific user account, then any client who tries to connect with that user account will have to be configured to connect with TLS.

See [Securing Connections for Client and Server](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/securing-connections-for-client-and-server/) for information on how to enable TLS on the client and server.

## Roles

### Syntax

```sql
GRANT role TO grantee [, grantee2 ... ]
[ WITH ADMIN OPTION ]
```

The GRANT statement is also used to grant the use a [role](/mariadb-administration/user-server-security/user-account-management/roles/) to one or more users or other roles. In order to be able to grant a role, the grantor doing so must have permission to do so (see WITH ADMIN in the [CREATE ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/create-role/) article).

Specifying the `WITH ADMIN OPTION` permits the grantee to in turn grant the role to another.

For example, the following commands show how to grant the same role to a couple different users.

```sql
GRANT journalist TO hulda;

GRANT journalist TO berengar WITH ADMIN OPTION;
```

If a user has been granted a role, they do not automatically obtain all permissions associated with that role. These permissions are only in use when the user activates the role with the [SET ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/set-role/) statement.

## Grant Examples

### Granting Root-like Privileges

You can create a user that has privileges similar to the default `root` accounts by executing the following:

```sql
CREATE USER 'alexander'@'localhost';
GRANT ALL PRIVILEGES ON  *.* to 'alexander'@'localhost' WITH GRANT OPTION;
```

## See Also

- [Troubleshooting Connection Issues](/kb/en/troubleshooting-connection-issues/)
- [--skip-grant-tables](/kb/en/mysqld-options/#-skip-grant-tables) allows you to start MariaDB without `GRANT`.  This is useful if you lost your root password.
- [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/)
- [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user/)
- [DROP USER](/sql-statements-structure/sql-statements/account-management-sql-commands/drop-user/)
- [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password/)
- [SHOW CREATE USER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-user/)
- [mysql.user table](/kb/en/mysqluser-table/)
- [Password Validation Plugins](/columns-storage-engines-and-plugins/plugins/password-validation-plugins/) - permits the setting of basic criteria for passwords
- [Authentication Plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins/) - allow various authentication methods to be used, and new ones to be developed.