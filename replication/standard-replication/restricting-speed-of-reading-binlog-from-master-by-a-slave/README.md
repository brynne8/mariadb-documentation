# Restricting speed of reading binlog from master by a slave

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

## Restricting speed of reading binlog from master by a slave

When a slave starts after being stopped for some time, or a new slave starts
that was created from a backup from some time back, a lot of old binlog events
may need to be downloaded from the master. If this happens from many slaves
simultaneously, it can put a lot of load on the master.

The option <strong>read_binlog_speed_limit</strong> can be used to reduce such load, by
limiting the speed at which events are downloaded. The limit is given as
maximum kilobytes per second to download on one slave connection.

With this option set, the replication I/O thread will limit the rate of
download. Since the I/O thread is often much faster to download events than
the SQL thread is at applying them, an appropriate value for
<strong>read_binlog_speed_limit</strong> may reduce load spikes on the master without
much limit in the speed of the replication slave.

The option <strong>read_binlog_speed_limit</strong> is available starting from [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

#### `read_binlog_speed_limit`

- <strong>Description:</strong> Maximum speed(KB/s) to read binlog from master
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--read-binlog-speed-limit[=#]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)

---