# Configuring MariaDB Galera Cluster

A number of options need to be set in order for Galera Cluster to work when using MariaDB. These should be set in the [MariaDB option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/).

## Mandatory Options

Several options are mandatory, which means that they *must* be set in order for Galera Cluster to be enabled or to work properly with MariaDB. The mandatory options are:

- <a undefined>wsrep_provider</a> — Path to the Galera library
- <a undefined>wsrep_cluster_address</a> — See [Galera Cluster address format and usage](/replication/galera-cluster/galera-cluster-address/)
- <a undefined>binlog_format=ROW</a> — See [Binary Log Formats](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats/)
- <a undefined>default_storage_engine=InnoDB</a>
- <a undefined>innodb_autoinc_lock_mode=2</a>
- <a undefined>innodb_doublewrite=1</a> — This is the default value, but it should not be changed when using Galera provider version &gt;= 2.0.
- <a undefined>query_cache_size=0</a> — Only mandatory for MariaDB versions prior to MariaDB Galera Cluster 5.5.40, MariaDB Galera Cluster 10.0.14, and [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/).
- <a undefined>wsrep_on=ON</a> — Enable wsrep replication (starting 10.1.1)

## Performance-related Options

These are optional optimizations that can be made to improve performance.

- <a undefined>innodb_flush_log_at_trx_commit=0</a> — This is not usually recommended in the case of standard MariaDB. However, it is a bit safer with Galera Cluster, since inconsistencies can always be fixed by recovering from another node.

## Writing Replicated Write Sets to the Binary Log

Like with [MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/), write sets that are received by a node with [Galera Cluster's certification-based replication](/replication/galera-cluster/about-galera-replication/) are not written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) by default. If you would like a node to write its replicated write sets to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/), then you will have to set <a undefined>log_slave_updates=ON</a>. This is especially helpful if the node is a replication master. See [Using MariaDB Replication with MariaDB Galera Cluster: Configuring a Cluster Node as a Replication Master](https://mariadb.com/kb/en/library/using-mariadb-replication-with-mariadb-galera-cluster-using-mariadb-replica/#configuring-a-cluster-node-as-a-replication-master).

## Replication Filters

Like with [MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/), [replication filters](/replication/standard-replication/replication-filters/) can be used to filter write sets from being replicated by [Galera Cluster's certification-based replication](/replication/galera-cluster/about-galera-replication/). However, they should be used with caution because they may not work as you'd expect.

The following replication filters are honored for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) DML, but not DDL:

- <a undefined>binlog_do_db</a>
- <a undefined>binlog_ignore_db</a>
- <a undefined>replicate_wild_do_table</a>
- <a undefined>replicate_wild_ignore_table</a>

The following replication filters are honored for DML and DDL for tables that use both the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) and [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engines:

- <a undefined>replicate_do_table</a>
- <a undefined>replicate_ignore_table</a>

However, it should be kept in mind that if replication filters cause inconsistencies that lead to replication errors, then nodes may abort.

See also [MDEV-421](https://jira.mariadb.org/browse/MDEV-421) and [MDEV-6229](https://jira.mariadb.org/browse/MDEV-6229).

## Network Ports

Galera Cluster needs access to the following ports:

- <strong>Standard MariaDB Port</strong> (default: 3306) - For MySQL client connections and [State Snapshot Transfers](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts/) that use the `mysqldump` method. This can be changed by setting <a undefined>port</a>.
- <strong>Galera Replication Port</strong> (default: 4567) - For Galera Cluster replication traffic, multicast replication uses both UDP transport and TCP on this port. Can be changed by setting <a undefined>wsrep_node_address</a>.
- <strong>IST Port</strong> (default: 4568) - For Incremental State Transfers. Can be changed by setting <a undefined>ist.recv_addr</a> in <a undefined>wsrep_provider_options</a>.
- <strong>SST Port</strong> (default: 4444) - For all [State Snapshot Transfer](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts/) methods other than `mysqldump`. Can be changed by setting <a undefined>wsrep_sst_receive_address</a>.

## Mutiple Galera Cluster Instances on One Server

If you want to run multiple Galera Cluster instances on one server, then you can do so by starting each instance with [mysqld_multi](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_multi/), or if you are using [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/), then you can use the relevant [systemd method for interacting with multiple MariaDB instances](/kb/en/systemd/#interacting-with-multiple-mariadb-server-processes).

You need to ensure that each instance is configured with a different <a undefined>datadir</a>.

You also need to ensure that each instance is configured with different [network ports](#network-ports).