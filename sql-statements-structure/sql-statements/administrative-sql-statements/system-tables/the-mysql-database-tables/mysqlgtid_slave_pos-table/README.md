# mysql.gtid_slave_pos Table

##### MariaDB starting with [10.0.3](/kb/en/mariadb-1003-release-notes/)

Global transaction IDs (GTIDs) for replication have been supported since [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/), and the `gtid_slave_pos` table since [MariaDB 10.0.3](/kb/en/mariadb-1003-release-notes/)

The `mysql.gtid_slave_pos` table is used in [replication](/replication/) by slave servers to keep track of their current position (the global transaction ID of the last transaction applied). Using the table allows the slave to maintain a consistent value for the [gtid_slave_pos](/kb/en/global-transaction-id/#gtid_slave_pos) system variable across server restarts. See [Global Transaction ID](/kb/en/global-transaction-id/).

You should never attempt to modify the table directly. If you do need to change the global gtid_slave_pos value, use `SET GLOBAL gtid_slave_pos = ...` instead.

The table is updated with the new position as part of each transaction
committed during replication. This makes it preferable that the table is
using the same storage engine as the tables otherwise being modified in the
transaction, since otherwise a multi-engine transaction is needed that can
reduce performance.

Starting from [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), multiple versions of this table are supported,
each using a different storage engine. This is selected with the
[gtid_pos_auto_engines option](/kb/en/global-transaction-id/#gtid_pos_auto_engines), by giving a comma-separated list of engine
names. The server will then on-demand create an extra version of the table
using the appropriate storage engine, and select the table version using the
same engine as the rest of the transaction, avoiding multi-engine
transactions.

For example, when `gtid_pos_auto_engines=innodb,rocksdb`, tables
`mysql.gtid_slave_pos_InnoDB` and `mysql.gtid_slave_pos_RocksDB` will be created
and used, if needed. If there is no match to the storage engine, the
default `mysql.gtid_slave_pos` table will be used; this also happens if
non-transactional updates (like MyISAM) are replicated, since there is then
no active transaction at the time of the `mysql.gtid_slave_pos` table update.

Prior to [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), only the default `mysql.gtid_slave_pos` table is
available. In these versions, the table should preferably be using the
storage engine that is used for most replicated transactions.

The default `mysql.gtid_slave_pos` table will be initially created using the
default storage engine set for the server (which itself defaults to InnoDB).
If the application load is primarily non-transactional MyISAM or Aria
tables, it can be beneficial to change the storage engine to avoid including
an InnoDB update with every operation:

```sql
ALTER TABLE mysql.gtid_slave_pos ENGINE=MyISAM;
```

The `mysql.gtid_slave_pos` table should not be changed manually in any other
way. From [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), it is preferable to use the `gtid_pos_auto_engines`
server variable to get the GTID position updates to use the TokuDB or
RocksDB storage engine.

Note that for scalability reasons, the automatic creation of a new
`mysql.gtid_slave_posXXX` table happens asynchronously when the first
transaction with the new storage engine is committed. So the very first few
transactions will update the old version of the table, until the new version
is created and available.

The table `mysql.gtid_slave_pos` contains the following fields

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>domain_id</code></td><td><code>int(10) unsigned</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Domain id (see <a href="/kb/en/global-transaction-id/#the-domain-id">Global Transaction ID domain ID</a>.</td></tr>
<tr><td><code>sub_id</code></td><td><code>bigint(20) unsigned</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>This field enables multiple parallel transactions within same <code>domain_id</code> to update this table without contention. At any instant, the replication state corresponds to records with largest <code>sub_id</code> for each <code>domain_id</code>.</td></tr>
<tr><td><code>server_id</code></td><td><code>int(10) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td><td><a href="/kb/en/global-transaction-id/#server_id">Server id</a>.</td></tr>
<tr><td><code>seq_no</code></td><td><code>bigint(20) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Sequence number, an integer that is monotonically increasing for each new event group logged into the binlog.</td></tr>
</tbody></table>

From [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), some status variables are available to monitor the use
of the different `gtid_slave_pos` table versions:

[Transactions_gtid_foreign_engine](/kb/en/replication-and-binary-log-status-variables/#transactions_gtid_foreign_engine)

Number of replicated transactions where the update of the `gtid_slave_pos`
  table had to choose a storage engine that did not otherwise participate in
  the transaction. This can indicate that setting gtid_pos_auto_engines
  might be useful.

[Rpl_transactions_multi_engine](/kb/en/replication-and-binary-log-status-variables/#rpl_transactions_multi_engine)

Number of replicated transactions that involved changes in multiple
  (transactional) storage engines, before considering the update of
  `gtid_slave_pos`. These are transactions that were already cross-engine,
  independent of the GTID position update introduced by replication

[Transactions_multi_engine](/kb/en/replication-and-binary-log-status-variables/#transactions_multi_engine)

Number of transactions that changed data in multiple (transactional)
  storage engines. If this is significantly larger than
  Rpl_transactions_multi_engine, it indicates that setting
  `gtid_pos_auto_engines` could reduce the need for cross-engine transactions.