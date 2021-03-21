# MASTER_GTID_WAIT

##### MariaDB starting with [10.0.9](/kb/en/mariadb-1009-release-notes/)

MASTER_GTID_WAIT() was included in [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/).

## Syntax

```sql
MASTER_GTID_WAIT(gtid-list[, timeout)
```

## Description

This function takes a string containing a comma-separated list of [global transaction id's](/kb/en/global-transaction-id/)
(similar to the value of, for example, [gtid_binlog_pos](/kb/en/global-transaction-id/#gtid_binlog_pos)). It waits until the value of [gtid_slave_pos](/kb/en/global-transaction-id/#gtid_slave_pos) has the same or higher seq_no within all replication domains specified in the gtid-list; in other words, it waits until the slave has
reached the specified GTID position.

An optional second argument gives a timeout in seconds. If the timeout
expires before the specified GTID position is reached, then the function
returns -1. Passing NULL or a negative number for the timeout means no timeout, and the function will wait indefinitely.

If the wait completes without a timeout, 0 is returned. Passing NULL for the
 gtid-list makes the function return NULL immediately, without waiting.

The gtid-list may be the empty string, in which case MASTER_GTID_WAIT()
returns immediately. If the gtid-list contains fewer domains than
[gtid_slave_pos](/kb/en/global-transaction-id/#gtid_slave_pos), then only those domains are waited upon. If gtid-list
contains a domain that is not present in @@gtid_slave_pos, then
MASTER_GTID_WAIT() will wait until an event containing such domain_id arrives
on the slave (or until timed out or killed).

MASTER_GTID_WAIT() can be useful to ensure that a slave has caught up to
a master. Simply take the value of [gtid_binlog_pos](/kb/en/global-transaction-id/#gtid_binlog_pos) on the master, and use it in a MASTER_GTID_WAIT() call on the slave; when the call completes, the slave
will have caught up with that master position.

MASTER_GTID_WAIT() can also be used in client applications together with the
[last_gtid](/kb/en/global-transaction-id/#last_gtid) session variable. This is useful in a read-scaleout [replication](/replication) setup, where the application writes to a single master but divides the
reads out to a number of slaves to distribute the load. In such a setup, there
is a risk that an application could first do an update on the master, and then
a bit later do a read on a slave, and if the slave is not fast enough, the
data read from the slave might not include the update just made, possibly
confusing the application and/or the end-user. One way to avoid this is to
request the value of [last_gtid](/kb/en/global-transaction-id/#last_gtid) on the master just after the update. Then
before doing the read on the slave, do a MASTER_GTID_WAIT() on the value
obtained from the master; this will ensure that the read is not performed
until the slave has replicated sufficiently far for the update to have become
visible.

Note that MASTER_GTID_WAIT() can be used even if the slave is configured not
to use GTID for connections ([CHANGE MASTER TO master_use_gtid=no](/kb/en/change-master-to/#master_use_gtid)). This is
because from MariaDB 10, GTIDs are always logged on the master server, and
always recorded on the slave servers.

## Differences to MASTER_POS_WAIT()

- MASTER_GTID_WAIT() is global; it waits for any master connection to reach
   the specified GTID position. [MASTER_POS_WAIT()](/built-in-functions/secondary-functions/miscellaneous-functions/master_pos_wait) works only against a
   specific connection. This also means that while MASTER_POS_WAIT() aborts if
   its master connection is terminated with [STOP SLAVE](/kb/en/stop-slave/) or due to an error,
   MASTER_GTID_WAIT() continues to wait while slaves are stopped.

- MASTER_GTID_WAIT() can take its timeout as a floating-point value, so a
   timeout in fractional seconds is supported, eg. MASTER_GTID_WAIT("0-1-100",
   0.5). (The minimum wait is one microsecond, 0.000001 seconds).

- MASTER_GTID_WAIT() allows one to specify a timeout of zero in order to do a
   non-blocking check to see if the slaves have progressed to a specific GTID position
   (MASTER_POS_WAIT() takes a zero timeout as meaning an infinite wait). To do
   an infinite MASTER_GTID_WAIT(), specify a negative timeout, or omit the
   timeout argument.

- MASTER_GTID_WAIT() does not return the number of events executed since the
   wait started, nor does it return NULL if a slave thread is stopped. It
   always returns either 0 for successful wait completed, or -1 for timeout
   reached (or NULL if the specified gtid-pos is NULL).

Since MASTER_GTID_WAIT() looks only at the seq_no part of the GTIDs, not the
server_id, care is needed if a slave becomes diverged from another server so
that two different GTIDs with the same seq_no (in the same domain) arrive at
the same server. This situation is in any case best avoided; setting
[gtid_strict_mode](/kb/en/global-transaction-id/#gtid_strict_mode) is recommended, as this will prevent any such out-of-order sequence numbers from ever being replicated on a slave.