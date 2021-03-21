# Binlog Event Checksum Interoperability

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

The introduction of [checksums on binlog events](/replication/standard-replication/binlog-event-checksums/) changes the format that events
are stored in [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) files and sent over the network to slaves. This raises the question on what happens when replicating between different versions of the
server, where one server is a newer version that has the binlog checksum
feature implemented, while the other server is an older version that does not
know about binlog checksums.

When checksums are disabled on the master (or the master has the old version
with no checksums implemented), there is no problem. In this case the binlog
format is backwards compatible, and replication works fine.

When the master is a newer version with checksums enabled in the binlog, but
the slave is an old version that does not understand checksums, replication
will fail. The master will disconnect the slave with an error, and also log a
warning in its own error log. This prevents sending events to the slave that it
will be unable to interpret correctly, but means that binlog checksums can not
be used with older slaves. (With the recommended upgrade path, where slaves are
upgraded before masters, this is not a problem of course).

Replicating from a new MySQL master with checksums enabled to a new MariaDB
which also understands checksums works, and the MariaDB slave will verify
checksums on replicated events.

There is however a problem when a newer MySQL slave replicates against a newer
MariaDB master with checksums enabled. The slave server looks at the master
server version to know whether events include checksums or not, and MySQL has
not yet been updated to learn that MariaDB does this already from version 5.3.0
(as of the time of writing, MySQL 5.6.2). Thus, if MariaDB at least version
5.3.0 but less that 5.6.1 is used as a master with binlog checksums enabled, a
MySQL slave will interpret the received events incorrectly as it does not
realise the last part of the events is the checksum. So replication will fail
with an error about corrupt events or even silent corruption of replicated data
in unlucky cases. This requires changes to the MySQL server to fix.

Here is a summary table of the status of replication between different
combination of master and slave servers and checksum enabled/disabled:

- <strong>OLD:</strong> MySQL &lt;5.6.1 or MariaDB &lt; 5.3.0 with no checksum capabilities
- <strong>NEW-MARIA:</strong> MariaDB &gt;= 5.3.0 with checksum capabilities
- <strong>NEW-MYSQL:</strong> MySQL &gt;= 5.6.1 with checksum capabilities

<table><tbody><tr><th>Master mysqlbinlog</th><th>Slave / enabled?</th><th>Checksums</th><th>Status</th></tr>
<tr><td>OLD</td><td>OLD</td><td>-</td><td>Ok</td></tr>
<tr><td>OLD</td><td>NEW-MARIA</td><td>-</td><td>Ok</td></tr>
<tr><td>OLD</td><td>MYSQL</td><td>-</td><td>Ok</td></tr>
<tr><td>NEW-MARIA</td><td>OLD</td><td>No</td><td>Ok</td></tr>
<tr><td>NEW-MARIA</td><td>OLD</td><td>Yes</td><td>Master will refuse with error</td></tr>
<tr><td>NEW-MARIA</td><td>NEW-MARIA</td><td>Yes/No</td><td>Ok</td></tr>
<tr><td>NEW-MARIA</td><td>NEW-MYSQL</td><td>No</td><td>Ok</td></tr>
<tr><td>NEW-MARIA</td><td>NEW-MYSQL</td><td>Yes</td><td>Fail. Requires changes in MySQL, otherwise it will not realise MariaDB &lt; 5.6.1 does checksums and will be confused.</td></tr>
<tr><td>NEW-MYSQL</td><td>OLD</td><td>No</td><td>Ok</td></tr>
<tr><td>NEW-MYSQL</td><td>OLD</td><td>Yes</td><td>Master will refuse with error</td></tr>
<tr><td>NEW-MYSQL</td><td>NEW-MARIA</td><td>Yes/No</td><td>Ok</td></tr>
<tr><td>NEW-MYSQL</td><td>NEW-MYSQL</td><td>Yes/No</td><td>Ok</td></tr>
</tbody></table>

## Checksums and `mysqlbinlog`

When using the [mysqlbinlog](/clients-utilities/mysqlbinlog/) client program, there are similar issues.

A version of `mysqlbinlog` which understands checksums can read binlog files
from either old or new servers, with or without checksums enabled.

An old version of `mysqlbinlog` can read binlog files produced by a new
server version <strong>if</strong> checksums were disabled when the log was produced. Old
versions of `mysqlbinlog` reading a new binlog file containing checksums will
be confused, and output will be garbled, with the added checksums being
interpreted as extra garbage at the end of query strings and similar entries. No
error will be reported in this case, just wrong output.

A version of `mysqlbinlog` from MySQL &gt;= 5.6.1 will have similar problems as
a slave until this is fixed in MySQL. When reading a binlog file with checksums
produced by MariaDB &gt;= 5.3.0 but &lt; 5.6.1, it will not realise that checksums
are included, and will produce garbled output just like an old version of
`mysqlbinlog`. The MariaDB version of `mysqlbinlog` can read binlog files
produced by either MySQL or MariaDB just fine.

## See Also

- [Binlog Event Checksums](/replication/standard-replication/binlog-event-checksums/)