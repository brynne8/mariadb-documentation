# RESET SLAVE

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

## Syntax

```sql
RESET SLAVE ["connection_name"] [ALL]                
```

## Description

RESET SLAVE makes the slave forget its [replication](/replication/) position in the
master's [binary log](/mariadb-administration/server-monitoring-logs/binary-log/). This statement is meant to be used for a clean
start. It deletes the master.info and relay-log.info files, all the
[relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) files, and starts a new relay log file. To use RESET SLAVE,
the slave replication threads must be stopped (use [STOP SLAVE](/kb/en/stop-slave/) if
necessary).

Note: All relay log files are deleted, even if they have not been
completely executed by the slave SQL thread. (This is a condition
likely to exist on a replication slave if you have issued a STOP SLAVE
statement or if the slave is highly loaded.)

Connection information stored in the master.info file is immediately
reset using any values specified in the corresponding startup options.
This information includes values such as master host, master port,
master user, and master password. If the slave SQL thread was in the
middle of replicating temporary tables when it was stopped, and RESET
SLAVE is issued, these replicated temporary tables are deleted on the
slave.

The <code class="highlight fixed" style="white-space:pre-wrap">ALL</code> also resets the <code class="highlight fixed" style="white-space:pre-wrap">PORT</code>, <code class="highlight fixed" style="white-space:pre-wrap">HOST</code>, <code class="highlight fixed" style="white-space:pre-wrap">USER</code> and <code class="highlight fixed" style="white-space:pre-wrap">PASSWORD</code> parameters for the slave. If you are using a connection name, it will permanently delete it and it will not show up anymore in [SHOW ALL SLAVES STATUS](/kb/en/show-slave-status/).

#### connection_name

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `connection_name` option was added as part of [multi-source replication](/replication/standard-replication/multi-source-replication/) added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

If there is only one nameless master, or the default master (as specified by the [default_master_connection](/kb/en/replication-and-binary-log-server-system-variables/#default_master_connection) system variable) is intended, `connection_name` can be omitted. If provided, the `RESET SLAVE` statement will apply to the specified master. `connection_name` is case-insensitive.

#### RESET REPLICA

##### MariaDB starting with [10.5.1](/kb/en/mariadb-1051-release-notes/)

`RESET REPLICA` is an alias for `RESET SLAVE` from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/).

## See Also

- [STOP SLAVE](/kb/en/stop-slave/) stops the slave, but it can be restarted with  [START SLAVE](/kb/en/start-slave/) or after next MariaDB server restart.