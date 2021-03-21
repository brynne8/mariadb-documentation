# Compressing Events to Reduce Size of the Binary Log

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

Starting from [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/), selected events in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) can be
optionally compressed, to save space in the binary log on disk and in
network transfers.

The events that can be compressed are the events that normally can be of a
significant size: Query events (for DDL and DML in [statement-based](/kb/en/binary-log-formats/#statement-based)
[replication](/replication/standard-replication)), and row events (for DML in [row-based](/kb/en/binary-log-formats/#row-based) [replication](/replication/standard-replication)).

Compression is fully transparent. Events are compressed on the primary before
being written into the binary log, and are uncompressed by the I/O thread on
the replica before being written into the relay log. The [mysqlbinlog](/clients-utilities/mysqlbinlog)
command will likewise uncompress events for its output.

Currently, the zlib compression algorithm is used to compress events.

Compression will have the most impact when events are of a non-negligible size, as each event is compressed individually. For example, batch INSERT statements that insert many rows or large values, or row-based events that touch a number of rows in one query.

The [log_bin_compress](/kb/en/replication-and-binary-log-server-system-variables/#log_bin_compress) option is used to enable compression of events. Only events with data (query text or row data) above a certain size are
compressed; the limit is set with the [log_bin_compress_min_len](/kb/en/replication-and-binary-log-server-system-variables/#log_bin_compress_min_len) option.