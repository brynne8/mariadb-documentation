# wsrep_provider_options

## wsrep_provider_options

The following options can be set as part of the Galera [wsrep_provider_options](/kb/en/galera-cluster-system-variables/#wsrep_provider_options) variable. Dynamic options can be changed while the server is running.

Options need to be provided as a semicolon (;) separated list on a single line.

Note that before Galera 3, the `repl` tag was named `replicator`.

#### `base_dir`

- <strong>Description:</strong> Specifies the data directory

---

#### `base_host`

- <strong>Description:</strong> For internal use. Should not be manually set.
- <strong>Default:</strong> `127.0.0.1` (detected network address)

---

#### `base_port`

- <strong>Description:</strong> For internal use. Should not be manually set.
- <strong>Default:</strong> `4567`

---

#### `cert.log_conflicts`

- <strong>Description:</strong> Certification failure log details.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `no`

---

#### `cert.optimistic_pa`

- <strong>Description:</strong> Controls parallel application of actions on the replica. If set, the full range of parallelization as determined by the certification algorithm is permitted. If not set, the parallel applying window will not exceed that seen on the primary, and applying will start no sooner than after all actions it has seen on the master are committed.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `yes`
- <strong>Introduced:</strong> [MariaDB 10.3.12](/kb/en/mariadb-10312-release-notes/), [MariaDB 10.2.20](/kb/en/mariadb-10220-release-notes/), [MariaDB 10.1.38](/kb/en/mariadb-10138-release-notes/)

---

#### `debug`

- <strong>Description:</strong> Enable debugging.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `no`

---

#### `evs.auto_evict`

- <strong>Description:</strong> Number of entries the node permits for a given delayed node before triggering the Auto Eviction protocol. An entry is added to a delayed list for each delayed response from a node. If set to `0`, the default, the Auto Eviction protocol is disabled for this node. See [Auto Eviction](https://galeracluster.com/library/documentation/auto-eviction.html) for more.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `0`

---

#### `evs.causal_keepalive_period`

- <strong>Description:</strong> Used by the developers only, and not manually serviceable.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> The [evs.keepalive_period](#evskeepalive_period).

---

#### `evs.debug_log_mask`

- <strong>Description:</strong> Controls EVS debug logging. Only effective when [wsrep_debug](/kb/en/galera-cluster-system-variables/#wsrep_debug) is on.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `0x1`

---

#### `evs.delay_margin`

- <strong>Description:</strong> Time that response times can be delayed before this node adds an entry to the delayed list. See [evs.auto_evict](#evsauto_evict). Must be set to a higher value than the round-trip delay time between nodes.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `PT1S`

---

#### `evs.delayed_keep_period`

- <strong>Description:</strong> Time that this node requires a previously delayed node to remain responsive before being removed from the delayed list. See [evs.auto_evict](#evsauto_evict).
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `PT30S`

---

#### `evs.evict`

- <strong>Description:</strong> When set to the gcomm UUID of a node, that node is evicted from the cluster. When set to an empty string, the eviction list is cleared on the node where it is set. See [evs.auto_evict](#evsauto_evict).
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> Empty string

---

#### `evs.inactive_check_period`

- <strong>Description:</strong> Frequency of checks for peer inactivity (looking for nodes with delayed responses), after which nodes may be added to the delayed list, and later evicted.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> ` PT0.5S `

---

#### `evs.inactive_timeout`

- <strong>Description:</strong> Time limit that a node can be inactive before being pronounced as dead.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `PT15S`

---

#### `evs.info_log_mask `

- <strong>Description:</strong> Controls extra EVS info logging. Bits:
<ul start="1"><li>0x1 – extra view change information
</li><li>0x2 – extra state change information
</li><li>0x4 – statistics
</li><li>0x8 – profiling (only available in builds with profiling enabled) 
</li></ul>
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `0`

---

#### `evs.install_timeout`

- <strong>Description:</strong> Timeout on waits for install message acknowledgments. Replaces evs.consensus_timeout.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `PT7.5S`

---

#### `evs.join_retrans_period`

- <strong>Description:</strong> Time period for how often retransmission of EVS join messages when forming cluster membership should occur.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `PT1S`

---

#### `evs.keepalive_period`

- <strong>Description:</strong> How often keepalive signals should be transmitted when there's no other traffic.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `PT1S`

---

#### `evs.max_install_timeouts`

- <strong>Description:</strong> Number of membership install rounds to attempt before timing out. The total rounds will be this value plus two.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `3`

---

#### `evs.send_window`

- <strong>Description:</strong> Maximum number of packets that can be replicated at a time, Must be more than [evs.user_send_window](#evsuser_send_window), which applies to data packets only (double is recommended). In WAN environments can be set much higher than the default, for example `512`.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `4`

---

#### `evs.stats_report_period`

- <strong>Description:</strong> Reporting period for EVS statistics.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `PT1M`

---

#### `evs.suspect_timeout`

- <strong>Description:</strong> A node will be suspected to be dead after this period of inactivity. If all nodes agree, the node is dropped from the cluster before [evs.inactive_timeout](#evsinactive_timeout) is reached.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `PT5S`

---

#### `evs.use_aggregate`

- <strong>Description:</strong> If set to `true` (the default), small packets will be aggregated into one where possible.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `true`

---

#### `evs.user_send_window`

- <strong>Description:</strong> Maximum number of data packets that can be replicated at a time. Must be smaller than [evs.send_window](#evssend_window) (half is recommended). In WAN environments can be set much higher than the default, for example `512`.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `2`

---

#### `evs.version`

- <strong>Description:</strong> EVS protocol version. Defaults to `0` for backward compatibility. Certain EVS features (e.g. auto eviction) require more recent versions.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `0`

---

#### `evs.view_forget_timeout`

- <strong>Description:</strong> Time after which past views will be dropped from the view history.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `P1D`

---

#### `gcache.dir`

- <strong>Description:</strong>  Directory where GCache files are placed.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> The working directory

---

#### `gcache.keep_pages_size`

- <strong>Description:</strong> Total size of the page storage pages for caching. One page is always present if only page storage is enabled.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `0`

---

#### `gcache.mem_size`

- <strong>Description:</strong> Maximum size of size of the malloc() store for setups that have spare RAM.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `0`

---

#### `gcache.name`

- <strong>Description:</strong> Gcache ring buffer storage file name. By default placed in the working directory, changing to another location or partition can reduce disk IO.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `./galera.cache`
---

#### `gcache.page_size`

- <strong>Description:</strong> Size of the page storage page files. These are prefixed by `gcache.page`. Can be set to as large as the disk can handle.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `128M`

---

#### `gcache.recover`

- <strong>Description:</strong> Whether or not gcache recovery takes place when the node starts up. If it is possible to recover gcache, the node can then provide IST to other joining nodes, which assists when the whole cluster is restarted.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `no`
- <strong>Introduced:</strong> [MariaDB 10.1.20](/kb/en/mariadb-10120-release-notes/), [MariaDB Galera 10.0.29](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10029-release-notes), [MariaDB Galera 5.5.54](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5554-release-notes)

---

#### `gcache.size`

- <strong>Description:</strong> Gcache ring buffer storage size (the space the node uses for caching write sets), preallocated on startup.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `128M`

---

#### `gcomm.thread_prio`

- <strong>Description:</strong> Gcomm thread policy and priority (in the format `policy:priority`. Priority is an integer, while policy can be one of:
<ul><li>`fifo`: First-in, first-out scheduling. Always preempt other, batch or idle threads and can only be preempted by other `fifo` threads of a higher priority or blocked by an I/O request.
</li><li>`rr`: Round-robin scheduling. Always preempt other, batch or idle threads. Runs for a fixed period of time after which the thread is stopped and moved to the end of the list, being replaced by another round-robin thread with the same priority. Otherwise runs until preempted by other `rr` threads of a higher priority or blocked by an I/O request.
</li><li>`other`: Default scheduling on Linux. Threads run until preempted by a thread of a higher priority or a superior scheduling designation, or blocked by an I/O request. 
</li></ul>
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> Empty string

---

#### `gcs.fc_debug`

- <strong>Description:</strong> If set to a value greater than zero (the default), debug statistics about SST flow control will be posted each time after the specified number of writesets.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `0`

---

#### `gcs.fc_factor`

- <strong>Description:</strong>Fraction below [gcs.fc_limit](#gcsfc_limit) which if the recv queue drops below, replication resumes.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `1.0`

---

#### `gcs.fc_limit`

- <strong>Description:</strong> If the recv queue exceeds this many writesets, replication is paused.  Can increase greatly in master-slave setups. Replication will resume again according to the [gcs.fc_factor](#gcsfc_factor) setting.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `16`

---

#### `gcs.fc_master_slave`

- <strong>Description:</strong> Whether to assume that the cluster only contains one master.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `no`

---

#### `gcs.max_packet_size`

- <strong>Description:</strong> Maximum packet size, after which writesets become fragmented.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `64500`

---

#### `gcs.max_throttle`

- <strong>Description:</strong>  How much we can throttle replication rate during state transfer (to avoid running out of memory). Set it to 0.0 if stopping replication is acceptable for the sake of completing state transfer.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `0.25`

---

#### `gcs.recv_q_hard_limit`

- <strong>Description:</strong> Maximum size of the recv queue. If exceeded, the server aborts. Half of available RAM plus swap is a recommended size.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> LLONG_MAX

---

#### `gcs.recv_q_soft_limit`

- <strong>Description:</strong> Fraction of [gcs.recv_q_hard_limit](#gcsrecv_q_hard_limit) after which replication rate is throttled. The rate of throttling increases linearly from zero (the regular, varying rate of replication) at and below `csrecv_q_soft_limit` to one (full throttling) at [gcs.recv_q_hard_limit](#gcsrecv_q_hard_limit)
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `0.25`

---

#### `gcs.sync_donor`

- <strong>Description:</strong>  Whether or not the rest of the cluster should stay in sync with the donor. If set to `YES` (`NO` is default), if the donor is blocked by state transfer, the whole cluster is also blocked.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `no`

---

#### `gmcast.listen_addr`

- <strong>Description:</strong> Address Galera listens for connections from other nodes. Can be used to override the default port to listen, which is obtained from the connection address.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `tcp:<em>0.0.0.0:4567</em>`

---

#### `gmcast.mcast_addr`

- <strong>Description:</strong> Not set by default, but if set, UDP multicast will be used for replication. Must be identical on all nodes.For example, `gmcast.mcast_addr=239.192.0.11`
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> None

---

#### `gmcast.mcast_ttl`

- <strong>Description:</strong> Multicast packet TTL (time to live) value.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `1`

---

#### `gmcast.peer_timeout`

- <strong>Description:</strong> Connection timeout for initiating message relaying.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `PT3S`

---

#### `gmcast.segment`

- <strong>Description:</strong> Defines the segment to which the node belongs. By default, all nodes are placed in the same segment (`0`). Usually, you would place all nodes in the same datacenter in the same segment. Galera protocol traffic is only redirected to one node in each segment, and then relayed to other nodes in that same segment, which saves cross-datacenter network traffic at the expense of some extra latency. State transfers are also, preferably but not exclusively, taken from the same segment. If there are no nodes available in the same segment, state transfer will be taken from a node in another segment.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `0`
- <strong>Range:</strong> `0` to `255`

---

#### `gmcast.time_wait`

- <strong>Description:</strong> Waiting time before allowing a peer that was declared outside of the stable view to reconnect.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `PT5S`

---

#### `gmcast.version`

- <strong>Description:</strong> Deprecated option. Gmcast version.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `0`

---

#### `ist.recv_addr`

- <strong>Description:</strong> Address for listening for Incremental State Transfer.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> &lt;address&gt;:&lt;port+1&gt; from [wsrep_node_address](/kb/en/galera-cluster-system-variables/#wsrep_node_address)

---

#### `ist.recv_bind`

- <strong>Description:</strong>
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> Empty string
- <strong>Introduced:</strong> [MariaDB 10.1.17](/kb/en/mariadb-10117-release-notes/), [MariaDB Galera 10.0.27](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10027-release-notes), [MariaDB Galera 5.5.51](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5551-release-notes)

---

#### `pc.announce_timeout`

- <strong>Description:</strong> Period of time for which cluster joining announcements are sent every 1/2 second.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `PT3S`

---

#### `pc.checksum`

- <strong>Description:</strong> For debug purposes, by default `false` (`true` in earlier releases), indicates whether to checksum replicated messages on PC level. Safe to turn off.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `false`

---

#### `pc.ignore_quorum`

- <strong>Description:</strong> Whether to ignore quorum calculations, for example when a master splits from several slaves, it will remain in operation if set to `true` (`false is default`). Use with care however, as in master-slave setups, slaves will not automatically reconnect to the master if set.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `false`

---

#### `pc.ignore_sb`

- <strong>Description:</strong> Whether to permit updates to be processed even in the case of split brain (when a node is disconnected from its remaining peers). Safe in master-slave setups, but could lead to data inconsistency in a multi-master setup.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `false`

---

#### `pc.linger`

- <strong>Description:</strong> Time that the PC protocol waits for EVS termination.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `PT20S`

---

#### `pc.npvo`

- <strong>Description:</strong> If set to `true` (`false` is default), when there are primary component conficts, the most recent component will override the older.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `false`

---

#### `pc.recovery`

- <strong>Description:</strong> If set to `true` (the default), the Primary Component state is stored on disk and in the case of a full cluster crash (e.g power outages), automatic recovery is then possible. Subsequent graceful full cluster restarts will require explicit bootstrapping for a new Primary Component.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `true`

---

#### `pc.version`

- <strong>Description:</strong> Deprecated option. PC protocol version.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `0`

---

#### `pc.wait_prim`

- <strong>Description:</strong> When set to `true`, the default, the node will wait for a primary component for the period of time specified by [pc.wait_prim_timeout](#pc.wait_prim_timeout). Used to bring up non-primary components and make them primary using [pc.bootstrap](#pcbootstrap.).
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `true`

---

#### `pc.wait_prim_timeout`

- <strong>Description:</strong> Ttime to wait for a primary component. See [pc.wait_prim](#pcwait_prim).
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `PT30S`

---

#### `pc.weight`

- <strong>Description:</strong> Node weight, used for quorum calculation. See the Codership article [Weighted Quorum](http://www.codership.com/wiki/doku.php?id=weighted_quorum).
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `1`

---

#### `protonet.backend `

- <strong>Description:</strong> Transport backend to use. Only ASIO is supported currently.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `asio`

---

#### `protonet.version`

- <strong>Description:</strong> Deprecated option. Protonet version.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `0`

---

#### `repl.causal_read_timeout`

- <strong>Description:</strong> Timeout period for causal reads.
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `PT30S`

---

#### `repl.commit_order`

- <strong>Description:</strong> Whether or not out-of-order committing is permitted, and under what conditions. By default it is not permitted, but setting this can improve parallel performance.
<ul start="1"><li>`0` BYPASS: No commit order monitoring is done (useful for measuring the performance penalty).
</li><li>`1` OOOC: Out-of-order committing is permitted for all transactions.
</li><li>`2` LOCAL_OOOC: Out-of-order committing is permitted for local transactions only.
</li><li>`3` NO_OOOC: Out-of-order committing is not permitted at all.
</li></ul>
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `3`

---

#### `repl.key_format`

- <strong>Description:</strong> Format for key replication. Can be one of:
<ul start="1"><li>`FLAT8` - shorter key with a higher probability of false positives when matching
</li><li>`FLAT16` - longer key with a lower probability of false positives when matching
</li><li>`FLAT8A` - shorter key with a higher probability of false positives when matching, includes annotations for debug purposes
</li><li>`FLAT16A` - longer key with a lower probability of false positives when matching, includes annotations for debug purposes
</li></ul>
- <strong>Dynamic:</strong> Yes
- <strong>Default:</strong> `FLAT8`

---

#### `repl.max_ws_size`

- <strong>Description:</strong>
- <strong>Dynamic:</strong>
- <strong>Default:</strong> `2147483647`

---

#### `repl.proto_max`

- <strong>Description:</strong>
- <strong>Dynamic:</strong>
- <strong>Default:</strong> `9`

---

#### `socket.checksum`

- <strong>Description:</strong> Method used for generating checksum. Note: If Galera 25.2.x and 25.3.x are both being used in the cluster, MariaDB with Galera 25.3.x must be started with `wsrep_provider_options='socket.checksum=1'` in order to make it backward compatible with Galera v2. Galera wsrep providers other than 25.3.x or 25.2.x are not supported.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `2`

---

#### `socket.recv_buf_size`

- <strong>Description:</strong> Size in bytes of the receive buffer used on the network sockets between nodes, passed on to the kernel via the SO_RCVBUF socket option.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> 
<ul start="1"><li>&gt;= [MariaDB 10.3.23](/kb/en/mariadb-10323-release-notes/), [MariaDB 10.2.32](/kb/en/mariadb-10232-release-notes/), [MariaDB 10.1.45](/kb/en/mariadb-10145-release-notes/): Auto
</li><li>&lt; [MariaDB 10.3.22](/kb/en/mariadb-10322-release-notes/): [MariaDB 10.2.31](/kb/en/mariadb-10231-release-notes/), [MariaDB 10.1.44](/kb/en/mariadb-10144-release-notes/): `212992`
</li></ul>

---

#### `socket.send_buf_size`

- <strong>Description:</strong> Size in bytes of the send buffer used on the network sockets between nodes, passed on to the kernel via the SO_SNDBUF socket option.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong>: Auto
- <strong>Introduced:</strong> [MariaDB 10.3.23](/kb/en/mariadb-10323-release-notes/), [MariaDB 10.2.32](/kb/en/mariadb-10232-release-notes/), [MariaDB 10.1.45](/kb/en/mariadb-10145-release-notes/)

---

#### `socket.ssl`

- <strong>Description:</strong> Explicitly enables TLS usage by the wsrep Provider.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> `NO`

---

#### `socket.ssl_ca`

- <strong>Description:</strong> Path to Certificate Authority (CA) file.  Implicitly enables the <a undefined>socket.ssl</a> option.
- <strong>Dynamic:</strong> No

---

#### `socket.ssl_cert`

- <strong>Description:</strong> Path to TLS certificate. Implicitly enables the <a undefined>socket.ssl</a> option.
- <strong>Dynamic:</strong> No

---

#### `socket.ssl_cipher`

- <strong>Description:</strong> TLS cipher to use. Implicitly enables the <a undefined>socket.ssl</a> option.  Since [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/) defaults to the value of the <a undefined>ssl_cipher</a> system variable.
- <strong>Dynamic:</strong> No
- <strong>Default:</strong> system default, before [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/) defaults to `AES128-SHA`.

---

#### `socket.ssl_compression`

- <strong>Description:</strong> Compression to use on TLS connections. Implicitly enables the <a undefined>socket.ssl</a> option.
- <strong>Dynamic:</strong> No

---

#### `socket.ssl_key`

- <strong>Description:</strong> Path to TLS key file. Implicitly enables the <a undefined>socket.ssl</a> option.
- <strong>Dynamic:</strong> No

---

#### `socket.ssl_password_file`

- <strong>Description:</strong> Path to password file to use in TLS connections. Implicitly enables the <a undefined>socket.ssl</a> option.
- <strong>Dynamic:</strong> No

---

## See Also

- [Galera parameters documentation from Codership](https://galeracluster.com/library/documentation/galera-parameters.html)