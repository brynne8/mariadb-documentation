# Transaction Coordinator Log Overview

The transaction coordinator log (tc_log) is used to coordinate transactions that affect multiple [XA-capable](/sql-statements-structure/sql-statements/transactions/xa-transactions/) [storage engines](/columns-storage-engines-and-plugins/storage-engines/). If you have two or more XA-capable storage engines enabled, then a transaction coordinator log must be available.

## Types of Transaction Coordinator Logs

There are currently two implementations of the transaction coordinator log:

- Binary log-based transaction coordinator log
- Memory-mapped file-based transaction coordinator log

If the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is enabled on a server, then the server will use the binary log-based transaction coordinator log. Otherwise, it will use the memory-mapped file-based transaction coordinator log.

### Binary Log-Based Transaction Coordinator Log

This transaction coordinator uses the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/), which is enabled by the <a undefined>log_bin</a> server option.

### Memory-Mapped File-Based Transaction Coordinator Log

This transaction coordinator uses the memory-mapped file defined by the <a undefined>--log-tc</a> server option. The size is defined by the <a undefined>log_tc_size</a> system variable.

Some facts about this log:

- The log consists of a memory-mapped file that is divided into pages of 8KB size.
- The usable size of the first page is smaller because of the log header. There is a PAGE control structure for each page.
- Each page (or rather its PAGE control structure) can be in one of the three states - active, syncing, pool.
- There could be only one page in the active or syncing state, but many in the pool state - pool is a fifo queue.
- The usual lifecycle of a page is pool-&gt;active-&gt;syncing-&gt;pool.
- The "active" page is a page where new xid's are logged.
- The page stays active as long as the syncing slot is taken.
- The "syncing" page is being synced to disk. no new xid can be added to it.
- When the syncing is done the page is moved to a pool and an active page becomes "syncing".

The result of such an architecture is a natural "commit grouping" - If commits are coming faster than the system can sync, they do not stall. Instead, all commits that came since the last sync are logged to the same "active" page, and they all are synced with the next - one - sync. Thus, thought individual commits are delayed, throughput is not decreasing.

When an xid is added to an active page, the thread of this xid waits for a page's condition until the page is synced. When a syncing slot becomes vacant one of these waiters is awakened to take care of syncing. It syncs the page and signals all waiters that the page is synced. The waiters are counted, and a page may never become active again until waiters==0, which means that is all waiters from the previous sync have noticed that the sync was completed.

Note that a page becomes "dirty" and has to be synced only when a new xid is added into it. Removing a xid from a page does not make it dirty - we don't sync xid removals to disk.

#### Monitoring the Memory-Mapped File-Based Transaction Coordinator Log

The memory-mapped transaction coordinator log can be monitored with the following status variables:

- <a undefined>Tc_log_max_pages_used</a>
- <a undefined>Tc_log_page_size</a>
- <a undefined>Tc_log_page_waits</a>

## Heuristic Recovery with the Transaction Coordinator Log

One of the main purposes of the transaction coordinator log is in crash recovery. See [Heuristic Recovery with the Transaction Coordinator Log](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log/heuristic-recovery-with-the-transaction-coordinator-log/) for more information about that.

## Known Issues

### You must enable exactly N storage engines

Prior to [MariaDB 10.1.10](/kb/en/mariadb-10110-release-notes/), if you were using the memory-mapped file-based transaction coordinator log, and then if the server crashed and you changed the number of XA-capable storage engines that it loaded, then you could see errors like the following:

```sql
2018-11-30 23:08:49 140046048638848 [Note] Recovering after a crash using tc.log          
2018-11-30 23:08:49 140046048638848 [ERROR] Recovery failed! You must enable exactly 3 storage engines that support two-phase commit protocol
2018-11-30 23:08:49 140046048638848 [ERROR] Crash recovery failed. Either correct the problem (if it's, for example, out of memory error) and restart, or delete tc log and start mysqld with --tc-heuristic-recover={commit|rollback}
2018-11-30 23:08:49 140046048638848 [ERROR] Can't init tc log
2018-11-30 23:08:49 140046048638848 [ERROR] Aborting
```

To recover from this error, delete the file defined by the <a undefined>--log-tc</a> server option, and then restart the server with the <a undefined>--tc-heuristic-recover</a> option set.

See [MDEV-9214](https://jira.mariadb.org/browse/MDEV-9214) for more information.

### Bad magic header in tc log

If you are using the memory-mapped file-based transaction coordinator log, then it is possible to see errors like the following:

```sql
2018-09-19  4:29:31 0 [Note] Recovering after a crash using tc.log                                                               
2018-09-19  4:29:31 0 [ERROR] Bad magic header in tc log
2018-09-19  4:29:31 0 [ERROR] Crash recovery failed. Either correct the problem (if it's, for example, out of memory error) and restart, or delete tc log and start mysqld with --tc-heuristic-recover={commit|rollback}                                           
2018-09-19  4:29:31 0 [ERROR] Can't init tc log
2018-09-19  4:29:31 0 [ERROR] Aborting
```

This means that the header of the memory-mapped file-based transaction coordinator log is corrupt. To recover from this error, delete the file defined by the <a undefined>--log-tc</a> server option, and then restart the server with the <a undefined>--tc-heuristic-recover</a> option set.

This issue is known to occur when using docker. In that case, the problem may be caused by using a MariaDB container version with a data directory from a different MariaDB or MySQL version. Therefore, some potential fixes are:

- Pinning the docker instance to a specific MariaDB version in the docker compose file, so that it consistently uses the same version.
- Running [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade/) to ensure that the data directory is upgraded to match the server version.

See [this docker issue](https://github.com/docker-library/mariadb/issues/201) for more information.

### MariaDB Galera Cluster

[MariaDB Galera Cluster](/replication/galera-cluster/) builds include a built-in plugin called `wsrep`. Prior to [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/), this plugin was internally considered an [XA-capable](/sql-statements-structure/sql-statements/transactions/xa-transactions/) [storage engine](/columns-storage-engines-and-plugins/storage-engines/). Consequently, these [MariaDB Galera Cluster](/replication/galera-cluster/) builds have multiple XA-capable storage engines by default, even if the only "real" storage engine that supports external [XA transactions](/sql-statements-structure/sql-statements/transactions/xa-transactions/) enabled on these builds by default is [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/). Therefore, when using one these builds MariaDB would be forced to use a transaction coordinator log by default, which could have performance implications.

For example, [MDEV-16509](https://jira.mariadb.org/browse/MDEV-16509) describes performance problems where [MariaDB Galera Cluster](/replication/galera-cluster/) actually performs better when the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is enabled. It is possible that this is caused by the fact that MariaDB is forced to use the memory-mapped file-based transaction coordinator log in this case, which may not perform as well.

This became a bigger issue in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) when the [MySQL-wsrep](https://github.com/codership/mysql-wsrep) patch that powers [MariaDB Galera Cluster](/replication/galera-cluster/) was enabled on most MariaDB builds on Linux by default. Consequently, this built-in `wsrep` plugin would exist on those MariaDB builds on Linux by default. Therefore, MariaDB users might pay a performance penalty, even if they never actually intended to use the [MariaDB Galera Cluster](/replication/galera-cluster/) features included in [MariaDB 10.1](/kb/en/what-is-mariadb-101/).

In [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later, the built-in `wsrep` plugin has been changed to a replication plugin. Therefore, it is no longer considered an [XA-capable](/sql-statements-structure/sql-statements/transactions/xa-transactions/) storage engine, so it no longer forces MariaDB to use a transaction coordinator log by default.

See [MDEV-16442](https://jira.mariadb.org/browse/MDEV-16442) for more information.