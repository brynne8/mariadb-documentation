# Using MariaDB GTIDs with MariaDB Galera Cluster

MariaDB's [global transaction IDs (GTIDs)](/replication/standard-replication/gtid/) are very useful when used with [MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/), which is primarily what that feature was developed for. [Galera Cluster](http://galeracluster.com/), on the other hand, was developed by Codership for all MySQL and MariaDB variants, and the initial development of the technology pre-dated MariaDB's [GTID](/replication/standard-replication/gtid/) implementation. As a side effect, [MariaDB Galera Cluster](/replication/galera-cluster/) (at least until [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)) only partially supports MariaDB's [GTID](/replication/standard-replication/gtid/) implementation.

## GTID Support for Write Sets Replicated by Galera Cluster

Galera Cluster has its own [certification-based replication method](/replication/galera-cluster/about-galera-replication/) that is substantially different from [MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/). However, it would still be beneficial if [MariaDB Galera Cluster](/replication/galera-cluster/) was able to associate a Galera Cluster write set with a [GTID](/replication/standard-replication/gtid/) that is globally unique, but that is also consistent for that write set on each cluster node.

Before [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/), [MariaDB Galera Cluster](/replication/galera-cluster/) did not replicate the original [GTID](/replication/standard-replication/gtid/) with the write set except in cases where the transaction was originally applied by a [slave SQL thread](/kb/en/replication-threads/#slave-sql-thread). Each node independently generated its own [GTID](/replication/standard-replication/gtid/) for each write set in most cases. See [MDEV-20720](https://jira.mariadb.org/browse/MDEV-20720).

### Wsrep GTID Mode

##### MariaDB starting with [10.1.4](/kb/en/mariadb-1014-release-notes/)

MariaDB has supported <a undefined>wsrep_gtid_mode</a> since version 10.1.4.

[MariaDB 10.1](/kb/en/what-is-mariadb-101/) and above has a feature called wsrep GTID mode. When this mode is enabled, MariaDB uses some tricks to try to associate each Galera Cluster write set with a [GTID](/replication/standard-replication/gtid/) that is globally unique, but that is also consistent for that write set on each cluster node. These tricks work in some cases, but [GTIDs](/replication/standard-replication/gtid/) can still become inconsistent among cluster nodes.

#### Enabling Wsrep GTID Mode

Several things need to be configured for wsrep GTID mode to work, such as:

- <a undefined>wsrep_gtid_mode=ON</a> needs to be set on all nodes in the cluster.

- <a undefined>wsrep_gtid_domain_id</a> needs to be set to the same value on all nodes in a given cluster, so that each cluster node uses the same domain when assigning [GTIDs](/replication/standard-replication/gtid/) for Galera Cluster's write sets. When replicating between two clusters, each cluster should have this set to a different value, so that each cluster uses different domains when assigning [GTIDs](/replication/standard-replication/gtid/) for their write sets.

- <a undefined>log_slave_updates</a> needs to be enabled on all nodes in the cluster. See [MDEV-9855](https://jira.mariadb.org/browse/MDEV-9855).

- <a undefined>log_bin</a> needs to be set to the same path on all nodes in the cluster. See [MDEV-9856](https://jira.mariadb.org/browse/MDEV-9856).

And as an extra safety measure:

- <a undefined>gtid_domain_id</a> should be set to a different value on all nodes in a given cluster, and each of these values should be different than the configured <a undefined>wsrep_gtid_domain_id</a> value. This is to prevent a node from using the same domain used for Galera Cluster's write sets when assigning [GTIDs](/replication/standard-replication/gtid/) for non-Galera transactions, such as DDL executed with <a undefined>wsrep_sst_method=RSU</a> set or DML executed with <a undefined>wsrep_on=OFF</a> set.

For information on setting <a undefined>server_id</a>, see [Using MariaDB Replication with MariaDB Galera Cluster: Setting server_id on Cluster Nodes](/kb/en/using-mariadb-replication-with-mariadb-galera-cluster-using-mariadb-replica/#setting-server_id-on-cluster-nodes).

#### Known Problems with Wsrep GTID Mode

Until [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/), there were known cases where [GTIDs](/replication/standard-replication/gtid/) could become inconsistent across the cluster nodes.

A known issue (fixed in [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)) is:

- Implicitly dropped temporary tables can make GTIDs inconsistent. See [MDEV-14153](https://jira.mariadb.org/browse/MDEV-14153) and [MDEV-20720](https://jira.mariadb.org/browse/MDEV-20720).

This does not necessarily imply that wsrep GTID mode works perfectly in all other situations. If you discover any other issues with it, please [report a bug](/kb/en/mariadb-community-bug-reporting/#reporting-a-bug).

### GTIDs for Transactions Applied by Slave Thread

If a Galera Cluster node is also a [replication slave](/replication/standard-replication/replication-overview/), then that node's [slave SQL thread](/kb/en/replication-threads/#slave-sql-thread) will be applying transactions that it replicates from its replication master. If the node has <a undefined>log_slave_updates=ON</a> set, then each transaction that the [slave SQL thread](/kb/en/replication-threads/#slave-sql-thread) applies will also generate a Galera Cluster write set that is replicated to the rest of the nodes in the cluster.

In [MariaDB 10.1.30](/kb/en/mariadb-10130-release-notes/) and earlier, the node acting as slave would apply the transaction with the original GTID that it received from the master, and the other Galera Cluster nodes would generate their own GTIDs for the transaction when they replicated the write set.

In [MariaDB 10.1.31](/kb/en/mariadb-10131-release-notes/) and later, the node acting as slave will include the transaction's original `Gtid_Log_Event` in the replicated write set, so all nodes should associate the write set with its original GTID. See [MDEV-13431](https://jira.mariadb.org/browse/MDEV-13431) about that.