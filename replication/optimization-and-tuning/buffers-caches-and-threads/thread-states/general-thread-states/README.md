# General Thread States

This article documents the major general thread states. More specific lists related to [delayed inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/), [replication](/replication/), the [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache/) and the [event scheduler](/programming-customizing-mariadb/triggers-events/event-scheduler/events/) are listed in:

- [Event Scheduler Thread States](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-states/event-scheduler-thread-states/)
- [Query Cache Thread States](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-states/query-cache-thread-states/)
- [Master Thread States](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-states/master-thread-states/)
- [Slave Connection Thread States](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-states/slave-connection-thread-states/)
- [Slave I/O Thread States](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-states/slave-io-thread-states/)
- [Slave SQL Thread States](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-states/slave-sql-thread-states/)

These correspond to the `STATE` values listed by the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) statement or in the [Information Schema PROCESSLIST Table](/kb/en/information-schema-processlist-table/) as well as the `PROCESSLIST_STATE` value listed in the [Performance Schema threads Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table/)

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td>After create</td><td>The function that created (or tried to create) a table (temporary or non-temporary) has just ended.</td></tr>
<tr><td>Analyzing</td><td>Calculating table key distributions, such as when running an <a href="/kb/en/analyze-table/">ANALYZE TABLE</a> statement.</td></tr>
<tr><td>checking permissions</td><td>Checking to see whether the permissions are adequate to perform the statement.</td></tr>
<tr><td>Checking table</td><td><a href="/kb/en/sql-commands-check-table/">Checking</a> the table.</td></tr>
<tr><td>cleaning up</td><td>Preparing to reset state variables and free memory after executing a command.</td></tr>
<tr><td>closing tables</td><td>Flushing the changes to disk and closing the table. This state will only persist if the disk is full or under extremely high load.</td></tr>
<tr><td>converting HEAP to Aria</td><td>Converting an internal <a href="/kb/en/memory-storage-engine/">MEMORY</a> temporary table into an on-disk <a href="/kb/en/aria/">Aria</a> temporary table.</td></tr>
<tr><td>converting HEAP to MyISAM</td><td>Converting an internal <a href="/kb/en/memory-storage-engine/">MEMORY</a> temporary table into an on-disk <a href="/kb/en/myisam/">MyISAM</a> temporary table.</td></tr>
<tr><td>copy to tmp table</td><td>A new table has been created as part of an <a href="/kb/en/alter-table/">ALTER TABLE</a> statement, and rows are about to be copied into it.</td></tr>
<tr><td>Copying to group table</td><td>Sorting the rows by group and copying to a temporary table, which occurs when a statement has different <a href="/kb/en/select/#group-by">GROUP BY</a> and <a href="/kb/en/select/#order-by">ORDER BY</a> criteria.</td></tr>
<tr><td>Copying to tmp table</td><td>Copying to a temporary table in memory.</td></tr>
<tr><td>Copying to tmp table on disk</td><td>Copying to a temporary table on disk, as the resultset is too large to fit into memory.</td></tr>
<tr><td>Creating index</td><td>Processing an <a href="/kb/en/alter-table/">ALTER TABLE ... ENABLE KEYS</a> for an <a href="/kb/en/aria/">Aria</a> or <a href="/kb/en/myisam/">MyISAM</a> table.</td></tr>
<tr><td>Creating sort index</td><td>Processing a <a href="/kb/en/select/">SELECT</a> statement resolved using an internal temporary table.</td></tr>
<tr><td>creating table</td><td>Creating a table (temporary or non-temporary).</td></tr>
<tr><td>Creating tmp table</td><td>Creating a temporary table (in memory or on-disk).</td></tr>
<tr><td>deleting from main table</td><td>Deleting from the first table in a multi-table <a href="/kb/en/delete/">delete</a>, saving columns and offsets for use in deleting from the other tables.</td></tr>
<tr><td>deleting from reference tables</td><td>Deleting matched rows from secondary reference tables as part of a multi-table <a href="/kb/en/delete/">delete</a>.</td></tr>
<tr><td>discard_or_import_tablespace</td><td>Processing an <a href="/kb/en/alter-table/">ALTER TABLE ... IMPORT TABLESPACE</a> or <a href="/kb/en/alter-table/">ALTER TABLE ... DISCARD TABLESPACE</a> statement.</td></tr>
<tr><td>end</td><td>State before the final cleanup of an <a href="/kb/en/alter-table/">ALTER TABLE</a>, <a href="/kb/en/create-view/">CREATE VIEW</a>, <a href="/kb/en/delete/">DELETE</a>, <a href="/kb/en/insert/">INSERT</a>, <a href="/kb/en/select/">SELECT</a>, or <a href="/kb/en/update/">UPDATE</a> statement.</td></tr>
<tr><td>executing</td><td>Executing a statement.</td></tr>
<tr><td>Execution of init_command</td><td>Executing statements specified by the <code>--init_command</code> <a href="/kb/en/mysql-command-line-client/">mysql client</a> option.</td></tr>
<tr><td>filling schema table</td><td>A table in the <code><a href="/kb/en/information-schema/">information_schema</a></code> database is being built.</td></tr>
<tr><td>freeing items</td><td>Freeing items from the <a href="/kb/en/query-cache/">query cache</a> after executing a command. Usually followed by the <code>cleaning up</code> state.</td></tr>
<tr><td>Flushing tables</td><td>Executing a <a href="/kb/en/flush/">FLUSH TABLES</a> statement and waiting for other threads to close their tables.</td></tr>
<tr><td>FULLTEXT initialization</td><td>Preparing to run a <a href="/kb/en/full-text-indexes/">full-text</a> search</td></tr>
<tr><td>init</td><td>About to initialize an <a href="/kb/en/alter-table/">ALTER TABLE</a>, <a href="/kb/en/delete/">DELETE</a>, <a href="/kb/en/insert/">INSERT</a>, <a href="/kb/en/select/">SELECT</a>, or <a href="/kb/en/update/">UPDATE</a> statement. Could be performaing <a href="/kb/en/query-cache/">query cache</a> cleanup, or flushing the <a href="/kb/en/binary-log/">binary log</a> or InnoDB log.</td></tr>
<tr><td>Killed</td><td>Thread will abort next time it checks the kill flag. Requires waiting for any locks to be released.</td></tr>
<tr><td>Locked</td><td>Query has been locked by another query.</td></tr>
<tr><td>logging slow query</td><td>Writing statement to the <a href="/kb/en/slow-query-log/">slow query log</a>.</td></tr>
<tr><td>NULL</td><td>State used for <a href="/kb/en/show-processlist/">SHOW PROCESSLIST</a>.</td></tr>
<tr><td>login</td><td>Connection thread has not yet been authenticated.</td></tr>
<tr><td>manage keys</td><td>Enabling or disabling a table index.</td></tr>
<tr><td>Opening table[s]</td><td>Trying to open a table. Usually very quick unless the limit set by <a href="/kb/en/server-system-variables/#table_open_cache">table_open_cache</a> has been reached, or an <a href="/kb/en/alter-table/">ALTER TABLE</a> or <a href="/kb/en/lock-tables-and-unlock-tables/">LOCK TABLE</a> is in progress.</td></tr>
<tr><td>optimizing</td><td>Server is performing initial optimizations in for a query.</td></tr>
<tr><td>preparing</td><td>State occurring during query optimization.</td></tr>
<tr><td>Purging old relay logs</td><td>Relay logs that are no longer needed are being removed.</td></tr>
<tr><td>query end</td><td>Query has finished being processed, but items have not yet been freed (the <code>freeing items</code> state.</td></tr>
<tr><td>Reading file</td><td>Server is reading the file (for example during <a href="/kb/en/load-data-infile/">LOAD DATA INFILE</a>).</td></tr>
<tr><td>Reading from net</td><td>Server is reading a network packet.</td></tr>
<tr><td>Removing duplicates</td><td>Duplicated rows being removed before sending to the client. This happens when <code>SELECT DISTINCT</code> is used in a way that the distinct operation could not be optimized at an earlier point.</td></tr>
<tr><td>removing tmp table</td><td>Removing an internal temporary table after processing a <a href="/kb/en/select/">SELECT</a> statement.</td></tr>
<tr><td>rename</td><td>Renaming a table.</td></tr>
<tr><td>rename result table</td><td>Renaming a table that results from an <a href="/kb/en/alter-table/">ALTER TABLE</a> statement having created a new table.</td></tr>
<tr><td>Reopen tables</td><td>Table is being re-opened after thread obtained a lock but the underlying table structure had changed, so the lock was released.</td></tr>
<tr><td>Repair by sorting</td><td>Indexes are being created with the use of a sort. Much faster than the related <code>Repair with keycache</code>.</td></tr>
<tr><td>Repair done</td><td>Multi-threaded repair has been completed.</td></tr>
<tr><td>Repair with keycache</td><td>Indexes are being created through the key cache, one-by-one. Much slower than the related <code>Repair by sorting</code>.</td></tr>
<tr><td>Rolling back</td><td>A transaction is being rolled back.</td></tr>
<tr><td>Saving state</td><td>New table state is being saved. For example, after, analyzing a <a href="/kb/en/myisam/">MyISAM</a> table, the key distributions, rowcount etc. are saved to the .MYI file.</td></tr>
<tr><td>Searching rows for update</td><td>Finding matching rows before performing an <a href="/kb/en/update/">UPDATE</a>, which is needed when the UPDATE would change the index used for the UPDATE</td></tr>
<tr><td>Sending data</td><td>Sending data to the client as part of processing a <a href="/kb/en/select/">SELECT</a> statement. Often the longest-occurring state due to the relatively slow disk accesses that may be required.</td></tr>
<tr><td>setup</td><td>Setting up an <a href="/kb/en/alter-table/">ALTER TABLE</a> operation.</td></tr>
<tr><td>Sorting for group</td><td>Sorting as part of a <a href="/kb/en/select/#group-by">GROUP BY</a></td></tr>
<tr><td>Sorting for order</td><td>Sorting as part of an  <a href="/kb/en/select/#order-by">ORDER BY</a></td></tr>
<tr><td>Sorting index</td><td>Sorting index pages as part of a table optimization operation.</td></tr>
<tr><td>Sorting result</td><td>Processing a <a href="/kb/en/select/">SELECT</a> statement using a non-temporary table.</td></tr>
<tr><td>statistics</td><td>Calculating statistics as part of deciding on a query execution plan. Usually a brief state unless the server is disk-bound.</td></tr>
<tr><td>System lock</td><td>Requesting or waiting for an <em>external</em> lock for a specific table. The <a href="/kb/en/storage-engines/">storage engine</a> determines what kind of <em>external</em> lock to use. For example, the <a href="/kb/en/myisam-storage-engine/">MyISAM</a> storage engine uses file-based locks. However, MyISAM's <em>external</em> locks are disabled by default, due to the default value of the <code><a href="/kb/en/server-system-variables/#skip_external_locking">skip_external_locking</a></code> system variable. Transactional storage engines such as <a href="/kb/en/innodb/">InnoDB</a> also register the transaction or statement with MariaDB's <a href="/kb/en/transaction-coordinator-log/">transaction coordinator</a> while in this thread state. See <a href="https://jira.mariadb.org/browse/MDEV-19391">MDEV-19391</a> for more information about that.</td></tr>
<tr><td>Table lock</td><td>About to request a table's <em>internal</em> lock after acquiring the table's <em>external</em> lock. This thread state occurs after the <em>System lock</em> thread state.</td></tr>
<tr><td>update</td><td>About to start updating table.</td></tr>
<tr><td>Updating</td><td>Searching for and updating rows in a table.</td></tr>
<tr><td>updating main table</td><td>Updating the first table in a multi-table update, and saving columns and offsets for use in the other tables.</td></tr>
<tr><td>updating reference tables</td><td>Updating the secondary (reference) tables in a multi-table update</td></tr>
<tr><td>updating status</td><td>This state occurs after a query's execution is complete. If the query's execution time exceeds <code><a href="/kb/en/server-system-variables/#long_query_time">long_query_time</a></code>, then <code><a href="/kb/en/server-status-variables/#slow_queries">Slow_queries</a></code> is incremented, and if the <a href="/kb/en/slow-query-log/">slow query log</a> is enabled, then the query is logged. If the <code><a href="/kb/en/mariadb-audit-plugin/">SERVER_AUDIT</a></code> plugin is enabled, then the query is also logged into the audit log at this stage. If the <code><a href="/kb/en/user-statistics/">userstats</a></code> plugin is enabled, then CPU statistics are also updated at this stage.</td></tr>
<tr><td>User lock</td><td>About to request or waiting for an advisory lock from a <a href="/kb/en/get_lock/">GET LOCK()</a> call. For <a href="/kb/en/show-profile/">SHOW PROFILE</a>, means requesting a lock only.</td></tr>
<tr><td>User sleep</td><td>A <a href="/kb/en/sleep/">SLEEP()</a> call has been invoked.</td></tr>
<tr><td>Waiting for commit lock</td><td><a href="/kb/en/flush/">FLUSH TABLES WITH READ LOCK</a> is waiting for a commit lock, or a statement resulting in an explicit or implicit commit is waiting for a read lock to be released. This state was called <code>Waiting for all running commits to finish </code> in earlier versions.</td></tr>
<tr><td>Waiting for global read lock</td><td>Waiting for a global read lock.</td></tr>
<tr><td>Waiting for table level lock</td><td>External lock acquired,and internal lock about to be requested. Occurs after the <code>System lock</code> state. In earlier versions, this was called <code>Table lock</code>.</td></tr>
<tr><td>Waiting for <em>xx</em> lock</td><td>Waiting to obtain a lock of type <code>xx</code>.</td></tr>
<tr><td>Waiting on cond</td><td>Waiting for an unspecified condition to occur.</td></tr>
<tr><td>Writing to net</td><td>Writing a packet to the network.</td></tr>
</tbody></table>