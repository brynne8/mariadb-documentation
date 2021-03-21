# Delayed Replication

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

Delayed replication was introduced in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

When using older versions, a 3rd party tool called [pt-slave-delay](https://www.percona.com/doc/percona-toolkit/LATEST/pt-slave-delay.html) can be used. It is part of the Percona Toolkit. Note that pt-slave-delay does not support MariaDB multi-channel replication syntax.

Delayed replication allows specifying that a replication slave should lag
behind the master by (at least) a specified amount of time (specified in seconds). Before executing
an event, the slave will first wait, if necessary, until the given time has
passed since the event was created on the master. The result is that the
slave will reflect the state of the master some time back in the past.

The default is zero, or no delay, and the maximum value is 2147483647, or about 68 years.

Delayed replication is enabled using the MASTER_DELAY option to [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to):

```sql
  CHANGE MASTER TO master_delay=3600;
```

A zero delay disables delayed
replication. The slave must be stopped when changing the delay value.

Three fields in [SHOW SLAVE STATUS](/kb/en/show-slave-status/) are associated with delayed replication:

1 SQL_Delay: This is the value specified by MASTER_DELAY in CHANGE MASTER
(or 0 if none).
2 SQL_Remaining_Delay. When the slave is delaying the execution of an event
due to MASTER_DELAY, this is the number of seconds of delay remaining before
the event will be applied. Otherwise, the value is NULL.
3 Slave_SQL_Running_State. This shows the state of the SQL driver threads,
same as in [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist). When the slave is delaying the execution of an
event due to MASTER_DELAY, this fields displays: "Waiting until MASTER_DELAY
seconds after master executed event".