# MariaDB Galera Cluster - Known Limitations

This article contains information on known problems and limitations of MariaDB Galera Cluster.

## Limitations from codership.com:

- Currently replication works only with the [InnoDB storage engine](/kb/en/xtradb-and-innodb/). Any writes
  to tables of other types, including system (mysql.*) tables are not
  replicated (this limitation excludes DDL statements such as [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user),  which implicitly modify the mysql.* tables <span>—</span> those are replicated). There is however experimental support for [MyISAM](/kb/en/myisam/) - see the [wsrep_replicate_myisam](/kb/en/galera-cluster-system-variables/#wsrep_replicate_myisam) system variable)

- Unsupported explicit locking include [LOCK TABLES](/kb/en/transactions-lock/), [FLUSH TABLES {explicit table list} WITH READ LOCK](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush), ([GET_LOCK()](/built-in-functions/secondary-functions/miscellaneous-functions/get_lock), [RELEASE_LOCK()](/built-in-functions/secondary-functions/miscellaneous-functions/release_lock),…). Using transactions properly should be able to overcome these limitations. Global locking operators like [FLUSH TABLES WITH READ LOCK](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) are supported.

- All tables should have a primary key (multi-column primary keys are supported). [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) operations are unsupported on tables without a primary key. Also, rows in tables without a primary key may appear in a different order on different nodes.

- The [general query log](/mariadb-administration/server-monitoring-logs/general-query-log) and the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log) cannot be directed to a table. If you enable these logs, then you must forward the log to a file by setting <a undefined>log_output=FILE</a>.

- [XA transactions](/sql-statements-structure/sql-statements/transactions/xa-transactions) are not supported.

- Transaction size. While Galera does not explicitly limit the transaction
  size, a writeset is processed as a single memory-resident buffer and as a
  result, extremely large transactions (e.g. [LOAD DATA](/kb/en/load-data/)) may adversely affect
  node performance. To avoid that, the [wsrep_max_ws_rows](/kb/en/galera-cluster-system-variables/#wsrep_max_ws_rows) and [wsrep_max_ws_size](/kb/en/galera-cluster-system-variables/#wsrep_max_ws_size) system
  variables limit transaction rows to 128K and the transaction size to 2Gb by
  default. If necessary, users may want to increase those limits. Future versions
  will add support for transaction fragmentation.

## Other observations, in no particular order:

- If you are using [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump) for state transfer, and it failed for whatever
  reason (e.g. you do not have the database account it attempts to connect
  with, or it does not have necessary permissions), you will see an SQL SYNTAX
  error in the server [error log](/mariadb-administration/server-monitoring-logs/error-log). Don't let it fool you, this is just a fancy
  way to deliver a message (the pseudo-statement inside of the bogus SQL will
  actually contain the error message).

- Do not use transactions of any essential size. Just to insert 100K rows, the
  server might require additional 200-300 Mb. In a less fortunate scenario it
  can be 1.5 Gb for 500K rows, or 3.5 Gb for 1M rows. See [MDEV-466](https://jira.mariadb.org/browse/MDEV-466) for some
  numbers (you'll see that it's closed, but it's not closed because it was
  fixed).

- Locking is lax when DDL is involved. For example, if your DML transaction
  uses a table, and a parallel DDL statement is started, in the normal MySQL
  setup it would have waited for the metadata lock, but in Galera context it
  will be executed right away. It happens even if you are running a single
  node, as long as you configured it as a cluster node. See also [MDEV-468](https://jira.mariadb.org/browse/MDEV-468). This
  behavior might cause various side-effects, the consequences have not been
  investigated yet. Try to avoid such parallelism.

- Do not rely on auto-increment values to be sequential. Galera uses a
  mechanism based on autoincrement increment to produce unique non-conflicting
  sequences, so on every single node the sequence will have gaps. See
  [http://codership.blogspot.com/2009/02/managing-auto-increments-with-multi.html](http://codership.blogspot.com/2009/02/managing-auto-increments-with-multi.html)

- A command may fail with <code class="fixed" style="white-space:pre-wrap">ER_UNKNOWN_COM_ERROR</code> producing 'WSREP has not yet prepared node for application use' (or 'Unknown command' in older versions) error message. It happens when a cluster is suspected to be split and the node is in a smaller part <span>—</span> for example, during a network glitch, when nodes temporarily lose each other. It can also occur during state transfer. The node takes this measure to prevent data inconsistency. Its usually a temporary state which can be detected by checking [wsrep_ready](/kb/en/galera-cluster-status-variables/#wsrep_ready) value. The node, however, allows SHOW and SET command during this period.

- After a temporary split, if the 'good' part of the cluster was still
  reachable and its state was modified, resynchronization occurs. As a part of
  it, nodes of the 'bad' part of the cluster drop all client connections. It
  might be quite unexpected, especially if the client was idle and did not even
  know anything wrong was happening. Please also note that after the connection
  to the isolated node is restored, if there is a flow on the node, it takes a
  long time for it to synchronize, during which the "good" node says that the
  cluster is already of the normal size and synced, while the rejoining node
  says it's only joined (but not synced). The connections keep getting 'unknown
  command'. It should pass eventually.

- While [binlog_format](/kb/en/replication-and-binary-log-server-system-variables/#binlog_format) is checked on startup and can only be ROW (see [Binary Log Formats](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats)), it can be changed at runtime. Do NOT change binlog_format at runtime, it is likely not only cause replication failure, but make all other nodes crash.

- If you are using rsync for state transfer, and a node crashes before the
  state transfer is over, rsync process might hang forever, occupying the port
  and not allowing to restart the node. The problem will show up as 'port in
  use' in the server error log. Find the orphan rsync process and kill it
  manually.

- Performance: by design performance of the cluster cannot be higher than
  performance of the slowest node; however, even if you have only one node, its
  performance can be considerably lower comparing to running the same server in
  a standalone mode (without wsrep provider). It is particularly true for big
  enough transactions (even those which are well within current limitations on
  transaction size quoted above).

- Windows is not supported.

- Replication filters: When using Galera cluster, replication filters should be used with caution. See [Configuring MariaDB Galera Cluster: Replication Filters](/kb/en/configuring-mariadb-galera-cluster/#replication-filters) for more details. See also [MDEV-421](https://jira.mariadb.org/browse/MDEV-421) and [MDEV-6229](https://jira.mariadb.org/browse/MDEV-6229).

- Flashback isn't supported in Galera due to incompatible binary log format.

- <code class="fixed" style="white-space:pre-wrap">FLUSH PRIVILEGES</code> is not replicated.

- The [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache) needed to be disabled by setting <a undefined>query_cache_size=0</a> prior to MariaDB Galera Cluster 5.5.40, MariaDB Galera Cluster 10.0.14, and [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)..

- In an asynchronous replication setup where a master replicates to a galera node acting as slave, parallel replication (slave-parallel-threads &gt; 1) on slave is currently not supported (see [MDEV-6860](https://jira.mariadb.org/browse/MDEV-6860)).

- The disk-based [Galera gcache](https://galeracluster.com/library/documentation/state-transfer.html#write-set-cache-gcache) is not encrypted ([MDEV-8072](https://jira.mariadb.org/browse/MDEV-8072)).

- Nodes may have different table definitions, especially temporarily during [rolling schema upgrade](/kb/en/galera-cluster-system-variables/#wsrep_osu_method) operations, but the same [schema compatibility restrictions](/replication/standard-replication/replication-when-the-master-and-slave-have-different-table-definitions) apply as they do for ROW  based replication