# Binlog Event Checksums

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

MariaDB includes a feature to include a checksum in [binary log](/mariadb-administration/server-monitoring-logs/binary-log) events.

Checksums are enabled with the [binlog_checksum option](/kb/en/replication-and-binary-log-server-system-variables/#binlog_checksum). Until [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/), this was disabled by default. From [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/), the option is set to `CRC32`.

The variable can be changed dynamically without restarting the server. Setting
the variable in any way (even to the existing value) forces a rotation of the
[binary log](/mariadb-administration/server-monitoring-logs/binary-log) (the intention is to avoid having a single binlog where some events
are checksummed and others are not).

When checksums are enabled, replication slaves will check events received over
the network for checksum errors, and will stop with an error if a corrupt event
is detected.

In addition, the server can be configured to verify checksums in two other
places.

One is when reading events from the binlog on the master, for example when
sending events to a slave or for something like SHOW BINLOG EVENTS. This is
controlled by option master_verify_checksum, and is thus used to detect file
system corruption of the binlog files.

The other is when the slave SQL thread reads events from the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log). This is
controlled by the slave_sql_verify_checksum option, and is used to detect file
system corruption of slave relay log files.

`master_verify_checksum`

- <strong>Description:</strong> Verify binlog checksums when reading events from the binlog on the master.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master_verify_checksum=[0|1]</code>
- <strong>Scope:</strong> Global
- <strong>Access Type:</strong> Can be changed dynamically
- <strong>Data Type:</strong> `bool`
- <strong>Default Value:</strong> `OFF (0)`

`slave_sql_verify_checksum`

- <strong>Description:</strong> Verify binlog checksums when the slave SQL thread reads events from the relay log.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--slave_sql_verify_checksum=[0|1]</code>
- <strong>Scope:</strong> Global
- <strong>Access Type:</strong> Can be changed dynamically
- <strong>Data Type:</strong> `bool`
- <strong>Default Value:</strong> `ON (1)`

The [mysqlbinlog](/clients-utilities/mysqlbinlog) client program by default does not verify checksums when
reading a binlog file, however it can be instructed to do so with the option
verify-binlog-checksum:

- <strong>Variable Name:</strong> `verify-binlog-checksum`
- <strong>Data Type:</strong> `bool`
- <strong>Default Value:</strong> `OFF`

## See Also

- [Binlog Event Checksum Interoperability](/replication/standard-replication/binlog-event-checksum-interoperability)
- [What is MariaDB 5.3](/kb/en/what-is-mariadb-53/)