# Galera Cluster Status Variables

## Viewing Galera Cluster Status variables

Galera Status variables can be viewed with the [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status) statement.

```sql
SHOW STATUS LIKE 'wsrep%';
```

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables).

## List of Galera Cluster Status variables

MariaDB Galera Cluster has the following status variables:

#### `wsrep_applier_thread_count`

- <strong>Description:</strong> Stores current number of applier threads to make clear how many slave threads of this type there are.
- <strong>Introduced:</strong> [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/)

---

#### `wsrep_apply_oooe`

- <strong>Description:</strong> How often writesets have been applied out of order, an indicators of parallelization efficiency.

---

#### `wsrep_apply_oool`

- <strong>Description:</strong> How often writesets with a higher sequence number were applied before ones with a lower sequence number, implying slow writesets.

---

#### `wsrep_apply_window`

- <strong>Description:</strong> Average distance between highest and lowest concurrently applied seqno.

---

#### `wsrep_cert_deps_distance`

- <strong>Description:</strong> Average distance between the highest and the lowest sequence numbers that can possibly be applied in parallel, or the potential degree of parallelization.

---

#### `wsrep_cert_index_size`

- <strong>Description:</strong> The number of entries in the certification index.

---

#### `wsrep_cert_interval`

- <strong>Description:</strong> Average number of transactions received while a transaction replicates.

---

#### `wsrep_cluster_capabilities`

- <strong>Description:</strong>

---

#### `wsrep_cluster_conf_id`

- <strong>Description:</strong> Total number of cluster membership changes that have taken place.

---

#### `wsrep_cluster_size`

- <strong>Description:</strong> Number of nodes currently in the cluster.

---

#### `wsrep_cluster_state_uuid`

- <strong>Description:</strong>  UUID state of the cluster. If it matches the value in [wsrep_local_state_uuid](#wsrep_local_state_uuid), the local and cluster nodes are in sync.

---

#### `wsrep_cluster_status`

- <strong>Description:</strong> Cluster component status. Possible values are `PRIMARY` (primary group configuration, quorum present), `NON_PRIMARY` (non-primary group configuration, quorum lost) or `DISCONNECTED` (not connected to group, retrying).

---

#### `wsrep_cluster_weight`

- <strong>Description:</strong> The total weight of the current members in the cluster. The value is counted as a sum of of pc.weight of the nodes in the current Primary Component.

---

#### `wsrep_commit_oooe`

- <strong>Description:</strong> How often a transaction was committed out of order.

---

#### `wsrep_commit_oool`

- <strong>Description:</strong> No meaning.

---

#### `wsrep_commit_window`

- <strong>Description:</strong> Average distance between highest and lowest concurrently committed seqno.

---

#### `wsrep_connected`

- <strong>Description:</strong> Whether or not MariaDB is connected to the wsrep provider. Possible values are `ON` or `OFF`.

---

#### `wsrep_desync_count`

- <strong>Description:</strong> Returns the number of operations in progress that require the node to temporarily desync from the cluster.

---

#### `wsrep_evs_delayed`

- <strong>Description:</strong> Provides a comma separated list of all the nodes this node has registered on its delayed list.

---

#### `wsrep_evs_evict_list`

- <strong>Description:</strong> Lists the UUID’s of all nodes evicted from the cluster. Evicted nodes cannot rejoin the cluster until you restart their mysqld processes.

---

#### `wsrep_evs_repl_latency`

- <strong>Description:</strong> This status variable provides figures for the replication latency on group communication. It measures latency (in seconds)  from the time point when a message is sent out to the time point when a message is received. As replication is a group operation, this essentially gives you the slowest ACK and longest RTT in the cluster.

---

#### `wsrep_evs_state`

- <strong>Description:</strong> Shows the internal state of the EVS Protocol.

---

#### `wsrep_flow_control_paused`

- <strong>Description:</strong> The fraction of time since the last FLUSH STATUS command that replication was paused due to flow control.

---

#### `wsrep_flow_control_paused_ns`

- <strong>Description:</strong> The total time spent in a paused state measured in nanoseconds.

---

#### `wsrep_flow_control_recv`

- <strong>Description:</strong> Number of FC_PAUSE events received as well as sent since the most recent status query.

---

#### `wsrep_flow_control_sent`

- <strong>Description:</strong> Number of FC_PAUSE events sent since the most recent status query

---

#### `wsrep_gcomm_uuid`

- <strong>Description:</strong> The UUID assigned to the node.

---

#### `wsrep_incoming_addresses`

- <strong>Description:</strong> Comma-separated list of incoming server addresses in the cluster component.

---

#### `wsrep_last_committed`

- <strong>Description:</strong> Sequence number of the most recently committed transaction.

---

#### `wsrep_local_bf_aborts`

- <strong>Description:</strong> Total number of local transactions aborted by slave transactions while being executed

---

#### `wsrep_local_cached_downto`

- <strong>Description:</strong> The lowest sequence number, or seqno, in the write-set cache (GCache).

---

#### `wsrep_local_cert_failures`

- <strong>Description:</strong> Total number of local transactions that failed the certification test.

---

#### `wsrep_local_commits`

- <strong>Description:</strong> Total number of local transactions committed on the node.

---

#### `wsrep_local_index`

- <strong>Description:</strong> The node's index in the cluster. The index is zero-based.

---

#### `wsrep_local_recv_queue`

- <strong>Description:</strong> Current length of the receive queue, which is the number of writesets waiting to be applied.

---

#### `wsrep_local_recv_queue_avg`

- <strong>Description:</strong> Average length of the receive queue since the most recent status query. If this value is noticeably larger than zero, the node is likely to be overloaded, and cannot apply the writesets as quickly as they arrive, resulting in replication throttling.

---

#### `wsrep_local_recv_queue_max`

- <strong>Description:</strong> The maximum length of the recv queue since the last FLUSH STATUS command.

---

#### `wsrep_local_recv_queue_min`

- <strong>Description:</strong> The minimum length of the recv queue since the last FLUSH STATUS command.

---

#### `wsrep_local_replays`

- <strong>Description:</strong> Total number of transaction replays due to asymmetric lock granularity.

---

#### `wsrep_local_send_queue`

- <strong>Description:</strong> Current length of the send queue, which is the number of writesets waiting to be sent.

---

#### `wsrep_local_send_queue_avg`

- <strong>Description:</strong> Average length of the send queue since the most recent status query. If this value is noticeably larger than zero, there is most likely network throughput or replication throttling issues.

---

#### `wsrep_local_send_queue_max`

- <strong>Description:</strong> The maximum length of the send queue since the last FLUSH STATUS command.

---

#### `wsrep_local_send_queue_min`

- <strong>Description:</strong> The minimum length of the send queue since the last FLUSH STATUS command.

---

#### `wsrep_local_state`

- <strong>Description:</strong> Internal Galera Cluster FSM state number.

---

#### `wsrep_local_state_comment`

- <strong>Description:</strong> Human-readable explanation of the state.

---

#### `wsrep_local_state_uuid`

- <strong>Description:</strong> The node's UUID state. If it matches the value in [wsrep_cluster_state_uuid](#wsrep_cluster_state_uuid), the local and cluster nodes are in sync.

---

#### `wsrep_open_connections`

- <strong>Description:</strong> The number of open connection objects inside the wsrep provider.

---

#### `wsrep_open_transactions`

- <strong>Description:</strong> The number of locally running transactions which have been registered inside the wsrep provider. This means transactions which have made operations which have caused write set population to happen. Transactions which are read only are not counted.

---

#### `wsrep_protocol_version`

- <strong>Description:</strong> The wsrep protocol version being used.

---

#### `wsrep_provider_name`

- <strong>Description:</strong> The name of the provider. The default is "Galera".

---

#### `wsrep_provider_vendor`

- <strong>Description:</strong> The vendor string.

---

#### `wsrep_provider_version`

- <strong>Description:</strong> The version number of the Galera wsrep provider

---

#### `wsrep_ready`

- <strong>Description:</strong> Whether or not the Galera wsrep provider is ready. Possible values are `ON` or `OFF`

---

#### `wsrep_received`

- <strong>Description:</strong> Total number of writesets received from other nodes.

---

#### `wsrep_received_bytes`

- <strong>Description:</strong> Total size in bytes of all writesets received from other nodes.

---

#### `wsrep_repl_data_bytes`

- <strong>Description:</strong>  Total size of data replicated.

---

#### `wsrep_repl_keys`

- <strong>Description:</strong>  Total number of keys replicated.

---

#### `wsrep_repl_keys_bytes`

- <strong>Description:</strong>  Total size of keys replicated.

---

#### `wsrep_repl_other_bytes`

- <strong>Description:</strong>  Total size of other bits replicated.

---

#### `wsrep_replicated`

- <strong>Description:</strong>  Total number of writesets replicated to other nodes.

---

#### `wsrep_replicated_bytes`

- <strong>Description:</strong>  Total size in bytes of all writesets replicated to other nodes.

---

#### `wsrep_rollbacker_thread_count`

- <strong>Description:</strong> Stores current number of rollbacker threads to make clear how many slave threads of this type there are.
- <strong>Introduced:</strong> [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/)

---

#### `wsrep_thread_count`

- <strong>Description:</strong>  Total number of wsrep (applier/rollbacker) threads.
- <strong>Introduced:</strong> `MariaDB Galera Cluster 5.5.38` `MariaDB Galera Cluster 10.0.11`

---