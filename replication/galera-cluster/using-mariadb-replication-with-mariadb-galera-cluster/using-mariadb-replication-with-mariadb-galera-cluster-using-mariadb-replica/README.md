# Using MariaDB Replication with MariaDB Galera Cluster

[MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/) and [MariaDB Galera Cluster](/replication/galera-cluster/) can be used together. However, there are some things that have to be taken into account.

## Tutorials

If you want to use [MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/) and [MariaDB Galera Cluster](/replication/galera-cluster/)  together, then the following tutorials may be useful:

- [Configuring MariaDB Replication between MariaDB Galera Cluster and MariaDB Server](/replication/galera-cluster/using-mariadb-replication-with-mariadb-galera-cluster/using-mariadb-replication-with-mariadb-galera-cluster-configuring-mariadb-r/)
- [Configuring MariaDB Replication between Two MariaDB Galera Clusters](/replication/galera-cluster/using-mariadb-replication-with-mariadb-galera-cluster/configuring-mariadb-replication-between-two-mariadb-galera-clusters/)

## Configuring a Cluster Node as a Replication Master

If a Galera Cluster node is also a [replication master](/replication/standard-replication/replication-overview/), then some additional configuration may be needed.

Like with [MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/), write sets that are received by a node with [Galera Cluster's certification-based replication](/replication/galera-cluster/about-galera-replication/) are not written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) by default.

If the node is a replication master, then its replication slaves only replicate transactions which are in the binary log, so this means that the transactions that correspond to Galera Cluster write-sets would not be replicated by any replication slaves by default. If you would like a node to write its replicated write sets to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/), then you will have to set <a undefined>log_slave_updates=ON</a>. If the node has any replication slaves, then this would also allow those slaves to replicate the transactions that corresponded to those write sets.

See [Configuring MariaDB Galera Cluster: Writing Replicated Write Sets to the Binary Log](/kb/en/configuring-mariadb-galera-cluster/#writing-replicated-write-sets-to-the-binary-log) for more information.

## Configuring a Cluster Node as a Replication Slave

If a Galera Cluster node is also a [replication slave](/replication/standard-replication/replication-overview/), then some additional configuration may be needed.

If the node is a replication slave, then the node's [slave SQL thread](/kb/en/replication-threads/#slave-sql-thread) will be applying transactions that it replicates from its replication master. Transactions applied by the slave SQL thread will only generate Galera Cluster write-sets if the node has <a undefined>log_slave_updates=ON</a> set. Therefore, in order to replicate these transactions to the rest of the nodes in the cluster, <a undefined>log_slave_updates=ON</a> must be set.

If the node is a replication slave, then it is probably also a good idea to enable <a undefined>wsrep_restart_slave</a>. When this is enabled, the node will restart its [slave threads](/kb/en/replication-threads/#threads-on-the-slave) whenever it rejoins the cluster.

## Replication Filters

Both [MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/) and [MariaDB Galera Cluster](/replication/galera-cluster/) support [replication filters](/replication/standard-replication/replication-filters/), so extra caution must be taken when using all of these features together. See [Configuring MariaDB Galera Cluster: Replication Filters](/kb/en/configuring-mariadb-galera-cluster/#replication-filters) for more details on how MariaDB Galera Cluster interprets replication filters.

## Setting server_id on Cluster Nodes

### Setting the Same server_id on Each Cluster Node

It is most common to set <a undefined>server_id</a> to the same value on each node in a given cluster. Since [MariaDB Galera Cluster](/replication/galera-cluster/) uses a [virtually synchronous certification-based replication](/replication/galera-cluster/about-galera-replication/), all nodes should have the same data, so in a logical sense, a cluster can be considered in many cases a single logical server for purposes related to [MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/). The [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/) of each cluster node might even contain roughly the same transactions and [GTIDs](/replication/standard-replication/gtid/) if <a undefined>log_slave_updates=ON</a> is set and if [wsrep GTID mode](/kb/en/using-mariadb-gtids-with-mariadb-galera-cluster/#wsrep-gtid-mode) is enabled and if non-Galera transactions are not being executed on any nodes.

### Setting a Different server_id on Each Cluster Node

There are cases when it might make sense to set a different <a undefined>server_id</a> value on each node in a given cluster. For example, if <a undefined>log_slave_updates=OFF</a> is set and if another cluster or a standard MariaDB Server is using [multi-source replication](/replication/standard-replication/multi-source-replication/) to replicate transactions from each cluster node individually, then it would be required to set a different <a undefined>server_id</a> value on each node for this to work.

Keep in mind that if replication is set up in a scenario where each cluster node has a different <a undefined>server_id</a> value, and if the replication topology is set up in such a way that a cluster node can replicate the same transactions through Galera and through MariaDB replication, then you may need to configure the cluster node to ignore these transactions when setting up MariaDB replication. You can do so by setting <a undefined>IGNORE_SERVER_IDS</a> to the server IDs of all nodes in the same cluster when executing [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/). For example, this might be required when circular replication is set up between two separate clusters, and each cluster node as a different <a undefined>server_id</a> value, and each cluster has <a undefined>log_slave_updates=ON</a> set.