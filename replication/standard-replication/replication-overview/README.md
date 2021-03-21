# Replication Overview

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

Replication is a feature allowing the contents of one or more servers (called masters) to be mirrored on one or more servers (called slaves).

You can exert control over which data to replicate. All databases, one or more databases, or tables within a database can each be selectively replicated.

The main mechanism used in replication is the [binary log](/mariadb-administration/server-monitoring-logs/binary-log). If binary logging is enabled, all updates to the database (data manipulation and data definition) are written into the binary log as binlog events. Slaves read the binary log from each master in order to access the data to replicate. A [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log) is created on the slave server, using the same format as the binary log, and this is used to perform the replication. Old relay log files are removed when no longer needed.

A slave server keeps track of the position in the master's binlog of the last event applied on the slave. This allows the slave server to re-connect and resume from where it left off after replication has been temporarily stopped. It also allows a slave to disconnect, be cloned and then have the new slave resume replication from the same master.

Masters and slaves do not need to be in constant communication with each other. It's quite possible to take servers offline or disconnect from the network, and when they come back, replication will continue where it left off.

## Replication Uses

Replication is used in a number of common scenarios. Uses include:

- Scalability. By having one or more slave servers, reads can be spread over multiple servers, reducing the load on the master. The most common scenario for a high-read, low-write environment is to have one master, where all the writes occur, replicating to multiple slaves, which handle most of the reads.
- Data analysis. Analyzing data may have too much of an impact on a master server, and this can similarly be handled on a slave server, while the master continues unaffected by the extra load.
- Backup assistance. [Backups](/kb/en/backing-up-and-restoring/) can more easily be run if a server is not actively changing the data. A common scenario is to replicate the data to slave, which is then disconnected from the master with the data in a stable state. Backup is then performed from this server. See [Replication as a Backup Solution](/mariadb-administration/backing-up-and-restoring-databases/replication-as-a-backup-solution).
- Distribution of data. Instead of being connected to a remote master, it's possible to replicate the data locally and work from this data instead.

## Common Replication Setups

### Standard Replication

<img src="/kb/en/replication-overview/+image/standard_replication" alt="standard_replication" title="standard_replication">

- Provides infinite read scale out.
- Provides high-availability by upgrading slave to master.

### Ring Replication

<img src="/kb/en/replication-overview/+image/ring_replication" alt="ring_replication" title="ring_replication">

- Provides read and write scaling.
- Doesn’t handle conflicts.
- If one master fails, replication stops.

### Star Replication

<img src="/kb/en/replication-overview/+image/star_replication" alt="star_replication" title="star_replication">

- Provides read and write scaling.
- Doesn’t handle conflicts.
- Have to use replication filters to avoid duplication of data.

### Multi-Source Replication

<img src="/kb/en/replication-overview/+image/multi_source_replication" alt="multi_source_replication" title="multi_source_replication">

- Allows you to combine data from different sources.
- Different domains executed independently in parallel on all slaves.

## See Also

- [Setting Up Replication](/replication/standard-replication/setting-up-replication)
- [Replication Compatibility](/kb/en/mariadb-vs-mysql-compatibility/#replication-compatibility)