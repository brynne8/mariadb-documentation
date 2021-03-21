# FLUSH

## Syntax

```sql
FLUSH [NO_WRITE_TO_BINLOG | LOCAL]
    flush_option [, flush_option] ...
```

or when flushing tables:

```sql
FLUSH [NO_WRITE_TO_BINLOG | LOCAL] TABLES [table_list]  [table_flush_option]
```

where `table_list` is a list of tables separated by `,` (comma).

## Description

The `FLUSH` statement clears or reloads various internal caches used by
MariaDB. To execute `FLUSH`, you must have the `RELOAD`
privilege. See [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant).

The <code class="fixed" style="white-space:pre-wrap">RESET</code> statement is similar to `FLUSH`. See
[RESET](/sql-statements-structure/sql-statements/administrative-sql-statements/reset).

You cannot issue a FLUSH statement from within a [stored function](/programming-customizing-mariadb/stored-routines/stored-functions) or a [trigger](/programming-customizing-mariadb/triggers-events/triggers). Doing so within a stored procedure is permitted, as long as it is not called by a stored function or trigger. See [Stored Routine Limitations](/programming-customizing-mariadb/stored-routines/stored-routine-limitations), [Stored Function Limitations](/programming-customizing-mariadb/stored-routines/stored-functions/stored-function-limitations) and [Trigger Limitations](/programming-customizing-mariadb/triggers-events/triggers/trigger-limitations).

If a listed table is a view, an error like the following will be produced:

```sql
ERROR 1347 (HY000): 'test.v' is not BASE TABLE
```

By default, `FLUSH` statements are written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) and will be [replicated](/replication). The `NO_WRITE_TO_BINLOG` keyword (`LOCAL` is an alias) will ensure the statement is not written to the binary log.

The different flush options are:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>CHANGED_PAGE_BITMAPS</code></td><td>Internal command used for backup purposes. See the <a href="/kb/en/information-schema-changed_page_bitmaps-table/">Information Schema CHANGED_PAGE_BITMAPS Table</a>.</td></tr>
<tr><td><code>CLIENT_STATISTICS</code></td><td>Reset client statistics (see <a href="/kb/en/show-client-statistics/">SHOW CLIENT_STATISTICS</a>).</td></tr>
<tr><td><code>DES_KEY_FILE</code></td><td>Reloads the DES key file (Specified with the <a href="/kb/en/mysqld-options-full-list/">--des-key-file startup option</a>).</td></tr>
<tr><td><code>HOSTS</code></td><td>Flush the hostname cache (used for converting ip to host names and for unblocking blocked hosts. See <a href="/kb/en/server-system-variables/#max_connect_errors">max_connect_errors</a>)</td></tr>
<tr><td><code>INDEX_STATISTICS</code></td><td>Reset index statistics (see <a href="/kb/en/show-index-statistics/">SHOW INDEX_STATISTICS</a>).</td></tr>
<tr><td><code><code class="fixed" style="white-space:pre-wrap">[ERROR | ENGINE | GENERAL | SLOW | BINARY | RELAY] LOGS</code></code></td><td>Close and reopen the specified log type, or all log types if none are specified. <code>FLUSH RELAY LOGS [connection-name]</code> can be used to flush the relay logs for a specific connection. Only one connection can be specified per <code>FLUSH</code> command. See <a href="/kb/en/multi-source-replication/">Multi-source replication</a>. <code>FLUSH ENGINE LOGS</code> will delete all unneeded <a href="/kb/en/aria/">Aria</a> redo logs. Since <a href="/kb/en/mariadb-10130-release-notes/">MariaDB 10.1.30</a> and <a href="/kb/en/mariadb-10211-release-notes/">MariaDB 10.2.11</a>, <code>FLUSH BINARY LOGS DELETE_DOMAIN_ID=(list-of-domains)</code> can be used to discard obsolete <a href="/kb/en/gtid/">GTID</a> domains from the server's <a href="/kb/en/binary-log/">binary log</a> state. In order for this to be successful, no event group from the listed <a href="/kb/en/gtid/">GTID</a> domains can be present in existing <a href="/kb/en/binary-log/">binary log</a> files. If some still exist, then they must be purged prior to executing this command. If the command completes successfully, then it also rotates the <a href="/kb/en/binary-log/">binary log</a>.</td></tr>
<tr><td><code>MASTER</code></td><td>Deprecated option, use <a href="/kb/en/reset/">RESET MASTER</a> instead.</td></tr>
<tr><td><code>PRIVILEGES</code></td><td>Reload all privileges from the privilege tables in the <code>mysql</code> database. If the server is started with <code>--skip-grant-table</code> option, this will activate the privilege tables again.</td></tr>
<tr><td><a href="/kb/en/flush-query-cache/">QUERY CACHE</a></td><td>Defragment the <a href="/kb/en/query-cache/">query cache</a> to better utilize its memory. If you want to reset the query cache, you can do it with <a href="/kb/en/reset/">RESET QUERY CACHE</a>.</td></tr>
<tr><td><code>QUERY_RESPONSE_TIME</code></td><td>See the <a href="/kb/en/query_response_time-plugin/">QUERY_RESPONSE_TIME</a> plugin.</td></tr>
<tr><td><code>SLAVE</code></td><td>Deprecated option, use <a href="/kb/en/reset/">RESET SLAVE</a> instead.</td></tr>
<tr><td><code>SSL</code></td><td>Used to dynamically reinitialize the server's <a href="/kb/en/data-in-transit-encryption/">TLS</a> context by reloading the files defined by several <a href="/kb/en/ssltls-system-variables/">TLS system variables</a>. See <code><a href="#flush-ssl">FLUSH SSL</a></code> for more information. This command was first added in <a href="/kb/en/mariadb-1041-release-notes/">MariaDB 10.4.1</a>.</td></tr>
<tr><td><code>STATUS</code></td><td>Resets all <a href="/kb/en/server-status-variables/">server status variables</a> that can be reset to 0. Not all global status variables support this, so not all global values are reset. See <code><a href="#flush-status">FLUSH STATUS</a></code> for more information.</td></tr>
<tr><td><code>TABLE</code></td><td>Close tables given as options or all open tables if no table list was used. From <a href="/kb/en/mariadb-1041-release-notes/">MariaDB 10.4.1</a>, using without any table list will only close tables not in use, and tables not locked by the FLUSH TABLES connection. If there are no locked tables, FLUSH TABLES will be instant and will not cause any waits, as it no longer waits for tables in use. When a table list is provided, from <a href="/kb/en/mariadb-1041-release-notes/">MariaDB 10.4.1</a>, the server will wait for the end of any transactions that are using the tables. Previously, FLUSH TABLES only waited for the statements to complete.</td></tr>
<tr><td><code>TABLES</code></td><td>Same as <code>FLUSH TABLE</code>.</td></tr>
<tr><td><code><a href="/kb/en/flush-tables-for-export/">TABLES ... FOR EXPORT</a></code></td><td>For InnoDB tables, flushes table changes to disk to permit binary table copies while the server is running. Introduced in <a href="/kb/en/mariadb-1008-release-notes/">MariaDB 10.0.8</a>. See <a href="/kb/en/flush-tables-for-export/">FLUSH TABLES ... FOR EXPORT</a> for more.</td></tr>
<tr><td><code>TABLES WITH READ LOCK</code></td><td>Closes all open tables. New tables are only allowed to be opened with read locks until an <a href="/kb/en/transactions-lock/">UNLOCK TABLES</a> is given.</td></tr>
<tr><td><code>TABLES WITH READ LOCK AND DISABLE CHECKPOINT</code></td><td>As <code>TABLES WITH READ LOCK</code> but also disable all checkpoint writes by transactional table engines. This is useful when doing a disk snapshot of all tables.</td></tr>
<tr><td><code>TABLE_STATISTICS</code></td><td>Reset table statistics (see <a href="/kb/en/show-table-statistics/">SHOW TABLE_STATISTICS</a>).</td></tr>
<tr><td><code>USER_RESOURCES</code></td><td>Resets all per hour <a href="/kb/en/grant/#setting-per-account-resources-limits">user resources</a>. This enables clients that have exhausted their resources to connect again.</td></tr>
<tr><td><code>USER_STATISTICS</code></td><td>Reset user statistics (see <a href="/kb/en/show-user-statistics/">SHOW USER_STATISTICS</a>).</td></tr>
</tbody></table>

You can also use the [mysqladmin](/clients-utilities/mysqladmin) client to flush things. Use <code class="highlight fixed" style="white-space:pre-wrap">mysqladmin --help</code> to examine what flush commands it supports.

## `FLUSH STATUS`

[Server status variables](/replication/optimization-and-tuning/system-variables/server-status-variables) can be reset by executing the following:

```sql
FLUSH STATUS;
```

### Global Status Variables that Support `FLUSH STATUS`

Not all global status variables support being reset by `FLUSH STATUS`. Currently, the following status variables are reset by `FLUSH STATUS`:

- <a undefined>Aborted_clients</a>
- <a undefined>Aborted_connects</a>
- <a undefined>Aria_pagecache_blocks_not_flushed</a>
- <a undefined>Aria_pagecache_blocks_unused</a>
- <a undefined>Aria_pagecache_blocks_used</a>
- <a undefined>Binlog_cache_disk_use</a>
- <a undefined>Binlog_cache_use</a>
- <a undefined>Binlog_stmt_cache_disk_use</a>
- <a undefined>Binlog_stmt_cache_use</a>
- <a undefined>Connection_errors_accept</a>
- <a undefined>Connection_errors_internal</a>
- <a undefined>Connection_errors_max_connections</a>
- <a undefined>Connection_errors_peer_address</a>
- <a undefined>Connection_errors_select</a>
- <a undefined>Connection_errors_tcpwrap</a>
- <a undefined>Created_tmp_files</a>
- <a undefined>Delayed_errors</a>
- <a undefined>Delayed_writes</a>
- <a undefined>Feature_check_constraint</a>
- <a undefined>Feature_delay_key_write</a>
- <a undefined>Max_used_connections</a>
- <a undefined>Opened_plugin_libraries</a>
- <a undefined>Performance_schema_accounts_lost</a>
- <a undefined>Performance_schema_cond_instances_lost</a>
- <a undefined>Performance_schema_digest_lost</a>
- <a undefined>Performance_schema_file_handles_lost</a>
- <a undefined>Performance_schema_file_instances_lost</a>
- <a undefined>Performance_schema_hosts_lost</a>
- <a undefined>Performance_schema_locker_lost</a>
- <a undefined>Performance_schema_mutex_instances_lost</a>
- <a undefined>Performance_schema_rwlock_instances_lost</a>
- <a undefined>Performance_schema_session_connect_attrs_lost</a>
- <a undefined>Performance_schema_socket_instances_lost</a>
- <a undefined>Performance_schema_stage_classes_lost</a>
- <a undefined>Performance_schema_statement_classes_lost</a>
- <a undefined>Performance_schema_table_handles_lost</a>
- <a undefined>Performance_schema_table_instances_lost</a>
- <a undefined>Performance_schema_thread_instances_lost</a>
- <a undefined>Performance_schema_users_lost</a>
- <a undefined>Qcache_hits</a>
- <a undefined>Qcache_inserts</a>
- <a undefined>Qcache_lowmem_prunes</a>
- <a undefined>Qcache_not_cached</a>
- <a undefined>Rpl_semi_sync_master_no_times</a>
- <a undefined>Rpl_semi_sync_master_no_tx</a>
- <a undefined>Rpl_semi_sync_master_timefunc_failures</a>
- <a undefined>Rpl_semi_sync_master_wait_pos_backtraverse</a>
- <a undefined>Rpl_semi_sync_master_yes_tx</a>
- <a undefined>Rpl_transactions_multi_engine</a>
- <a undefined>Server_audit_writes_failed</a>
- <a undefined>Slave_retried_transactions</a>
- <a undefined>Slow_launch_threads</a>
- <a undefined>Ssl_accept_renegotiates</a>
- <a undefined>Ssl_accepts</a>
- <a undefined>Ssl_callback_cache_hits</a>
- <a undefined>Ssl_client_connects</a>
- <a undefined>Ssl_connect_renegotiates</a>
- <a undefined>Ssl_ctx_verify_depth</a>
- <a undefined>Ssl_ctx_verify_mode</a>
- <a undefined>Ssl_finished_accepts</a>
- <a undefined>Ssl_finished_connects</a>
- <a undefined>Ssl_session_cache_hits</a>
- <a undefined>Ssl_session_cache_misses</a>
- <a undefined>Ssl_session_cache_overflows</a>
- <a undefined>Ssl_session_cache_size</a>
- <a undefined>Ssl_session_cache_timeouts</a>
- <a undefined>Ssl_sessions_reused</a>
- <a undefined>Ssl_used_session_cache_entries</a>
- <a undefined>Subquery_cache_hit</a>
- <a undefined>Subquery_cache_miss</a>
- <a undefined>Table_locks_immediate</a>
- <a undefined>Table_locks_waited</a>
- <a undefined>Tc_log_max_pages_used</a>
- <a undefined>Tc_log_page_waits</a>
- <a undefined>Transactions_gtid_foreign_engine</a>
- <a undefined>Transactions_multi_engine</a>

## The different usage of `FLUSH TABLES`

### The purpose of `FLUSH TABLES`

The purpose of `FLUSH TABLES` is to clean up the open table cache and
table definition cache from not in use tables.  This frees up memory
and file descriptors. Normally this is not needed as the caches works
on a FIFO bases, but can be useful if the server seams to use up to
much memory for some reason.

### The purpose of `FLUSH TABLES WITH READ LOCK `

`FLUSH TABLES WITH READ LOCK` is useful if you want to take a backup
of some tables.  When `FLUSH TABLES WITH READ LOCK` returns, all
write access to tables are blocked and all tables are marked as
'properly closed' on disk.  The tables can still be used for read
operations.

### The purpose of `FLUSH TABLES table_list`

`FLUSH TABLES` table_list is useful if you want to copy a table
object/files to or from the server. This command puts a lock that
stops new users of the table and will wait until everyone has stopped
using the table. The table is then removed from the table definition
and table cache.

Note that it's up to the user to ensure that no one is accessing the
table between `FLUSH TABLES` and the table is copied to or from the
server. This can be secured by using [LOCK TABLES](/sql-statements-structure/sql-statements/transactions/lock-tables).

If there are any tables locked by the connection that is using `FLUSH TABLES` all the locked tables will be closed as part of the flush and
reopened and relocked before `FLUSH TABLES` returns. This allows one
to copy the table after `FLUSH TABLES` returns without having any
writes on the table.  For now this works works with most tables,
except InnoDB as InnoDB may do background purges on the table even
while it's write locked.

### The purpose of `FLUSH TABLES table_list WITH READ LOCK`

`FLUSH TABLES table_list WITH READ LOCK` should work as `FLUSH TABLES WITH READ LOCK`, but only those tables that are listed will be
properly closed.  However in practice this works exactly like `FLUSH TABLES WITH READ LOCK` as the `FLUSH` command has anyway to wait
for all WRITE operations to end because we are depending on a global
read lock for this code.  In the future we should consider fixing this
to instead use meta data locks.

## Implementation of `FLUSH TABLES` commands in [MariaDB 10.4.8](/kb/en/mariadb-1048-release-notes/) and above

### Implementation of `FLUSH TABLES`

- Free memory and file descriptors not in use

### Implementation of `FLUSH TABLES WITH READ LOCK`

- Lock all tables read only for simple old style backup.
- All background writes are suspended and tables are marked as closed.
- No statement requiring table changes are allowed for any user until `UNLOCK TABLES`.

Instead of using `FLUSH TABLE WITH READ LOCK` one should in most cases instead use
[BACKUP STAGE BLOCK_COMMIT](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage).

### Implementation of `FLUSH TABLES table_list`

- Free memory and file descriptors for tables not in use from table list.
- Lock given tables as read only.
- Wait until all translations has ended that uses any of the given tables.
- Wait until all background writes are suspended and tables are marked as closed.

### Implementation of `FLUSH TABLES table_list FOR EXPORT`

- Free memory and file descriptors for tables not in use from table list
- Lock given tables as read.
- Wait until all background writes are suspended and tables are marked as closed.
- Check that all tables supports `FOR EXPORT`
- No changes to these tables allowed until `UNLOCK TABLES`

This is basically the same behavior as in old MariaDB version if one first lock the tables, then do
`FLUSH TABLES`.  The tables will be copyable until `UNLOCK TABLES`.

## `FLUSH SSL`

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

The `FLUSH SSL` command was first added in [MariaDB 10.4](/kb/en/what-is-mariadb-104/).

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, the `FLUSH SSL` command can be used to dynamically reinitialize the server's [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption) context. This is most useful if you need to replace a certificate that is about to expire without restarting the server.

This operation is performed by reloading the files defined by the following [TLS system variables](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/ssltls-system-variables):

- <a undefined>ssl_cert</a>
- <a undefined>ssl_key</a>
- <a undefined>ssl_ca</a>
- <a undefined>ssl_capath</a>
- <a undefined>ssl_crl</a>
- <a undefined>ssl_crlpath</a>

These [TLS system variables](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/ssltls-system-variables) are not dynamic, so their values can <strong>not</strong> be changed without restarting the server.

If you want to dynamically reinitialize the server's [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption) context, then you need to change the certificate and key files at the relevant paths defined by these [TLS system variables](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/ssltls-system-variables), without actually changing the values of the variables. See [MDEV-19341](https://jira.mariadb.org/browse/MDEV-19341) for more information.

## Reducing Memory Usage

To flush some of the global caches that take up memory, you could execute the following command:

```sql
FLUSH LOCAL HOSTS,
   QUERY CACHE, 
   TABLE_STATISTICS, 
   INDEX_STATISTICS, 
   USER_STATISTICS;
```