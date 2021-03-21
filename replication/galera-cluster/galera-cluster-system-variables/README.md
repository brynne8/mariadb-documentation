# Galera Cluster System Variables

This page documents system variables related to [Galera Cluster](/kb/en/galera/). For options that are not system variables, see [Galera Options](/kb/en/mysqld-options/#galera-options).

See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables) for a complete list of system variables and instructions on setting them.

Also see the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables).

#### `wsrep_auto_increment_control`

- <strong>Description:</strong> If set to `1` (the default), will automatically adjust the [auto_increment_increment](/kb/en/replication-and-binary-log-server-system-variables/#auto_increment_increment) and [auto_increment_offset](/kb/en/replication-and-binary-log-server-system-variables/#auto_increment_offset) variables according to the size of the cluster, and when the cluster size changes. This avoids replication conflicts due to [auto_increment](/columns-storage-engines-and-plugins/data-types/auto_increment). In a master-slave environment, can be set to `OFF`.
- <strong>Commandline:</strong> `--wsrep-auto-increment-control[={0|1}]`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `ON`

---

#### `wsrep_causal_reads`

- <strong>Description:</strong> If set to `ON` (`OFF` is default), enforces [read-committed](/kb/en/set-transaction/#read-committed) characteristics across the cluster. In the case that a master applies an event more quickly than a slave, the two could briefly be out-of-sync. With this variable set to `ON`, the slave will wait for the event to be applied before processing further queries. Setting to `ON` also results in larger read latencies. Deprecated by [wsrep_sync_wait=1](#wsrep_sync_wait).
- <strong>Commandline:</strong> `--wsrep-causal-reads[={0|1}]`
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `OFF`

---

#### `wsrep_certification_rules`

- <strong>Description:</strong> Certification rules to use in the cluster. Possible values are: 
<ul start="1"><li>`strict`: Stricter rules that could result in more certification failures. For example with foreign keys, certification failure could result if different nodes receive non-conflicting insertions at about the same time that point to the same row in a parent table
</li><li>`optimized`: relaxed rules that allow more concurrency and cause less certification failures. 
</li></ul>
- <strong>Commandline:</strong> `--wsrep-certifcation-rules`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Enumeration
- <strong>Default Value:</strong> `strict`
- <strong>Valid Values:</strong> `strict`, `optimized`
- <strong>Introduced:</strong> [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/), [MariaDB 10.3.13](/kb/en/mariadb-10313-release-notes/), [MariaDB 10.2.22](/kb/en/mariadb-10222-release-notes/), [MariaDB 10.1.38](/kb/en/mariadb-10138-release-notes/)

---

#### `wsrep_certify_nonPK`

- <strong>Description:</strong> When set to `ON` (the default), Galera will still certify transactions for tables with no [primary key](/kb/en/getting-started-with-indexes/#primary-key). However, this can still cause undefined behavior in some circumstances. It is recommended to define primary keys for every InnoDB table when using Galera.
- <strong>Commandline:</strong> `--wsrep-certify-nonPK[={0|1}]`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `ON`

---

#### `wsrep_cluster_address`

- <strong>Description:</strong> The addresses of cluster nodes to connect to when starting up, for example `gcomm:<em>/</em>/192.168.0.1:1234?gmcast.listen_addr=0.0.0.0:2345`. Good practice is to specify all possible cluster nodes, in the form `gcomm:<em>/</em>/&lt;node1 or ip:port&gt;,&lt;node2 or ip2:port&gt;,&lt;node3 or ip3:port&gt;`. Specifying an empty ip (`gcomm:<em>/</em>/`) will cause the node to start a new cluster (which should not be done in the my.cnf file, as after each restart the server will not rejoin the current cluster). The variable can be changed at runtime in some configurations, and will result in the node closing the connection to any current cluster, and connecting to the new address. If specifying a port, note that this is the Galera port, not the MariaDB port.
- <strong>Commandline:</strong> `--wsrep-cluster-address=value`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> String

---

#### `wsrep_cluster_name`

- <strong>Description:</strong> The name of the cluster. Nodes cannot connect to clusters with a different name, so needs to be identical on all nodes in the same cluster.
- <strong>Commandline:</strong> `--wsrep-cluster-name=value`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> `my_wsrep_cluster`

---

#### `wsrep_convert_LOCK_to_trx`

- <strong>Description:</strong> Converts [LOCK/UNLOCK TABLES](/kb/en/lock-tables-and-unlock-tables/) statements to [BEGIN](/sql-statements-structure/sql-statements/transactions/start-transaction) and [COMMIT](/sql-statements-structure/sql-statements/transactions/commit). Used mainly for getting older applications to work with a multi-master setup, use carefully, as can result in extremely large writesets.
- <strong>Commandline:</strong> `--wsrep-convert-LOCK-to-trx[={0|1}]`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `OFF`

---

#### `wsrep_data_home_dir`

- <strong>Description:</strong> Directory where wsrep provider will store its internal files.
- <strong>Commandline:</strong> `--wsrep-data-home-dir=value`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> The [datadir](/kb/en/server-system-variables/#datadir) variable value.

---

#### `wsrep_dbug_option`

- <strong>Description:</strong> Used to pass the DBUG options to the wsrep provider.
- <strong>Commandline:</strong> `--wsrep-dbug-option=value`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> String

---

#### `wsrep_debug`

- <strong>Description:</strong> WSREP debug level logging. Before [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) was a boolean, which when set to `ON` (`OFF` was default), ensured debug messages would be logged to the [error log](/mariadb-administration/server-monitoring-logs/error-log) as well.
- <strong>Commandline:</strong> `--wsrep-debug[={0|1}]`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Enumeration (&gt;= [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)), Boolean (&lt;= [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/))
- <strong>Default Value:</strong> `NONE` (&gt;= [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)),  `OFF` (&lt;= [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/))
- <strong>Valid Values</strong>: (&gt;= [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)) `NONE`, `SERVER`, `TRANSACTION`, `STREAMING`, `CLIENT`

---

#### `wsrep_desync`

- <strong>Description:</strong> When a node receives more write-sets than it can apply, the transactions are placed in a received queue. If the node's received queue has too many write-sets waiting to be applied (as defined by the <a undefined>gcs.fc_limit</a> WSREP provider option), then the node would usually engage Flow Control. However, when this option is set to `ON`, Flow Control will be disabled for the desynced node. The desynced node works through the received queue until it reaches a more manageable size. The desynced node continues to receive write-sets from the other nodes in the cluster. The other nodes in the cluster do not wait for the desynced node to catch up, so the desynced node can fall even further behind the other nodes in the cluster. You can check if a node is desynced by checking if the <a undefined>wsrep_local_state_comment</a> status variable is equal to `Donor/Desynced`.
- <strong>Commandline:</strong> `--wsrep-desync[={0|1}]`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `OFF`

---

#### `wsrep_dirty_reads`

- <strong>Description:</strong> By default, when not synchronized with the group ([wsrep_ready](/kb/en/galera-cluster-status-variables/#wsrep_ready)=OFF) a node will reject all queries other than SET and SHOW. If `wsrep_dirty_reads` is set to `1`, queries which do not change data, like SELECT queries (dirty reads), creating of prepare statement, etc. will be accepted by the node.
- <strong>Commandline:</strong> `--wsrep-dirty-reads[={0|1}]`
- <strong>Scope:</strong> Global,Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `OFF`
- <strong>Valid Values:</strong> `ON`, `OFF`
- <strong>Introduced:</strong> [MariaDB Galera 5.5.42](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5542-release-notes), [MariaDB Galera 10.0.16](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10016-release-notes), [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `wsrep_drupal_282555_workaround`

- <strong>Description:</strong> If set to `ON`, a workaround for [Drupal/MySQL/InnoDB bug #282555](https://www.drupal.org/node/282555) is enabled. This is a bug where, in some cases, when inserting a `DEFAULT` value into an [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment) column, a duplicate key error may be returned.
- <strong>Commandline:</strong> `--wsrep-drupal-282555-workaround[={0|1}]`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `OFF`

---

#### `wsrep_forced_binlog_format`

- <strong>Description:</strong> A [binary log format](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats) that will override any session binlog format settings.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-forced-binlog-format=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Default Value:</strong> `NONE`
- <strong>Data Type:</strong> Enum
- <strong>Valid Values:</strong> `STATEMENT`, `ROW`, `MIXED` or `NONE` (which resets the forced binlog format state).

---

#### `wsrep_gtid_domain_id`

- <strong>Description:</strong> This system variable defines the [GTID](/replication/standard-replication/gtid) domain ID that is used for [wsrep GTID mode](/kb/en/using-mariadb-gtids-with-mariadb-galera-cluster/#wsrep-gtid-mode).
<ul start="1"><li>When <a undefined>wsrep_gtid_mode</a> is set to `ON`, `wsrep_gtid_domain_id` is used in place of <a undefined>gtid_domain_id</a> for all Galera Cluster write sets.
</li><li>When <a undefined>wsrep_gtid_mode</a> is set to `OFF`, `wsrep_gtid_domain_id` is simply ignored to allow for backward compatibility.
</li><li>There are some additional requirements that need to be met in order for this mode to generate consistent [GTIDs](/replication/standard-replication/gtid). For more information, see [Using MariaDB GTIDs with MariaDB Galera Cluster](/replication/galera-cluster/using-mariadb-replication-with-mariadb-galera-cluster/using-mariadb-gtids-with-mariadb-galera-cluster).
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-gtid-domain-id=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `wsrep_gtid_mode`

- <strong>Description:</strong> [Wsrep GTID mode](/kb/en/using-mariadb-gtids-with-mariadb-galera-cluster/#wsrep-gtid-mode) attempts to keep [GTIDs](/replication/standard-replication/gtid) consistent for Galera Cluster write sets on all cluster nodes. [GTID](/replication/standard-replication/gtid) state is initially copied to a joiner node during an [SST](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts). If you are planning to use Galera Cluster with [MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/), then wsrep GTID mode can be helpful.
<ul start="1"><li>When `wsrep_gtid_mode` is set to `ON`, <a undefined>wsrep_gtid_domain_id</a> is used in place of <a undefined>gtid_domain_id</a> for all Galera Cluster write sets.
</li><li>When `wsrep_gtid_mode` is set to `OFF`, <a undefined>wsrep_gtid_domain_id</a> is simply ignored to allow for backward compatibility.
</li><li>There are some additional requirements that need to be met in order for this mode to generate consistent [GTIDs](/replication/standard-replication/gtid). For more information, see [Using MariaDB GTIDs with MariaDB Galera Cluster](/replication/galera-cluster/using-mariadb-replication-with-mariadb-galera-cluster/using-mariadb-gtids-with-mariadb-galera-cluster).
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-gtid-mode[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `wsrep_gtid_seq_no`

- <strong>Description:</strong> Internal server usage, manually set WSREP GTID seqno.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Session only
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Range:</strong> `0` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)

---

#### `wsrep_ignore_apply_errors`

- <strong>Description:</strong> Bitmask determining whether errors are ignored, or reported back to the provider.
<ul><li>0: No errors are skipped.
</li><li>1: Ignore some DDL errors (DROP DATABASE, DROP TABLE, DROP INDEX, ALTER TABLE).
</li><li>2: Skip DML errors (Only ignores DELETE errors).
</li><li>4: Ignore all DDL errors.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-ignore-apply-errors</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> `7`
- <strong>Range:</strong> `0` to `7`
- <strong>Introduced:</strong> [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/)

---

#### `wsrep_load_data_splitting`

- <strong>Description:</strong> If set to `ON` (the default for [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/) and before), [LOAD DATA INFILE](/kb/en/load-data/) supports big data files by introducing transaction splitting. The setting has been deprecated in Galera 4, and defaults to `OFF` from [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-load-data-splitting[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `OFF` (&gt;= [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/)), `ON` (&lt;= [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/))
- <strong>Introduced:</strong> [MariaDB Galera 5.5.32](/kb/en/mariadb-galera-cluster-5532-release-notes/), [MariaDB Galera 10.0.7](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-1007-release-notes), [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `wsrep_log_conflicts`

- <strong>Description:</strong> If set to `ON` (`OFF` is default), details of conflicting MDL as well as InnoDB locks in the cluster will be logged.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-log-conflicts[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `OFF`

---

#### `wsrep_max_ws_rows`

- <strong>Description:</strong> Maximum permitted number of rows per writeset. Before [MariaDB Galera 10.0.27](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10027-release-notes) and [MariaDB 10.1.17](/kb/en/mariadb-10117-release-notes/) this variable was ignored internally and had no effect on the node. From [MariaDB Galera 10.0.27](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10027-release-notes) and [MariaDB 10.1.17](/kb/en/mariadb-10117-release-notes/) support for this variable has been added and in order to be backward compatible the default value has been changed to `0`, which essentially allows writesets to be any size.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-max-ws-rows=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> 
<ul start="1"><li>`0` (&gt;= [MariaDB Galera 10.0.27](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10027-release-notes), [MariaDB 10.1.17](/kb/en/mariadb-10117-release-notes/)) 
</li><li>`131072` (&lt;= [MariaDB Galera 10.0.26](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10026-release-notes), [MariaDB 10.1.16](/kb/en/mariadb-10116-release-notes/))
</li></ul>
- <strong>Range:</strong> `0` to `1048576`

---

#### `wsrep_max_ws_size`

- <strong>Description:</strong> Maximum permitted size in bytes per writeset. Writesets exceeding this will be rejected. Note that versions from and before [MariaDB 10.1.17](/kb/en/mariadb-10117-release-notes/) and [MariaDB Galera 10.0.27](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10027-release-notes) permitted the maximum to be set beyond 2GB, which was rejected by Galera.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-max-ws-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> 
<ul start="1"><li>`2147483647` (2GB, &gt;= [MariaDB Galera 10.0.27](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10027-release-notes), [MariaDB 10.1.17](/kb/en/mariadb-10117-release-notes/))
</li><li>`1073741824` (1GB, &lt;= [MariaDB Galera 10.0.26](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10026-release-notes), [MariaDB 10.1.16](/kb/en/mariadb-10116-release-notes/))
</li></ul>
- <strong>Range:</strong> `1024` to `2147483647`

---

#### `wsrep_mode`

- <strong>Description:</strong> Turns on WSREP features which are not part of default behavior.
<ul><li>BINLOG_ROW_FORMAT_ONLY: Only ROW [binlog format](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats) is supported.
</li><li>DISALLOW_LOCAL_GTID: Nodes can have GTIDs for local transactions in a number of scenarios. If DISALLOW_LOCAL_GTID is set, these operations produce an error ERROR HY000: Galera replication not supported. Scenarios include:
<ul><li>A DDL statement is executed with wsrep_OSU_method=RSU set.
</li><li>A DML statement writes to a non-InnoDB table.
</li><li>A DML statement writes to an InnoDB table with wsrep_on=OFF set.
</li></ul>
</li><li>REPLICATE_ARIA: Whether or not DML updates for [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) tables will be replicated. This functionality is experimental and should not be relied upon in production systems.
</li><li>REPLICATE_MYISAM: Whether or not DML updates for [MyISAM](/kb/en/myisam/) tables will be replicated. This functionality is experimental and should not be relied upon in production systems.
</li><li>REQUIRED_PRIMARY_KEY: Table should have PRIMARY KEY defined.
</li><li>STRICT_REPLICATION: Same as the old [wsrep_strict_ddl](#wsrep_strict_ddl) setting.
</li></ul>
- <strong>Commandline:</strong> `--wsrep-mode=value`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Enumeration
- <strong>Default Value:</strong> (Empty)
- <strong>Valid Values:</strong> `BINLOG_ROW_FORMAT_ONLY`, `DISALLOW_LOCAL_GTID`, `REQUIRED_PRIMARY_KEY`, `REPLICATE_ARIA`, `REPLICATE_MYISAM` and `STRICT_REPLICATION`
- <strong>Introduced:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `wsrep_mysql_replication_bundle`

- <strong>Description:</strong> Determines the number of replication events that are grouped together.  Experimental implementation aimed to assist with bottlenecks when a single slave faces a large commit time delay. If set to `0` (the default), there is no grouping.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-mysql-replication-bundle=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `1000`

---

#### `wsrep_node_address`

- <strong>Description:</strong> Specifies the node's network address, in the format `ip address[:port]`. As of [MariaDB 10.1.8](/kb/en/mariadb-1018-release-notes/), supports IPv6. The default behavior is for the node to pull the address of the first network interface on the system and the default Galera port. This autoguessing can be unreliable, particularly in the following cases:
<ul start="1"><li>cloud deployments
</li><li>container deployments
</li><li>servers with multiple network interfaces.
</li><li>servers running multiple nodes.
</li><li>network address translation (NAT).
</li><li>clusters with nodes in more than one region.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-node-address=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> Primary network address, usually `eth0` with a default port of 4567, or `0.0.0.0` if no IP address.

---

#### `wsrep_node_incoming_address`

- <strong>Description:</strong> This is the address from which the node listens for client connections. If an address is not specified or it's set to `AUTO` (default), mysqld uses either [--bind-address](/kb/en/server-system-variables/#bind_address) or [--wsrep-node-address](#wsrep_node_address), or tries to get one from the list of available network interfaces, in the same order.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-node-incoming-address=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> `AUTO`

---

#### `wsrep_node_name`

- <strong>Description:</strong> Name of this node. This name can be used in [wsrep_sst_donor](#wsrep_sst_donor) as a preferred donor. Note that multiple nodes in a cluster can have the same name.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-node-name=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> The server's hostname.

---

#### `wsrep_notify_cmd`

- <strong>Description:</strong> Command to be executed each time the node state or the cluster membership changes. Can be used for raising an alarm, configuring load balancers and so on. See the [Codership Notification Script page](https://galeracluster.com/library/documentation/notification-cmd.html) for more details.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-notify-command=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> 
<ul><li>No (&gt;= [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/), [MariaDB 10.4.18](/kb/en/mariadb-10418-release-notes/), [MariaDB 10.3.28](/kb/en/mariadb-10328-release-notes/), [MariaDB 10.2.37](/kb/en/mariadb-10237-release-notes/)) 
</li><li>Yes (&lt;= [MariaDB 10.5.8](/kb/en/mariadb-1058-release-notes/), [MariaDB 10.4.17](/kb/en/mariadb-10417-release-notes/), [MariaDB 10.3.27](/kb/en/mariadb-10327-release-notes/), [MariaDB 10.2.36](/kb/en/mariadb-10236-release-notes/))
</li></ul>
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> Empty

---

#### `wsrep_on`

- <strong>Description:</strong> Whether or not wsrep replication is enabled. If the global value is set to `OFF` (the default since [MariaDB 10.1](/kb/en/what-is-mariadb-101/)), it is not possible to load the provider and join the node in the cluster. If only the session value is set to `OFF`,  the operations from that particular session are not replicated in the cluster, but other sessions and applier threads will continue as normal. The session  value of the variable does not affect the node's membership and thus, regardless of its value, the node keeps receiving updates from other nodes in the cluster. Before [MariaDB 10.1](/kb/en/what-is-mariadb-101/), even though this variable is `ON` by default, its value gets automatically adjusted based on whether mandatory configurations to turn on Galera replication have been specified. Since [MariaDB 10.1](/kb/en/what-is-mariadb-101/), it is set to `OFF` by default and must be turned on to enable Galera replication.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-on[={0|1}]</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `OFF` (&gt;= [MariaDB 10.1](/kb/en/what-is-mariadb-101/)), `ON` (&lt;= MariaDB Galera Cluster 10.0),
- <strong>Valid Values:</strong> `ON`, `OFF`

---

#### `wsrep_OSU_method`

- <strong>Description:</strong> Online schema upgrade method. The default is `TOI`, specifying the setting without the optional parameter will set to `RSU`.
<ul start="1"><li>`TOI`: Total Order Isolation. In each cluster node, DDL is processed in the same order regarding other transactions, guaranteeing data consistency. However, affected parts of the database will be locked for the whole cluster.
</li><li>`RSU`: Rolling Schema Upgrade. DDL processing is only done locally on the node, and the user needs perform the changes manually on each node. The node is desynced from the rest of the cluster while the processing takes place to avoid the blocking other nodes. Schema changes [<em>must</em> be backwards compatible in the same way as for ROW based replication](/replication/standard-replication/replication-when-the-master-and-slave-have-different-table-definitions) to avoid breaking replication when the DDL processing is complete on the single node, and replication recommences.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-OSU-method[=value]</code>
- <strong>Scope:</strong> Global, Session (since [MariaDB Galera 10.0.19](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10019-release-notes))
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Enum
- <strong>Default Value:</strong> `TOI`
- <strong>Valid Values:</strong> `TOI`, `RSU`

---

#### `wsrep_patch_version`

- <strong>Description:</strong> Wsrep patch version, for example `wsrep_25.10`.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> None
- <strong>Introduced:</strong> [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/)

---

#### `wsrep_provider`

- <strong>Description:</strong> Location of the wsrep library, usually `/usr/lib/libgalera_smm.so` on Debian and Ubuntu, and `/usr/lib64/libgalera_smm.so` on Red Hat/CentOS.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-provider=value</code>
- <strong>Scope:</strong> Global
<ul><li>No (&gt;= [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/), [MariaDB 10.4.18](/kb/en/mariadb-10418-release-notes/), [MariaDB 10.3.28](/kb/en/mariadb-10328-release-notes/), [MariaDB 10.2.37](/kb/en/mariadb-10237-release-notes/)) 
</li><li>Yes (&lt;= [MariaDB 10.5.8](/kb/en/mariadb-1058-release-notes/), [MariaDB 10.4.17](/kb/en/mariadb-10417-release-notes/), [MariaDB 10.3.27](/kb/en/mariadb-10327-release-notes/), [MariaDB 10.2.36](/kb/en/mariadb-10236-release-notes/))
</li></ul>
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> None

---

#### `wsrep_provider_options`

- <strong>Description:</strong>  Semicolon (;) separated list of wsrep options (see [wsrep_provider_options](/replication/galera-cluster/wsrep_provider_options)).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-provider-options=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> Empty

---

#### `wsrep_recover`

- <strong>Description:</strong> If set to `ON` when the server starts, the server will recover the sequence number of the most recent write set applied by Galera, and it will be output to `stderr`, which is usually redirected to the [error log](/mariadb-administration/server-monitoring-logs/error-log). At that point, the server will exit. This sequence number can be provided to the <a undefined>wsrep_start_position</a> system variable.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-recover[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `OFF`

---

#### `wsrep_reject_queries`

- <strong>Description:</strong> Variable to set to reject queries from client connections, useful for maintenance. The node continues to apply write-sets, but an `Error 1047: Unknown command` error is generated by a client query.
<ul start="1"><li>`NONE` - Not set. Queries will be processed as normal.
</li><li>`ALL` - All queries from client connections will be rejected, but existing client connections will be maintained.
</li><li>`ALL_KILL` All queries from client connections will be rejected, and existing client connections, including the current one, will be immediately killed.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-reject-queries[=value]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Enum
- <strong>Default Value:</strong> `NONE`
- <strong>Valid Values:</strong> `NONE`, `ALL`, `ALL_KILL`
- <strong>Introduced:</strong> [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/), [MariaDB 10.2.14](/kb/en/mariadb-10214-release-notes/), [MariaDB 10.1.32](/kb/en/mariadb-10132-release-notes/)

---

#### `wsrep_replicate_myisam`

- <strong>Description:</strong> Whether or not DML updates for [MyISAM](/kb/en/myisam/) tables will be replicated. This functionality is still experimental and should not be relied upon in production systems. Deprecated in [MariaDB 10.6](/kb/en/what-is-mariadb-106/), use [wsrep_mode](#wsrep_mode) instead.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-replicate-myisam[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Default Value:</strong> `OFF`
- <strong>Data Type:</strong> Boolean
- <strong>Valid Values:</strong> `ON`, `OFF`
- <strong>Deprecated:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `wsrep_restart_slave`

- <strong>Description:</strong> If set to ON, the replication slave is restarted automatically, when node joins back to cluster.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-restart-slave[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Default Value:</strong> `OFF`
- <strong>Data Type:</strong> Boolean
- <strong>Introduced:</strong> [MariaDB Galera 5.5.39](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5539-release-notes), [MariaDB Galera 10.0.10](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10010-release-notes)

---

#### `wsrep_retry_autocommit`

- <strong>Description:</strong> Number of times autocommited queries will be retried due to cluster-wide conflicts before returning an error to the client. If set to `0`, no retries will be attempted, while a value of `1` (the default) or more specifies the number of retries attempted. Can be useful to assist applications using autocommit to avoid deadlocks.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-retry-autocommit=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `10000`

---

#### `wsrep_slave_FK_checks`

- <strong>Description:</strong> If set to ON (the default), the applier slave thread performs foreign key constraint checks.
- <strong>Commandline:</strong> `--wsrep-slave-FK-checks[={0|1}]`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> ON
- <strong>Introduced:</strong> [MariaDB Galera 5.5.39](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5539-release-notes), [MariaDB Galera 10.0.12](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10012-release-notes)

---

#### `wsrep_slave_threads`

- <strong>Description:</strong> Number of slave threads used to apply Galera write sets in parallel. The Galera slave threads are able to determine which write sets are safe to apply in parallel. However, if your cluster nodes seem to have frequent consistency problems, then setting the value to `1` will probably fix the problem. See [About Galera Replication: Galera Slave Threads](/kb/en/about-galera-replication/#galera-slave-threads) for more information.
- <strong>Commandline:</strong> `--wsrep-slave-threads=`#
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `1` to `512`

---

#### `wsrep_slave_UK_checks`

- <strong>Description:</strong> If set to ON, the applier slave thread performs secondary index uniqueness checks.
- <strong>Commandline:</strong> `--wsrep-slave-UK-checks[={0|1}]`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> OFF
- <strong>Introduced:</strong> [MariaDB Galera 5.5.39](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5539-release-notes), [MariaDB Galera 10.0.12](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10012-release-notes)

---

#### `wsrep_sr_store`

- <strong>Description:</strong> Storage for streaming replication fragments.
- <strong>Commandline:</strong> `--wsrep-sr-store=val`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> Enum
- <strong>Default Value:</strong> `table`
- <strong>Valid Values:</strong> `table`, `none`
- <strong>Introduced:</strong> [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/)

---

#### `wsrep_sst_auth`

- <strong>Description:</strong> Username and password of the user to use for replication. Unused if [wsrep_sst_method](#wsrep_sst_method) is set to `rsync`, while for other methods it should be in the format `&lt;user&gt;:&lt;password&gt;`. The contents are masked in logs and when querying the value with [SHOW VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables). See [Introduction to State Snapshot Transfers (SSTs)](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts) for more information.
- <strong>Commandline:</strong> `--wsrep-sst-auth=value`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> (Empty)

---

#### `wsrep_sst_donor`

- <strong>Description:</strong> Comma-separated list (from 5.5.33) or name (as per [wsrep_node_name](/kb/en/galera-cluster-system-variables/#wsrep_node_name)) of the servers as donors, or the source of the state transfer, in order of preference. The donor-selection algorithm, in general, prefers a donor capable of transferring only the missing transactions (IST) to the joiner node, instead of the complete state (SST). Thus, it starts by looking for an IST-capable node in the given donor list followed by rest of the nodes in the cluster. In case multiple candidate nodes are found outside the specified donor list, the node in the same segment ([gmcast.segment](/kb/en/wsrep_provider_options/#gmcastsegment)) as the joiner is preferred. If none of the existing nodes in the cluster can serve the missing transactions through IST, the algorithm moves on to look for a suitable node to transfer the entire state (SST). It first looks at the nodes specified in the donor list (irrespective of their segment). If no suitable donor is still found, the rest of the donor nodes are checked for suitability only if the donor list has a "terminating-comma". Note that a stateless node (the Galera arbitrator) can never be a donor. See [Introduction to State Snapshot Transfers (SSTs)](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts) for more information.
- <strong>Commandline:</strong> `--wsrep-sst-donor=value`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong>

---

#### `wsrep_sst_donor_rejects_queries`

- <strong>Description:</strong> If set to `ON` (`OFF` is default), the donor node will reject incoming queries, returning an `UNKNOWN COMMAND` error code. Can be used for informing load balancers that a node is unavailable.
- <strong>Commandline:</strong> `--wsrep-sst-donor-rejects-queries[={0|1}]`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `OFF`

---

#### `wsrep_sst_method`

- <strong>Description:</strong> Method used for taking the [state snapshot transfer (SST)](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts).  See [Introduction to State Snapshot Transfers (SSTs): SST Methods](/kb/en/introduction-to-state-snapshot-transfers-ssts/#sst-methods) for more information.
- <strong>Commandline:</strong> `--wsrep-sst-method=value`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> `rsync`
- <strong>Valid Values:</strong> `rsync`, `mysqldump`, `xtrabackup`, `xtrabackup-v2`, `mariabackup`

---

#### `wsrep_sst_receive_address`

- <strong>Description:</strong> This is the address where other nodes (donor) in the cluster connect to in order to send the state-transfer updates. If an address is not specified or its set to `AUTO` (default), mysqld uses [--wsrep_node_address](/kb/en/galera-cluster-system-variables/#wsrep_node_address)'s value as the receiving address.  However, if [--wsrep_node_address](/kb/en/galera-cluster-system-variables/#wsrep_node_address) is not set, it uses address from either [--bind-address](/kb/en/server-system-variables/#bind_address) or tries to get one from the list of available network interfaces, in the same order. Note: setting it to `localhost` will make it impossible for nodes running on other hosts to reach this node. See [Introduction to State Snapshot Transfers (SSTs)](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts) for more information.
- <strong>Commandline:</strong> `--wsrep-sst-receive-address=value`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> `AUTO`

---

#### `wsrep_start_position`

- <strong>Description:</strong> The start position that the node should use in the format: `UUID:seq_no`. The proper value to use for this position can be recovered with <a undefined>wsrep_recover</a>.
- <strong>Commandline:</strong> `--wsrep-start-position=value`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> ` 00000000-0000-0000-0000-000000000000:-1`

---

#### `wsrep_strict_ddl`

- <strong>Description:</strong> If set, reject DDL statements on affected tables not supporting Galera replication. This is done by checking if the table is InnoDB, which is the only table currently fully supporting Galera replication. MyISAM tables will not trigger the error if the experimental [wsrep_replicate_myisam](#wsrep_replicate_myisam) setting is `ON`. If set, should be set on all tables in the cluster. Affected DDL statements include:<br>
[CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) (e.g. CREATE TABLE t1(a int) engine=Aria)<br>
[ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table)<br>
[TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table)<br>
[CREATE VIEW](/programming-customizing-mariadb/views/create-view)<br>
[CREATE TRIGGER](/programming-customizing-mariadb/triggers-events/triggers/create-trigger)<br>
[CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index)<br>
[DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index)<br>
[RENAME TABLE](/sql-statements-structure/sql-statements/data-definition/rename-table)<br>
[DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table)<br>
Statements in [procedures](/programming-customizing-mariadb/stored-routines/stored-procedures), [events](/programming-customizing-mariadb/triggers-events/event-scheduler), and [functions](/programming-customizing-mariadb/stored-routines/stored-functions) are permitted as the affected
tables are only known at execution. Furthermore, the various USER, ROLE, SERVER and 
DATABASE statements are also allowed as they do not have an affected table. Deprecated in [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/). Use [wsrep_mode=STRICT_REPLICATION](#wsrep_mode) instead.
- <strong>Commandline:</strong> `--wsrep-strict-ddl[={0|1}`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)
- <strong>Deprecated:</strong> [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/)

---

#### `wsrep_sync_wait`

- <strong>Description:</strong> Setting this variable ensures causality checks will take place before executing an operation of the type specified by the value, ensuring that the statement is executed on a fully synced node. While the check is taking place, new queries are blocked on the node to allow the server to catch up with all updates made in the cluster up to the point where the check was begun. Once reached, the original query is executed on the node. This can result in higher latency. Note that when [wsrep_dirty_reads](#wsrep_dirty_reads) is ON, values of wsrep_sync_wait become irrelevant. Sample usage (for a critical read that must have the most up-to-date data) `SET SESSION wsrep_sync_wait=1; SELECT ...; SET SESSION wsrep_sync_wait=0;`
<ul><li>`0` - Disabled (default)
</li><li>`1` - READ (SELECT and BEGIN/START TRANSACTION). Up until [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/), [MariaDB 10.1.26](/kb/en/mariadb-10126-release-notes/), [MariaDB Galera 10.0.31](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10031-release-notes) and [MariaDB Galera 5.5.56](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5556-release-notes), also SHOW). This is the same as [wsrep_causal_reads=1](#wsrep_causal_reads).
</li><li>`2` - UPDATE and DELETE; 
</li><li>`3` - READ, UPDATE and DELETE;
</li><li>`4` - INSERT and REPLACE;
</li><li>`5` - READ, INSERT and REPLACE;
</li><li>`6` - UPDATE, DELETE, INSERT and REPLACE;
</li><li>`7` - READ, UPDATE, DELETE, INSERT and REPLACE;
</li><li>`8` - SHOW (from [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.1.27](/kb/en/mariadb-10127-release-notes/), [MariaDB Galera 10.0.32](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10032-release-notes), [MariaDB Galera 5.5.57](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5557-release-notes))
</li><li>`9` - READ and SHOW (from [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.1.27](/kb/en/mariadb-10127-release-notes/), [MariaDB Galera 10.0.32](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10032-release-notes), [MariaDB Galera 5.5.57](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5557-release-notes))
</li><li>`10` - UPDATE, DELETE and SHOW (from [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.1.27](/kb/en/mariadb-10127-release-notes/), [MariaDB Galera 10.0.32](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10032-release-notes), [MariaDB Galera 5.5.57](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5557-release-notes))
</li><li>`11` - READ, UPDATE, DELETE and SHOW (from [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.1.27](/kb/en/mariadb-10127-release-notes/), [MariaDB Galera 10.0.32](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10032-release-notes), [MariaDB Galera 5.5.57](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5557-release-notes))
</li><li>`12` - INSERT, REPLACE and SHOW (from [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.1.27](/kb/en/mariadb-10127-release-notes/), [MariaDB Galera 10.0.32](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10032-release-notes), [MariaDB Galera 5.5.57](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5557-release-notes))
</li><li>`13` - READ, INSERT, REPLACE and SHOW (from [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.1.27](/kb/en/mariadb-10127-release-notes/), [MariaDB Galera 10.0.32](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10032-release-notes), [MariaDB Galera 5.5.57](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5557-release-notes))
</li><li>`14` - UPDATE, DELETE, INSERT, REPLACE and SHOW (from [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.1.27](/kb/en/mariadb-10127-release-notes/), [MariaDB Galera 10.0.32](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10032-release-notes), [MariaDB Galera 5.5.57](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5557-release-notes))
</li><li>`15` - READ, UPDATE, DELETE, INSERT, REPLACE and SHOW (from [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.1.27](/kb/en/mariadb-10127-release-notes/), [MariaDB Galera 10.0.32](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10032-release-notes), [MariaDB Galera 5.5.57](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5557-release-notes))
</li></ul>
- <strong>Commandline:</strong> `--wsrep-sync-wait=`#
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> 
<ul start="1"><li>`0` to `15` (&gt;= [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/), [MariaDB 10.1.27](/kb/en/mariadb-10127-release-notes/), [MariaDB Galera 10.0.32](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10032-release-notes), [MariaDB Galera 5.5.57](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5557-release-notes))
</li><li>`0` to `7` (&lt;= [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/), [MariaDB 10.1.26](/kb/en/mariadb-10126-release-notes/), [MariaDB Galera 10.0.31](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10031-release-notes), [MariaDB Galera 5.5.56](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5556-release-notes))
</li></ul>
- <strong>Introduced:</strong> [MariaDB Galera 10.0.13](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10013-release-notes), [MariaDB Galera 5.5.39](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5539-release-notes)

---

#### `wsrep_trx_fragment_size`

- <strong>Description:</strong> Size of transaction fragments for streaming replication (measured in units as specified by [wsrep_trx_fragment_unit](#wsrep_trx_fragment_unit))
- <strong>Commandline:</strong> `--wsrep-trx-fragment-size=`#
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `2147483647`
- <strong>Introduced:</strong> [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/)

---

#### `wsrep_trx_fragment_unit`

- <strong>Description:</strong> Unit for streaming replication transaction fragments' size:
<ul start="1"><li>`bytes`: transaction’s binlog events buffer size in bytes
</li><li>`rows`: number of rows affected by the transaction
</li><li>`statements`: number of SQL statements executed in the multi-statement transaction
</li></ul>
- <strong>Commandline:</strong> `--wsrep-trx-fragment-unit=value`
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> enum
- <strong>Default Value:</strong> `bytes`
- <strong>Valid Values:</strong> `bytes`, `rows` or `statements`
- <strong>Introduced:</strong> [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/)

---