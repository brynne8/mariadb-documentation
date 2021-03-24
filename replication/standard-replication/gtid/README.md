# Global Transaction ID

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

##### MariaDB starting with [10.0.2](/kb/en/mariadb-1002-release-notes/)

MariaDB has supported global transaction IDs (GTIDs) for replication since version 10.0.2.

Note that MariaDB and MySQL have different GTID implementations, and that these are not compatible with each other.

## Overview

MariaDB replication in general works as follows (see
[Replication overview](/replication/standard-replication/replication-overview/) for more information):

On a master server, all updates to the database (DML and DDL) are written into the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) as binlog events. A slave server connects to the master and reads the binlog
events, then applies the events locally to replicate the same changes as done
on the master. A server can be both a master and a slave at the same time, and
it is thus possible for binlog events to replicated through multiple levels of
servers.

A slave server keeps track of the position in the master's binlog of the last
event applied on the slave. This allows the slave server to re-connect and
resume from where it left off after replication has been temporarily
stopped. It also allows a slave to disconnect, be cloned and then have the new slave resume replication from the same master.

Global transaction ID introduces a new event attached to each event group in
the binlog. (An event group is a collection of events that are always applied
as a unit. They are best thought of as a "transaction", though they also
include non-transactional DML statements, as well as DDL). As an event group
is replicated from master server to slave server, the global transaction ID is
preserved. Since the ID is globally unique across the entire group of servers,
this makes it easy to uniquely identify the same binlog events on different
servers that replicate each other (this was not easily possible before [MariaDB
10.0.2](/kb/en/mariadb-1002-release-notes/)).

## Benefits

Using global transaction ID provides two main benefits:

1. Easy to change a slave server to connect to and replicate from a different
master server.

The slave remembers the global transaction ID of the last event group
    applied from the old master. This makes it easy to know where to resume
    replication on the new master, since the global transaction IDs are known
    throughout the entire replication hierarchy. This is not the case when
    using old-style replication; in this case the slave knows only the
    specific file name and offset of the old master server of the last event
    applied. There is no simple way to guess from this the correct file name
    and offset on a new master.

2. The state of the slave is recorded in a crash-safe way.

The slave keeps track of its current position (the global transaction ID of the last transaction applied) in the [mysql.gtid_slave_pos](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgtid_slave_pos-table/) system table. If this table is using a transactional storage engine (such as InnoDB, which is the default), then updates to the state are done in the same transaction as the updates to the data. This makes the state crash-safe; if the slave server crashes, crash recovery on restart will make sure that the recorded replication position matches the changes that were actually replicated. This is not the case for old-style replication, where the state is recorded in a file relay-log.info, which is updated independently of the actual data changes and can easily get out of sync if the slave server crashes. (This works for DML to transactional tables; non-transactional tables and DDL in general are not crash-safe in MariaDB.)

Because of these two benefits, it is generally recommended to use global
transaction ID for any replication setups based on [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/) or later.
However, old-style replication continues to work as always, so there is no
pressing need to change existing setups. Global transaction ID integrates
smoothly with old-style replication, and the two can be used freely together
in the same replication hierarchy. There is no special configuration needed of
the server to start using global transaction ID. However, it must be
explicitly set for a slave server with the appropriate [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) option;
by default old-style replication is used by a replication slave, to maintain
backwards compatibility.

## Implementation

A global transaction ID, or GTID for short, consists of three numbers
separated with dashes '-'. For example:

<code class="fixed" style="white-space:pre-wrap">0-1-10</code>

- The first number 0 is the domain ID, which is specific for global transaction ID (more on this below). It is a 32-bit unsigned integer.
- The second number is the server ID, the same as is also used in old-style replication. It is a 32-bit unsigned integer.
- The third number is the sequence number. This is a 64-bit unsigned integer that is monotonically increasing for each new event group logged into the binlog.

The server ID is set to the server ID of the server where the event group is
first logged into the binlog. The sequence number is increased on a server for
every event group logged. Since server IDs must be unique for every server,
this makes the (server_id, sequence_number) pair, and hence the whole GTID,
globally unique.

Using a 64-bit number provides ample range that there should be no risk of it
overflowing in the foreseeable future. However, one should not artificially
(by setting <code class="fixed" style="white-space:pre-wrap">gtid_seq_no</code>) inject a GTID with a very high
sequence number close to the limit of 64-bit.

### The Domain ID

When events are replicated from a master server to a slave server, the events
are always logged into the slave's binlog in the same order that they were
read from the master's binlog. Thus, if there is only ever a single master
server receiving (non-replication) updates at a time, then the binlog order
will be identical on every server in the replication hierarchy.

This consistent binlog order is used by the slave to keep track of its current
position in the replication. Basically, the slave remembers the GTID of the
last event group replicated from the master. When reconnecting to a master,
whether the same one or a new one, it sends this GTID position to the master,
and the master starts sending events from the first event after the
corresponding event group.

However, if user updates are done independently on multiple servers at the
same time, then in general it is not possible for binlog order to be identical
across all servers. This can happen when using multi-source replication, with
multi-master ring topologies, or just if manual updates are done on a slave
that is replicating from active master. If the binlog order is different on
the new master from the order on the old master, then it is not sufficient for
the slave to keep track of a single GTID to completely record the current
state.

The domain ID, the first component of the GTID, is used to handle this.

In general, the binlog is not a single ordered stream. Rather, it consists of
a number of different streams, each one identified by its own domain
ID. Within each stream, GTIDs always have the same order in every server
binlog. However, different streams can be interleaved in different ways on
different servers.

A slave server then keeps track of its replication position by recording the
last GTID applied within each replication stream. When connecting to a new
master, the slave can start replication from a different point in the binlog
for each domain ID.

For more details on using multi-master setups and multiple domain IDs, see [Use with multi-source replication and other multi-master setups](#use-with-multi-source-replication-and-other-multi-master-setups).

Simple replication setups only have a single master being updated by the
application at any one time. In such setups, there is only a single
replication stream needed. Then domain ID can be ignored, and left as the
default of 0 on all servers.

## Using Global Transaction IDs

From [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/), global transaction ID is enabled automatically. Each event
group logged to the binlog receives a GTID event, as can be seen with
[mysqlbinlog](/clients-utilities/mysqlbinlog/) or [SHOW BINLOG EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-binlog-events/).

The slave automatically keeps track of the GTID of the last applied event
group, as can be seen from the [gtid_slave_pos](#gtid_slave_pos) variable:

```sql
SELECT @@GLOBAL.gtid_slave_pos
0-1-1
```

When a slave connects to a master, it can use either global transaction ID or
old-style filename/offset to decide where in the master binlogs to start
replicating from. To use global transaction ID, use the [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) <em>master_use_gtid</em> option:

<code class="fixed" style="white-space:pre-wrap">CHANGE MASTER TO master_use_gtid = { slave_pos | current_pos | no }</code>

A slave is configured to use GTID by <code class="fixed" style="white-space:pre-wrap">CHANGE MASTER TO master_use_gtid=slave_pos</code>.
When the slave connects to the master, it will start replication at the
position of the last GTID replicated to the slave, which can be seen in the
variable [gtid_slave_pos](#gtid_slave_pos).
Since GTIDs are the same across all replication
servers, the slave can then be pointed to a different master, and the correct
position will be determined automatically.

But suppose that we set up two servers A and B and let A be the master and B
the slave. It runs for a while. Then at some point we take down A, and B
becomes the new master. Then later we want to add A back, this time as a
slave.

Since A was never a slave before, it does not have any prior replicated GTIDs,
and [gtid_slave_pos](#gtid_slave_pos) will be empty.
To allow A to be added as a slave automatically,
<code class="fixed" style="white-space:pre-wrap">master_use_gtid=current_pos</code> can be used. This will connect
using the value of the variable [gtid_current_pos](#gtid_current_pos) instead of
[gtid_slave_pos](#gtid_slave_pos), which also takes into account GTIDs written
into the binlog when the server was a master.

When using `master_use_gtid=current_pos` there is no need to consider whether a server was a master or a slave prior to using [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/). But care must be taken not to
inject extra transactions into the binlog on the slave server that are not
intended to be replicated to other servers. If such an extra transaction
is the most recent when the slave starts, it will be used as the
starting point of replication. This will probably fail because that
transaction is not present on the master. To avoid local changes on a slave
server to go into the binlog, set [sql_log_bin](/kb/en/replication-and-binary-log-system-variables/#sql_log_bin) to 0.

If it is undesirable that changes to the binlog on the slave affects the
GTID replication position, then <code class="fixed" style="white-space:pre-wrap">master_use_gtid=slave_pos</code>
should be used. Then the slave will always connect to the master at the
position of the last replicated GTID. This may avoid some surprises for users
that expect behaviour consistent with traditional replication, where the
replication position is never changed by local changes done on a server.

When [GTID strict mode](#gtid_strict_mode) is enabled (by setting
<code class="fixed" style="white-space:pre-wrap">@@GLOBAL.gtid_strict_mode</code> to 1), it is normally best to use
<code class="fixed" style="white-space:pre-wrap">current_pos</code>. In strict mode, extra transactions on the master are
disallowed.

If a slave is configured with the binlog disabled,
<code class="fixed" style="white-space:pre-wrap">current_pos</code> and <code class="fixed" style="white-space:pre-wrap">slave_pos</code> are equivalent.

Even when a slave is configured to connect with the old-style binlog filename
and offset (<code class="fixed" style="white-space:pre-wrap">CHANGE MASTER TO master_log_file=..., master_log_pos=...</code>), it will
still keep track of the current GTID position in <code class="fixed" style="white-space:pre-wrap">@@GLOBAL.gtid_slave_pos</code>. This
means that an existing slave previously configured and running can be changed
to connect with GTID (to the same or a new master) simply with:

<code class="fixed" style="white-space:pre-wrap">CHANGE MASTER TO master_use_gtid = slave_pos</code>

The slave remembers that <code class="fixed" style="white-space:pre-wrap">master_use_gtid=slave_pos|master_pos</code>
was specified and will use it also
for subsequent connects, until it is explicitly changed by specifying
<code class="fixed" style="white-space:pre-wrap">master_log_file/pos=...</code> or
<code class="fixed" style="white-space:pre-wrap">master_use_gtid=no</code>.
The current value can be seen as the field Using_Gtid of SHOW SLAVE STATUS:

```sql
SHOW SLAVE STATUS\G
...
Using_Gtid: Slave_pos
```

The slave server internally uses the [mysql.gtid_slave_pos table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgtid_slave_pos-table/) to store the
GTID position (and so preserve the value of <code class="fixed" style="white-space:pre-wrap">@@GLOBAL.gtid_slave_pos</code> across
server restarts). After upgrading a server to 10.0, it is necessary to run
[mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade/) (as always) to get the table created.

In order to be crash-safe, this table must use a transactional storage engine
such as InnoDB. When MariaDB is first installed (or upgraded to 10.0.2+) the
table is created using the default storage engine - which itself defaults to
InnoDB. If there is a need to change the storage engine for this table (to
make it transactional on a system configured with [MyISAM](/kb/en/myisam/) as the default
storage engine, for example), use [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/):

<code class="fixed" style="white-space:pre-wrap">ALTER TABLE mysql.gtid_slave_pos ENGINE = InnoDB</code>

The [mysql.gtid_slave_pos table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgtid_slave_pos-table/) should not be modified in any other way. In
particular, do not try to update the rows in the table to change the slave's
idea of the current GTID position; instead use

<code class="fixed" style="white-space:pre-wrap">SET GLOBAL gtid_slave_pos = '0-1-1'</code>

Starting from [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), the server variable [gtid_pos_auto_engines](#gtid_pos_auto_engines) can
preferably be set to make the server handle this automatically. See the
description of the [mysql.gtid_slave_pos table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgtid_slave_pos-table/) for details.

##### MariaDB [10.0.2](/kb/en/mariadb-1002-release-notes/)

In 10.0.2, the name of the table that stores the GTID position was called
<code class="fixed" style="white-space:pre-wrap">rpl_slave_state</code>.
The syntax <code class="fixed" style="white-space:pre-wrap">master_use_gtid=0|1</code> was used instead of
<code class="fixed" style="white-space:pre-wrap">master_use_gtid=no|current_pos</code>. The
<code class="fixed" style="white-space:pre-wrap">master_use_gtid=slave_pos</code> option did not have any equivalent.

### Using `current_pos` vs. `slave_pos`

When setting the <a undefined>MASTER_USE_GTID</a> replication parameter, you have the option of enabling Global Transaction IDs to use either the `current_pos` or `slave_pos` values.

Using the value `current_pos` causes the slave to set its position based on the <a undefined>gtid_current_pos</a> system variable, which is a union of <a undefined>gtid_binlog_pos</a> and <a undefined>gtid_slave_pos</a>. Using the value `slave_pos` causes the slave to instead set its position based on the <a undefined>gtid_slave_pos</a> system variable.

You may run into issues when you use the value `current_pos` if you write any local transactions on the slave. For instance, if you issue an [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) statement or otherwise write to a table while the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) are stopped, then new local GTIDs may be generated in <a undefined>gtid_binlog_pos</a>, which will affect the slave's value of <a undefined>gtid_current_pos</a>. This may cause errors when the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) are restarted, since the local GTIDs will be absent from the master.

You can correct this issue by setting the <a undefined>MASTER_USE_GTID</a> replication parameter to `slave_pos` instead of `current_pos`. For example:

```sql
CHANGE MASTER TO MASTER_USE_GTID = slave_pos;
START SLAVE;
```

### Using GTIDs with Parallel Replication

If [parallel replication](/replication/standard-replication/parallel-replication/) is in use, then events that were logged with GTIDs with different <a undefined>gtid_domain_id</a> values can be applied in parallel in an [out-of-order](/kb/en/parallel-replication/#out-of-order-parallel-replication) manner.

### Using GTIDs with MariaDB Galera Cluster

Starting with [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/), MariaDB Galera Cluster has limited support for GTIDs. See [Using MariaDB GTIDs with MariaDB Galera Cluster](/replication/galera-cluster/using-mariadb-replication-with-mariadb-galera-cluster/using-mariadb-gtids-with-mariadb-galera-cluster/) for more information.

## Setting up a New Slave Server with Global Transaction ID

Setting up a new replication slave server with global transaction ID is not
much different from setting up an old-style slave. The basic steps are:

1. Setup the new server and load it with the initial data.

2. Start the slave replicating from the appropriate point in the master's
   binlog.

### Setting up a New Slave with an Empty Server

The simplest way for testing purposes is probably to setup a new, empty slave
server and replicate all of the master's binlogs from the start (this is
usually not feasible in a realistic production setup, as the initial binlog
files will probably have been purged or take too long to apply).

The slave server is installed in the normal way. By default, the GTID position
for a newly installed server is empty, which makes the slave replicate from
the start of the master's binlogs. But if the slave was used for other
purposes before, the initial position can be explicitly set to empty first:

<code class="fixed" style="white-space:pre-wrap">SET GLOBAL gtid_slave_pos = "";</code>

Next, point the slave to the master with [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/). Specify master_host
etc. as usual. But instead of specifying master_log_file and master_log_pos
manually, use <code class="fixed" style="white-space:pre-wrap">master_use_gtid=current_pos</code> (or
<code class="fixed" style="white-space:pre-wrap">slave_pos</code> to have GTID do it automatically:

```sql
CHANGE MASTER TO master_host="127.0.0.1", master_port=3310, master_user="root", master_use_gtid=current_pos;
START SLAVE;
```

### Setting up a New Slave From a Backup

The normal way to set up a new replication slave is to take a backup from
an existing server (either a master or slave in the replication topology), and then restore that backup on the server acting as the new slave, and the configure it to start replicating from the appropriate position in the master's binary log.

It is important that the position at which replication is started corresponds
exactly to the state of the data at the point in time that the backup was
taken. Otherwise, the slave can end up with different data than the master
because of missing or duplicated transactions. Of course, if there are no writes to the server being backed up during the backup process, then a simple <a undefined>SHOW MASTER STATUS</a> will give the correct position.

See the description of the specific backup tool to determine how to get the binary log position that corresponds to the backup.

Once the current binary log position for the backup has been obtained, in the form
of a binary log file name and position, the corresponding GTID position can be
obtained from [BINLOG_GTID_POS()](/built-in-functions/secondary-functions/information-functions/binlog_gtid_pos/) on the server that was backed up:

```sql
SELECT BINLOG_GTID_POS("master-bin.000001", 600);
```

The new slave can then start replicating from the master by setting the correct value for
<a undefined>gtid_slave_pos</a>, and then executing [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) with the relevant values for the master, and then starting the [slave threads](/kb/en/replication-threads/#threads-on-the-slave) by executing <a undefined>START SLAVE</a>. For example:

```sql
SET GLOBAL gtid_slave_pos = "0-1-2";
CHANGE MASTER TO master_host="127.0.0.1", master_port=3310, master_user="root", master_use_gtid=slave_pos;
START SLAVE;
```

This method is particularly useful when setting up a new slave from a backup of the master. Remember to ensure that the value of <a undefined>server_id</a> configured on the new slave is different from that of any other server in the replication topology.

If the backup was taken of an existing slave server, then the new slave should already have the
correct GTID position stored in the [mysql.gtid_slave_pos](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgtid_slave_pos-table/) table. This is assuming that this table was backed up and that it was backed up in a consistent manner with changes to other tables. In this case, there is no need to explicitly look up the GTID position on the old server and set it on the new slave - it will be already correctly loaded from the [mysql.gtid_slave_pos](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgtid_slave_pos-table/) table. This however does not work if the backup was taken from the master - because then the current GTID position is contained in the binary log, not in the [mysql.gtid_slave_pos](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgtid_slave_pos-table/) table or any other table.

#### Setting up a New Slave with Mariabackup

A new slave can easily be set up with [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/), which is a fork of [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/). See [Setting up a Replication Slave with Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/setting-up-a-replication-slave-with-mariabackup/) for more information.

#### Setting up a New Slave with mysqldump

A new slave can also be set up with [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/).

Starting with [MariaDB 10.0.13](/kb/en/mariadb-10013-release-notes/), [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) automatically includes the
GTID position as a comment in the backup file if either the <a undefined>--master-data</a> or <a undefined>--dump-slave</a> option is used. It also automatically includes the commands to set <a undefined>gtid_slave_pos</a> and execute [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/)
in the backup file if the <a undefined>--gtid</a> option is used with either the <a undefined>--master-data</a> or <a undefined>--dump-slave</a> option.

### Switching An Existing Old-Style Slave To Use GTID.

If there is already an existing slave running using old-style binlog
filename/offset position, then this can be changed to use GTID directly. This
can be useful for upgrades for example, or where there are already tools to
setup new slaves using old-style binlog positions.

When a slave connects to a master using old-style binlog positions, and the
master supports GTID (ie. is [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/) or later), then the slave
automatically downloads the GTID position at connect and updates it during
replication. Thus, once a slave has connected to the GTID-aware master at
least once, it can be switched to using GTID without any other actions needed;

```sql
STOP SLAVE;
CHANGE MASTER TO master_host="127.0.0.1", master_port=3310, master_user="root", master_use_gtid=current_pos;
START SLAVE;
```

(A later version will probably add a way to setup the slave so that it will
connect with old-style binlog file/offset the first time, and automatically
switch to using GTID on subsequent connects.)

## Changing a Slave to Replicate From a Different Master

Once replication is running with GTID (master_use_gtid=current_pos|slave_pos),
the slave can be
pointed to a new master simply by specifying in CHANGE MASTER the new
master_host (and if required master_port, master_user, and master_password):

```sql
STOP SLAVE;
CHANGE MASTER TO master_host='127.0.0.1', master_port=3312;
START SLAVE;
```

The slave has a record of the GTID of the last applied transaction from the
old master, and since GTIDs are identical across all servers in a replication
hierarchy, the slave will just continue from the appropriate point in the new
master's binlog.

It is important to understand how this change of masters work. The binlog is
an ordered stream of events (or multiple streams, one per replication domain,
(see [Use with multi-source replication and other multi-master setups](#use-with-multi-source-replication-and-other-multi-master-setups)). Events within the stream are always applied in
the same order on every slave that replicates it. The MariaDB GTID relies on
this ordering, so that it is sufficient to remember just a single point within
the stream. Since event order is the same on every server, switching to the
point of the same GTID in the binlog of another server will give the same
result.

This translates into some responsibility for the user. The MariaDB GTID
replication is fully asynchronous, and fully flexible in how it can be
configured. This makes it possible to use it in ways where the assumption that
binlog sequence is the same on all servers is violated. In such cases, when
changing master, GTID will still attempt to continue at the point of current
GTID in the new binlog.

The most common way that binlog sequence gets different between servers is
when the user/DBA does updates directly on a slave server (and these updates
are written into the slaves binlog). This results in events in the slaves
binlog that are not present on the master or any other slaves. This can be
avoided by setting the session variable sql_log_bin false while doing such
updates, so they do not go into the binlog.

It is normally best to avoid any differences in binlogs between servers. That
being said, MariaDB replication is designed for maximum flexibility, and there
can be valid reasons for introducing such differences from time to time. It
this case, it just needs to be understood that the GTID position is a single
point in each binlog stream (one per replication domain), and how this affects
the users particular setup.

Differences can also occur when two masters are active at the same time in a
replication hierarchy. This happens when using a multi-master ring. But it can
also occur in a simple master-slave setup, during switch to a new master, if
changes on the old master is not allowed to fully replicate to all slave
servers before switching master. Normally, to switch master, first writes to
the old master should be stopped, then one should wait for all changes to be
replicated to the new master, and only then should writes begin on the new
master. Deliberately using multiple active masters is also supported, this is
described in the next section.

The [GTID strict mode](#gtid_strict_mode) can be used to enforce identical binlogs across
servers. When it is enabled, most actions that would cause differences are
rejected with an error.

## Use With Multi-Source Replication and Other Multi-Master Setups

MariaDB global transaction ID supports having multiple masters active at the
same time. Typically this happens with either multi-source replication or
multi-master ring setups.

In such setups, each active master must be configured with its own distinct
replication domain ID, [gtid_domain_id](#gtid_domain_id). The binlog will then in effect consists
of multiple independent streams, one per active master. Within one replication
domain, binlog order is always the same on every server. But two different
streams can be interleaved differently in different server binlogs.

The GTID position of a given slave is then not a single GTID. Rather, it
becomes the GTID of the last event group applied for each value of domain ID,
in effect the position reached in each binlog stream. When the slave connects
to a master, it can continue from one stream in a different binlog position
than another stream. Since order within one stream is consistent across all
servers, this is sufficient to always be able to continue replication at the
correct point in any new master server(s).

Domain IDs are assigned by the DBA, according to the need of the application.
The default value of @@GLOBAL.gtid_domain_id is 0. This is appropriate for
most replication setups, where only a single master is active at a time. The
MariaDB server will never by itself introduce new domain_id values into the
binlog.

When using multi-source replication, where a single slave connects to multiple
masters at the same time, each such master should be configured with its own
distinct domain ID.

Similarly, in a multi-master ring topology, where all master in the ring are
updated by the application concurrently (with some mechanism to avoid
conflicts), a distinct domain ID should be configured for each server (In a
multi-master ring where the application is careful to only do updates on one
master at a time, a single domain ID is sufficient).

Normally, a slave server should not receive direct updates (as this creates
binlog differences compared to the master). Thus it does not matter what value
of gtid_domain_id is set on a slave, though it may make sense to make it the
same as the master (if not using multi-master) to make it easy to promote the
slave as a new master. Of course, if a slave is itself an active master, as in
a multi-master ring topology, the domain ID should be set according to the
server's role as active master.

Note that domain ID and server ID are distinct concepts. It is possible to use
a different domain ID on each server, but this is normally not desirable. It
makes the current GTID position (@@global.gtid_slave_pos) more complicated to
understand and work with, and loses the concept of a single ordered binlog
stream across all servers. It is recommended only to configure as many domain
IDs as there are master servers actively being updated by the application at
the same time.

It is not an error in itself to configure domain IDs incorrectly (for example,
not configuring them at all). For example, this will be typical in an upgrade
scenario where a multi-master ring using 5.5 is upgraded to 10.0. The ring
will continue to work as before even though everything is configured to use
the default domain ID 0. It is even possible to use GTID for replication
between the servers. However, care must be taken when switching a slave to a
different master. If the binlog order between the old and the new master
differs, then a single GTID position to start replication from in the new
master's binlog may not be sufficient.

## New Syntax For Global Transaction ID

### CHANGE MASTER

[CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) has a new option, `master_use_gtid=[current_pos|slave_pos|no]`. When enabled (set to
<em>current_pos</em> or <em>slave_pos</em>), the slave will connect to the master using the GTID position. When
disabled (set to "no"), the old-style binlog filename/offset position is used to decide
where to start replicating when connecting. Unlike in the old-style, when GTID is enabled, the values of the [MASTER_LOG_FILE](/kb/en/change-master-to/#master_log_file) and [MASTER_LOG_POS](/kb/en/change-master-to/#master_log_pos) options
are not updated per received event in  [master_info_file](/kb/en/mysqld-options/#-master-info-file) file.

The value of <code class="fixed" style="white-space:pre-wrap">master_use_gtid</code> is saved across server restarts (in
master.info). The current value can be seen as the field Using_Gtid in the
output of SHOW SLAVE STATUS.

For a detailed look at the difference between the <em>current_pos</em> and <em>slave_pos</em> options, see [Using global transaction IDs](#using-global-transaction-ids)

##### MariaDB [10.0.2](/kb/en/mariadb-1002-release-notes/)

In [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/) only, the syntax <code class="fixed" style="white-space:pre-wrap">master_use_gtid=0|1</code> was used instead of
<code class="fixed" style="white-space:pre-wrap">master_use_gtid=no|current_pos</code>. The
<code class="fixed" style="white-space:pre-wrap">master_use_gtid=slave_pos</code> option did not have any equivalent.

### START SLAVE UNTIL master_gtid_pos=xxx

When starting replication with [START SLAVE](/kb/en/start-slave/), it is possible to request the
slave to run only until a specific GTID position is reached. Once that
position is reached, the slave will stop.

The syntax for this is:

<code class="fixed" style="white-space:pre-wrap">    START SLAVE UNTIL master_gtid_pos = &lt;GTID position&gt;</code>

The slave will start replication from the current GTID position, run up to
and including the event with the GTID specified, and then stop.
Note that this stops both the IO thread and the SQL thread (unlike START
SLAVE UNTIL MASTER_LOG_FILE/MASTER_LOG_POS, which stops only the SQL
thread).

If multiple GTIDs are specified, then they must be with distinct replication
domain ID, for example:

<code class="fixed" style="white-space:pre-wrap">    START SLAVE UNTIL master_gtid_pos = "1-11-100,2-21-50"</code>

With multiple domains in the UNTIL condition, each domain runs only up to and
including the specified position, so it is possible for different domains to
stop at different places in the binlog (each domain will resume from the
stopped position when the slave is started the next time).

Not specifying a replication domain at all in the UNTIL condition means that
the domain is stopped immediately, nothing is replicated from that domain. In
particular, specifying the empty string will stop the slave immediately.

When using <code class="fixed" style="white-space:pre-wrap">START SLAVE UNTIL master_gtid_pos = XXX</code>, if the UNTIL position is
present in the master's binlog then it is permissible for the start position
to be missing on the master. In this case, replication for the associated
domains stop immediately.

Both slave threads must be already stopped when using UNTIL master_gtid_pos,
otherwise an error occurs. It is also an error if the slave is not configured
to use GTID (<code class="fixed" style="white-space:pre-wrap">CHANGE MASTER TO master_use_gtid=current_pos|slave_pos</code>). And both threads must be
started at the same time, the <code class="fixed" style="white-space:pre-wrap">IO_THREAD</code> or <code class="fixed" style="white-space:pre-wrap">SQL_THREAD</code> options can not be used
to start only one of them.

<code class="fixed" style="white-space:pre-wrap">START SLAVE UNTIL master_gtid_pos=XXX</code> is particularly useful for promoting a
new master among a set of slaves when the old master goes away and slaves may
have reached different positions in the old master's binlog. The new master
needs to be ahead of all the other slaves to avoid losing events. This can be
achieved by picking one server, say S1, and replicating any missing events
from each other server S2, S3, ..., Sn:

```sql
    CHANGE MASTER TO master_host="S2";
    START SLAVE UNTIL master_gtid_pos = "<S2 GTID position>";
    ...
    CHANGE MASTER TO master_host="Sn";
    START SLAVE UNTIL master_gtid_pos = "<Sn GTID position>";
```

Once this is completed, S1 will have all events present on any of the
servers. It can now be selected as the new master, and all the other servers
set to replicate from it.

##### MariaDB starting with [10.0.3](/kb/en/mariadb-1003-release-notes/)

Note: START SLAVE UNTIL first appeared in [MariaDB 10.0.3](/kb/en/mariadb-1003-release-notes/).

### BINLOG_GTID_POS().

The [BINLOG_GTID_POS()](/built-in-functions/secondary-functions/information-functions/binlog_gtid_pos/) function takes as input an old-style [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position in the form of a file name and a file offset. It looks up the position in the current binlog, and returns a string representation of the corresponding GTID
position. If the position is not found in the current binlog, NULL is returned.

### MASTER_GTID_WAIT

The [MASTER_GTID_WAIT](/built-in-functions/secondary-functions/miscellaneous-functions/master_gtid_wait/) function was introduced in [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/). It is useful in replication for controlling master/slave synchronization, and blocks until the slave has read and applied all updates up to the specified position in the master log. See [MASTER_GTID_WAIT](/built-in-functions/secondary-functions/miscellaneous-functions/master_gtid_wait/) for details.

## System Variables

#### `gtid_slave_pos`

This system variable contains the GTID of the last transaction applied to the database by the server's [slave threads](/kb/en/replication-threads/#threads-on-the-slave) for each replication domain. This system variable's value is automatically updated whenever a [slave thread](/kb/en/replication-threads/#threads-on-the-slave) applies an event group. This system variable's value can also be manually changed by users, so that the user can change the GTID position of the [slave threads](/kb/en/replication-threads/#threads-on-the-slave).

When using [multi-source replication](/replication/standard-replication/multi-source-replication/), the same GTID position is shared by all slave connections. In this case, different masters should use different replication domains by configuring different <a undefined>gtid_domain_id</a> values. If one master were using a <a undefined>gtid_domain_id</a> value of `1`, and if another master were using a <a undefined>gtid_domain_id</a> value of `2`, then any slaves replicating from both masters would have GTIDs with both <a undefined>gtid_domain_id</a> values in `gtid_slave_pos`.

This system variable's value can be manually changed by executing <a undefined>SET GLOBAL</a>, but all slave threads to be stopped with <a undefined>STOP SLAVE</a> first. For example:

```sql
STOP ALL SLAVES;
SET GLOBAL gtid_slave_pos = "1-10-100,2-20-500";
START ALL SLAVES;
```

This system variable's value can be reset by manually changing its value to the empty string. For example:

```sql
SET GLOBAL gtid_slave_pos = '';
```

The GTID position defined by `gtid_slave_pos` can be used as a slave's starting replication position by setting <a undefined>MASTER_USE_GTID=slave_pos</a> when the slave is configured with the [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) statement. As an alternative, the <a undefined>gtid_current_pos</a> system variable can also be used as a slave's starting replication position.

If a user sets the value of the `gtid_slave_pos` system variable, and <a undefined>gtid_binlog_pos</a> contains later GTIDs for certain replication domains, then <a undefined>gtid_current_pos</a> will contain the GTIDs from <a undefined>gtid_binlog_pos</a> for those replication domains. To protect users in this scenario, if a user sets tthe `gtid_slave_pos` system variable to a GTID position that is behind the GTID position in <a undefined>gtid_binlog_pos</a>, then the server will give the user a warning.

This can help protect the user when the slave is configured to use <a undefined>gtid_current_pos</a>  as its replication position. This can also help protect the user when a server has been rolled back to restart replication from an earlier point in time, but the user has forgotten to reset <a undefined>gtid_binlog_pos</a> with [RESET MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-master/).

The [mysql.gtid_slave_pos](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgtid_slave_pos-table/) system table is used to store the contents of global.gtid_slave_pos and preserve it over restarts.

- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default:</strong> Null
- <strong>Introduced:</strong> [MariaDB 10.0.3](/kb/en/mariadb-1003-release-notes/)

#### `gtid_binlog_pos`

This variable is the GTID of the last event group written to the binary log,
for each replication domain.

Note that when the binlog is empty (such as on a fresh install or after [RESET MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-master/)), there are no event groups written in any replication domain, so in
this case the value of `gtid_binlog_pos` will be the empty string.

The value is read-only, but it is updated whenever a DML or DDL statement is
written to the binary log. The value can be reset by executing [RESET MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-master/), which will also delete all binary logs. However, note that [RESET MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-master/) does not also reset [gtid_slave_pos](#gtid_slave_pos). Since [gtid_current_pos](#gtid_current_pos) is the union of [gtid_slave_pos](#gtid_slave_pos) and `gtid_binlog_pos`, that means that new GTIDs added to `gtid_binlog_pos` can lag behind those in [gtid_current_pos](#gtid_current_pos) if [gtid_slave_pos](#gtid_slave_pos) contains GTIDs in the same domain with higher sequence numbers. If you want to reset [gtid_current_pos](#gtid_current_pos) for a specific GTID domain in cases like this, then you will also have to change [gtid_slave_pos](#gtid_slave_pos) in addition to executing [RESET MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-master/). See [gtid_slave_pos](#gtid_slave_pos) for notes on how to change its value.

- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Read-only
- <strong>Data Type:</strong> `string`
- <strong>Default:</strong> Null
- <strong>Introduced:</strong> [MariaDB 10.0.3](/kb/en/mariadb-1003-release-notes/)

#### `gtid_binlog_state`

The variable gtid_binlog_state holds the internal state of the binlog. The
state consists of the last GTID ever logged to the binary log for every
combination of domain_id and server_id. This information is used by the master
to determine whether a given GTID has been logged to the binlog in the past,
even if it has later been deleted due to binlog purge. For each domain_id, the
last entry in @@gtid_binlog_state is the last GTID logged into binlog,
ie. this is the value that appears in @@gtid_binlog_pos.

Normally this internal state is not needed by users, as @@gtid_binlog_pos is
more useful in most cases. The main usage of @@gtid_binlog_state is to restore
the state of the binlog after RESET MASTER (or equivalently if the binlog
files are lost). If the value of @@gtid_binlog_state is saved before RESET
MASTER and restored afterwards, the master will retain information about past
history, same as if PURGE BINARY LOGS had been used (of course the actual
events in the binary logs are still deleted).

Note that to set the value of @@gtid_binlog_state, the binary log must be
empty, that is it must not contain any GTID events and the previous value of
@@gtid_binlog_state must be the empty string. If not, then RESET MASTER must
be used first to erase the binary log first.

The value of @@gtid_binlog_state is preserved by the server across restarts by
writing a file MASTER-BIN.state, where MASTER-BIN is the base name of the
binlog set with the --log-bin option. This file is written at server shutdown,
and re-read at next server start. (In case of a server crash, the data in the
MASTER-BIN.state is not correct, and the server instead recovers the correct
value during binlog crash recovery by scanning the binlog files and recording
each GTID found).

For completeness, note that setting @@gtid_binlog_state internally executes a
RESET MASTER. This is normally not noticeable as it can only be changed when
the binlog is empty of GTID events. However, if executed e.g. immediately after
upgrading to MariaDB 10, it is possible that the binlog is non-empty but
without any GTID events, in which case all such events will be deleted, just
as if RESET MASTER had been run.

- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string`
- <strong>Default:</strong> Null
- <strong>Introduced:</strong> [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)

#### `gtid_current_pos`

This system variable contains the GTID of the last transaction applied to the database for each replication domain.

The value of this system variable is constructed from the values of the <a undefined>gtid_binlog_pos</a> and <a undefined>gtid_slave_pos</a> system variables. It gets GTIDs of transactions executed locally from the value of the <a undefined>gtid_binlog_pos</a> system variable. It gets GTIDs of replicated transactions from the value of the <a undefined>gtid_slave_pos</a> system variable.

For each replication domain, if the <a undefined>server_id</a> of the corresponding GTID in
<a undefined>gtid_binlog_pos</a> is equal to the servers own <a undefined>server_id</a>,
<em>and</em> the sequence number is higher than the corresponding GTID in
<a undefined>gtid_slave_pos</a>, then the GTID from
<a undefined>gtid_binlog_pos</a> will be used. Otherwise the GTID from
<a undefined>gtid_slave_pos</a> will be used for that domain.

GTIDs from <a undefined>gtid_binlog_pos</a> in which the <a undefined>server_id</a> of the GTID is <strong>not</strong> equal to the server's own <a undefined>server_id</a> are effectively ignored. If <a undefined>gtid_binlog_pos</a> contains a GTID for a given replication domain, but the <a undefined>server_id</a> of the GTID is <strong>not</strong> equal to the server's own <a undefined>server_id</a>, and <a undefined>gtid_slave_pos</a> does <strong>not</strong> contain a GTID for that given replication domain, then `gtid_current_pos` will <strong>not</strong> contain any GTID for that replication domain.

Thus, `gtid_current_pos` contains the most recent GTID
executed on the server, whether this was done as a master or as a slave.

The GTID position defined by `gtid_current_pos` can be used as a slave's starting replication position by setting <a undefined>MASTER_USE_GTID=current_pos</a> when the slave is configured with the [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) statement. As an alternative, the <a undefined>gtid_slave_pos</a> system variable can also be used as a slave's starting replication position.

The value of `gtid_current_pos` is read-only, but it is updated whenever a transaction is
written to the binary log and/or replicated by a slave thread, and that transaction's GTID is considered <em>newer</em> than the current GTID for that domain. See above for the rules on how to determine if a GTID would be considered <em>newer</em>.

If you need to reset the value, see the notes on resetting <a undefined>gtid_slave_pos</a> and <a undefined>gtid_binlog_pos</a>, since `gtid_current_pos` is formed from the values of those variables.

- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Read-only
- <strong>Data Type:</strong> `string`
- <strong>Default:</strong> Null
- <strong>Introduced:</strong> [MariaDB 10.0.3](/kb/en/mariadb-1003-release-notes/)

#### `gtid_strict_mode`

The GTID strict mode is an optional setting that can be used to help the DBA
enforce a strict discipline about keeping binlogs identical across multiple
servers replicating using global transaction ID.

When GTID strict mode is enabled, some additional errors are enabled for
situations that could otherwise cause differences between binlogs on different
servers in a replication hierarchy:

1 If a slave server tries to replicate a GTID with a sequence number lower than what is already in the binlog for that replication domain, the SQL thread stops with an error (this indicates an extra transaction in the slave binlog not present on the master).
2 Similarly, an attempt to manually binlog a GTID with a lower sequence number (by setting <code class="fixed" style="white-space:pre-wrap">@@SESSION.gtid_seq_no</code>) is rejected with an error.
3 If the slave tries to connect starting at a GTID that is missing in the master's binlog, this is an error in GTID strict mode even if a GTID exists with a higher sequence number (this indicates a GTID on the slave missing on the master). Note that this error is controlled by the setting of GTID strict mode on the connecting slave server.

GTID mode is off by default; this is needed to preserve backwards
compatibility with existing replication setups (older versions of the server
did not enforce any strict mode for binlog order). Global transaction ID is
designed to work correctly even when strict mode is not enabled. However, with
strict mode enforced, the semantics is simpler and thus easier to understand,
because binlog order is always identical across servers and sequence numbers
are always strictly increasing within each replication domain. This can also
make automated scripting of large replication setups easier to implement
correctly.

When GTID strict mode is enabled, the slave will stop with an error when a
problem is encountered. This allows the DBA to become aware of the problem and
take corrective actions to avoid similar issues in the future. One way to
recover from such an error is to temporarily disable GTID strict mode on the
offending slave, to be able to replicate past the problem point (perhaps using
<code class="fixed" style="white-space:pre-wrap">START SLAVE UNTIL master_gtid_pos=XXX</code>).

- <strong>Commandline:</strong> `--gtid-strict-mode[={0|1}]`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default:</strong> `Off`
- <strong>Introduced:</strong> [MariaDB 10.0.3](/kb/en/mariadb-1003-release-notes/)

---

#### `gtid_domain_id`

- <strong>Description:</strong> This variable is used to decide which replication domain new GTIDs are logged in for a master server. See [Use with multi-source replication and other multi-master setups](#use-with-multi-source-replication-and-other-multi-master-setups) for details. This variable can also be set on the session level by a user with the SUPER privilege. This is used by [mysqlbinlog](/clients-utilities/mysqlbinlog/) to preserve the domain ID of GTID events.
- <strong>Commandline:</strong> `--gtid-domain-id=`#
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric (32-bit unsigned integer)`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `4294967295`
- <strong>Introduced:</strong> [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/)

---

#### `last_gtid`

- <strong>Description:</strong> Holds the GTID that was assigned to the last transaction, or statement that was logged to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/). If the binary log is disabled, or if no transaction or statement was executed in the session yet, then the value is an empty string.
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Read-only
- <strong>Data Type:</strong> `string`
- <strong>Introduced:</strong> [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/)

---

#### `server_id`

- <strong>Description:</strong> Server_id can be set on the session level to change which server_id value is logged in binlog events (both GTID and other events). This is used by mysqlbinlog to preserve the server ID of GTID events.
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric (32-bit unsigned integer)

---

#### `gtid_seq_no`

- <strong>Description:</strong> gtid_seq_no can be set on the session level to change which sequence number is logged in the following GTID event. This is used by [mysqlbinlog](/clients-utilities/mysqlbinlog/) to preserve the sequence number of GTID events.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric (64-bit unsigned integer)`
- <strong>Default:</strong> Null
- <strong>Introduced:</strong> [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/)

---

#### `gtid_ignore_duplicates`

- <strong>Description:</strong> When set, different master connections in multi-source replication are allowed to receive and process event groups with the same GTID (when using GTID mode). Only one will be applied, any others will be ignored. Within a given replication domain, just the sequence number will be used to decide whether a given GTID has been already applied; this means it is the responsibility of the user to ensure that GTID sequence numbers are strictly increasing. With gtid_ignore_duplicates=OFF, a duplicate event based on domain id and sequence number, will be executed.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--gtid-ignore-duplicates=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.0.10](/kb/en/mariadb-10010-release-notes/)

---

#### `gtid_pos_auto_engines`

This variable is used to enable multiple versions of the [mysql.gtid_slave_pos](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgtid_slave_pos-table/) table, one for each transactional storage engine in use. This can improve replication performance if a server is using multiple different storage engines in different transactions.

The value is a list of engine names, separated by commas (','). Replication
of transactions using these engines will automatically create new versions
of the mysql.gtid_slave_pos table in the same engine and use that for future
transactions (table creation takes place in a background thread). This avoids introducing a cross-engine transaction to update the GTID position. Only transactional storage engines are supported for
gtid_pos_auto_engines (this currently means [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/), [TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb/), or [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/)).

The variable can be changed dynamically, but slave SQL threads should be stopped when changing it, and it will take effect when the slaves are running again.

When setting the variable on the command line or in a configuration file, it is
possible to specify engines that are not enabled in the server. The server will then still start if, for example, that engine is no longer used. Attempting to set a non-enabled engine dynamically in a running server (with SET GLOBAL gtid_pos_auto_engines) will still result in an error.

Removing a storage engine from the variable will have no effect once the new tables have been created - as long as these tables are detected, they will be used.

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--gtid-pos-auto-engines=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `string` (comma-separated list of engine names)
- <strong>Default:</strong> empty
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)

---

#### `gtid_cleanup_batch_size`

- <strong>Description:</strong> Normally does not need tuning. How many old rows must accumulate in the [mysql.gtid_slave_pos table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgtid_slave_pos-table/) before a background job will be run to delete them. Can be increased to reduce number of commits if using many different engines with [gtid_pos_auto_engines](#gtid_pos_auto_engines), or to reduce CPU overhead if using a huge number of different [gtid_domain_ids](#gtid_domain_id). Can be decreased to reduce number of old rows in the table.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--gtid-cleanup-batch-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default:</strong> `64`
- <strong>Range:</strong> `0` to `2147483647`
- <strong>Introduced:</strong> [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/)

---