# About Galera Replication

In MariaDB Cluster, the Server replicates a transaction at commit time by broadcasting the write set associated with the transaction to every node in the cluster. The client connects directly to the DBMS and experiences behavior that is similar to native MariaDB in most cases. The wsrep API (write set replication API) defines the interface between Galera replication and MariaDB.

## Synchronous vs. Asynchronous Replication

The basic difference between synchronous and asynchronous replication is that
"synchronous" replication guarantees that if a change happened on one node in the cluster,
then the change will happen on other nodes in the cluster "synchronously", or at the same time.
"Asynchronous" replication gives no guarantees about the delay between applying changes on
"master" node and the propagation of changes to "slave" nodes. The delay with "asynchronous" replication can be short or long. This also implies that if master node crashes in an "asynchronous" replication topology, then some of the latest changes may be lost.

Theoretically, synchronous replication has a number of advantages over
asynchronous replication:

- Clusters utilizing synchronous replication are always highly available. If one of the nodes crashed, then there would be no data loss. Additionally, all cluster nodes are always consistent.
- Clusters utilizing synchronous replication allow transactions to be executed on all nodes in parallel.
- Clusters utilizing synchronous replication can guarantee causality across the whole cluster. This means that if a `SELECT` is executed on one cluster node after a transaction is executed on a cluster node, it should see the effects of that transaction.

However, in practice, synchronous database replication has traditionally been
implemented via the so-called "2-phase commit" or distributed locking which
proved to be very slow. Low performance and complexity of implementation of
synchronous replication led to a situation where asynchronous replication
remains the dominant means for database performance scalability and
availability. Widely adopted open-source databases such as MySQL or PostgreSQL
offer only asynchronous or semi-synchronous replication solutions.

Galera's replication is not completely synchronous. It is sometimes called <strong>virtually synchronous</strong> replication.

## Certification-Based Replication Method

An alternative approach to synchronous replication that uses Group
Communication and transaction ordering techniques was suggested by a number of
researchers. For example:

- [Database State Machine Approach](http://library.epfl.ch/theses/?nr=2090)
- [Don't Be Lazy, Be Consistent](http://www.cs.mcgill.ca/~kemme/papers/vldb00.html)

Prototype implementations have shown a lot of promise. We combined our
experience in synchronous database replication and the latest research in the
field to create the Galera Replication library and the wsrep API.

Galera replication is a <strong>highly transparent</strong>, <strong>scalable</strong>, and <strong>virtually synchronous</strong>
replication solution for database clustering to achieve high availability
and improved performance. Galera-based clusters are:

- Highly available
- Highly transparent
- Highly scalable (near linear scalability may be reached depending on the application)

## Generic Replication Library

Galera replication functionality is implemented as a shared library and can be
linked with any transaction processing system which implements the wsrep API
hooks.

The Galera replication library is a protocol stack providing functionality for
preparing, replicating and applying of transaction write sets. It consists of:

- <strong>wsrep API</strong> specifies the interface — responsibilities for DBMS and
  replication provider
- <strong>wsrep hooks</strong> is the wsrep integration in the DBMS engine.
- <strong>Galera provider</strong> implements the wsrep API for Galera library
- <strong>certification</strong> layer takes care of preparing write sets and performing
  certification
- <strong>replication</strong> manages replication protocol and provides total ordering
  capabilities
- <strong>GCS framework</strong> provides plugin architecture for group communication
  systems
- many gcs implementations can be adapted, we have experimented with spread and
  our in-house implementations: vsbes and gemini

Many components in the Galera replication library were redesigned and improved with the introduction of [MariaDB 10.4](/kb/en/what-is-mariadb-104/), which includes Galera 4.

## Galera Slave Threads

Although the <strong>Galera provider</strong> certifies the write set associated with a transaction at commit time on each node in the cluster, this write set is not necessarily applied on that cluster node immediately. Instead, the write set is placed in the cluster node's receive queue on the node, and it is eventually applied by one of the cluster node's Galera slave thread.

The number of Galera slave threads can be configured with the [wsrep_slave_threads](/kb/en/galera-cluster-system-variables/#wsrep_slave_threads) system variable.

The Galera slave threads are able to determine which write sets are safe to apply in parallel. However, if your cluster nodes seem to have frequent consistency problems, then setting the value to `1` will probably fix the problem.

When a cluster node's state, as seen by [wsrep_local_state_comment](/kb/en/galera-cluster-status-variables/#wsrep_local_state_comment), is `JOINED`, then increasing the number of slave threads may help the cluster node catch up with the cluster more quickly. In this case, it may be useful to set the number of threads to twice the number of CPUs on the system.

## Streaming Replication

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

Streaming replication was introduced in Galera 4, and so is only available from [MariaDB 10.4](/kb/en/what-is-mariadb-104/).

In older versions of MariaDB Cluster there was a 2GB limit on the size of the transaction you could run.  The node waits on the transaction commit before performing replication and certification.  With large transactions, long running writes, and changes to huge data-sets, there was a greater possibility of a conflict forcing rollback on an expensive operation.

Using Streaming replication, the node breaks huge transactions up into smaller and more manageable fragments, it then replicates these fragments to the cluster as it works instead of waiting for the commit.  Once certified, the fragment can no longer be aborted by conflicting transactions.  As this can have performance consequences both during execution and in the event of rollback, it is recommended that you only use it with large transactions that are unlikely to experience conflict.

For more information on Streaming Replication, see the [Galera](https://galeracluster.com/library/documentation/streaming-replication.html) documentation.

## Group Commits

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

Group Commit support for MariaDB Cluster was introduced in Galera 4, and so is only available from [MariaDB 10.4](/kb/en/what-is-mariadb-104/).

In MariaDB Group Commit, groups of transactions are flushed together to disk to improve performance. Prior to [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this feature was not available in MariaDB Cluster as it interfered with the global-ordering of transactions for replication. Beginning in 10.4, MariaDB Cluster can take advantage of Group Commit.

For more information on Group Commit, see the [Galera](https://galeracluster.com/library/kb/group-commit.html) documentation.

## See Also

- [Galera Cluster: Galera Replication](https://galeracluster.com/products/)
- [What is MariaDB Galera Cluster?](/replication/galera-cluster/what-is-mariadb-galera-cluster/)
- [Galera Use Cases](/replication/galera-cluster/galera-use-cases/)
- [Getting Started with MariaDB/Galera Cluster](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster/)