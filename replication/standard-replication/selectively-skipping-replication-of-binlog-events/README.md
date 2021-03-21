# Selectively Skipping Replication of Binlog Events

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

Normally, all changes that are logged as events in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) are also
replicated to all slaves (though still subject to filtering by
[replicate-do-db](/kb/en/replication-and-binary-log-system-variables/#replicate_do_db), [replicate-ignore-db](/kb/en/replication-and-binary-log-system-variables/#replicate_ignore_db),
and similar options). However, sometimes it may be desirable to have certain
events be logged into the binlog, but not be replicated to all or a subset of
slaves, where the distinction between events that should be replicated or not
is under the control of the application making the changes.

This could be useful if an application does some replication external to the
server outside of the built-in replication, or if it has some data that should
not be replicated for whatever reason.

This is possible with the following [system variables](/replication/optimization-and-tuning/system-variables/server-system-variables/).

## Master Session Variable: skip_replication

When the [skip_replication](/kb/en/replication-and-binary-log-server-system-variables/#skip_replication) variable is set to true, changes are logged into the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) with the flag `@@skip_replication` set. Such events will not be replicated by slaves that run with
<code class="fixed" style="white-space:pre-wrap">--replicate-events-marked-for-skip</code> set different from its default of `REPLICATE`.

<table><tbody><tr><th>Variable Name</th><td><code>skip_replication</code></td></tr>
<tr><th>Scope</th><td>Session only</td></tr>
<tr><th>Access Type</th><td>Dynamic</td></tr>
<tr><th>Data Type</th><td><code>bool</code></td></tr>
<tr><th>Default Value</th><td><code>OFF</code></td></tr>
</tbody></table>

The `skip_replication` option only has effect if [binary logging](/mariadb-administration/server-monitoring-logs/binary-log/) is enabled
and [sql_log_bin](/kb/en/replication-and-binary-log-server-system-variables/#skip_replication) is true.

Attempting to change `@@skip_replication` in the middle of a transaction will
fail; this is to avoid getting half of a transaction replicated while the other
half is not replicated. Be sure to end any current transaction with
`COMMIT`/`ROLLBACK` before changing the variable.

## Slave Option: --replicate-events-marked-for-skip

The [replicate_events_marked_for_skip](/kb/en/replication-and-binary-log-server-system-variables/#replicate_events_marked_for_skip) option tells the slave whether to replicate events that are marked with
the `@@skip_replication` flag. Default is `REPLICATE`, to ensure that all
changes are replicated to the slave. If set to `FILTER_ON_SLAVE`, events so
marked will be skipped on the slave and not replicated. If set to
`FILTER_ON_MASTER`, the filtering will be done on the master, saving on
network bandwidth as the events will not be received by the slave at all.

<table><tbody><tr><th>Variable Name</th><td><code>replicate_events_marked_for_skip</code></td></tr>
<tr><th>Scope</th><td>Global</td></tr>
<tr><th>Access Type</th><td>Dynamic</td></tr>
<tr><th>Data Type</th><td>enum: <code>REPLICATE</code> <code>|</code> <code>FILTER_ON_SLAVE</code> <code>|</code> <code>FILTER_ON_MASTER</code></td></tr>
<tr><th>Default Value</th><td><code>REPLICATE</code></td></tr>
</tbody></table>

<strong>Note:</strong> `replicate_events_marked_for_skip` is a dynamic variable (it can be
changed without restarting the server), however the slave threads must be
stopped when it is changed, otherwise an error will be thrown.

When events are filtered due to `@@skip_replication`, the filtering happens
on the master side; in other words, the event is never sent to the slave. If
many events are filtered like this, a slave can sit a long time without
receiving any events from the master. This is not a problem in itself, but must
be kept in mind when inquiring on the slave about events that are filtered. For
example `START SLAVE UNTIL &lt;some position&gt;` will stop when the first event
that is <strong>not</strong> filtered is encountered at the given position or beyond. If the
event at the given position is filtered, then the slave thread will only stop
when the next non-filtered event is encountered. In effect, if an event is
filtered, to the slave it appears that it was never written to the binlog on
the master.

Note that when events are filtered for a slave, the data in the database will
be different on the slave and on the master. It is the responsibility of the
application to replicate the data outside of the built-in replication or
otherwise ensure consistency of operation. If this is not done, it is possible
for replication to encounter, for example,
<a undefined>UNIQUE</a> contraint violations or
other problems which will cause replication to stop and require manual
intervention to fix.

The session variable `@@skip_replication` can be changed without requiring
special privileges. This makes it possible for normal applications to control
it without requiring `SUPER` privileges. But it must be kept in mind when using
slaves with <code class="fixed" style="white-space:pre-wrap">--replicate-events-marked-for-skip</code> set different
from `REPLICATE`, as it allows any connection to do changes that are not
replicated.

## skip_replication and sql_log_bin

[@@sql_log_bin](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set-sql_log_bin/) and `@@skip_replication` are somewhat
related, as they can both be used to prevent a change on the master from being
replicated to the slave. The difference is that with `@@skip_replication`,
changes are still written into the binlog, and replication of the events is
only skipped on slaves that explicitly are configured to do so, with
<code class="fixed" style="white-space:pre-wrap">--replicate-events-marked-for-skip</code> different from
`REPLICATE`. With `@@sql_log_bin`, events are not logged into the binlog,
and so are not replicated by any slave.

## skip_replication and the Binlog

When events in the binlog are marked with the `@@skip_replication` flag, the
flag will be preserved if the events are dumped by the [mysqlbinlog](/clients-utilities/mysqlbinlog/)
program and re-applied against a server with the
[mysql client](/clients-utilities/mysql-client/mysql-command-line-client/) program. Similarly, the
[BINLOG](/sql-statements-structure/sql-statements/administrative-sql-statements/binlog/) statement will preserve the flag from the
event being replayed. And a slave which runs with
<code class="fixed" style="white-space:pre-wrap">--log-slave-updates</code> and does not filter events
(<code class="fixed" style="white-space:pre-wrap">--replicate-events-marked-for-skip=REPLICATE</code>) will also
preserve the flag in the events logged into the binlog on the slave.

## See Also

- [Using SQL_SLAVE_SKIP_COUNTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/set-global-sql_slave_skip_counter/) - How to skip a number of events on the slave