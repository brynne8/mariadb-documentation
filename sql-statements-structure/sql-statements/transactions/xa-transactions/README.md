# XA Transactions

## Overview

The MariaDB XA implementation is based on the X/Open CAE document Distributed Transaction Processing: The XA Specification. This document is published by The Open Group and available at [http://www.opengroup.org/public/pubs/catalog/c193.htm](http://www.opengroup.org/public/pubs/catalog/c193.htm).

XA transactions are designed to allow distributed transactions, where a transaction manager (the application) controls a transaction which involves multiple resources. Such resources are usually DBMSs, but could be resources of any type. The whole set of required transactional operations is called a global transaction. Each subset of operations which involve a single resource is called a local transaction. XA used a 2-phases commit (2PC). With the first commit, the transaction manager tells each resource to prepare an effective commit, and waits for a confirm message. The changes are not still made effective at this point. If any of the resources encountered an error, the transaction manager will rollback the global transaction. If all resources communicate that the first commit is successful, the transaction manager can require a second commit, which makes the changes effective.

In MariaDB, XA transactions can only be used with storage engines that support them. At least [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/), [TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb/), [SPIDER](/columns-storage-engines-and-plugins/storage-engines/spider/) and [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/) support them. For InnoDB, XA transactions can be disabled by setting the [innodb_support_xa](/kb/en/xtradbinnodb-server-system-variables/#innodb_support_xa) server system variable to 0.

Like regular transactions, XA transactions create [metadata locks](/sql-statements-structure/sql-statements/transactions/metadata-locking/) on accessed tables.

XA transactions require [REPEATABLE READ](/kb/en/set-transaction/#repeatable-read) as a minimum isolation level. However, distributed transactions should always use [SERIALIZABLE](/kb/en/set-transaction/#serializable).

Trying to start more than one XA transaction at the same time produces a 1400 error ([SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate/) 'XAE09'). The same error is produced when attempting to start an XA transaction while a regular transaction is in effect. Trying to start a regular transaction while an XA transaction is in effect produces a 1399 error ([SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate/) 'XAE07').

The [statements that cause an implicit COMMIT](/sql-statements-structure/sql-statements/transactions/sql-statements-that-cause-an-implicit-commit/) for regular transactions produce a 1400 error ([SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate/) 'XAE09') if a XA transaction is in effect.

## Internal XA vs External XA

XA transactions are an overloaded term in MariaDB. If a [storage engine](/columns-storage-engines-and-plugins/storage-engines/) is XA-capable, it can mean one or both of these:

- It supports MariaDB's internal two-phase commit API. This is transparent to the user. Sometimes this is called "internal XA", since MariaDB's internal [transaction coordinator log](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log/) can handle coordinating these transactions.

- It supports XA transactions, with the `XA START`, `XA PREPARE`, `XA COMMIT`, etc. statements. Sometimes this is called "external XA", since it requires the use of an external transaction coordinator to use this feature properly.

## Transaction Coordinator Log

If you have two or more XA-capable storage engines enabled, then a transaction coordinator log must be available.

There are currently two implementations of the transaction coordinator log:

- Binary log-based transaction coordinator log
- Memory-mapped file-based transaction coordinator log

If the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is enabled on a server, then the server will use the binary log-based transaction coordinator log. Otherwise, it will use the memory-mapped file-based transaction coordinator log.

See [Transaction Coordinator Log](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log/) for more information.

## Syntax

```sql
XA {START|BEGIN} xid [JOIN|RESUME]

XA END xid [SUSPEND [FOR MIGRATE]]

XA PREPARE xid

XA COMMIT xid [ONE PHASE]

XA ROLLBACK xid

XA RECOVER [FORMAT=['RAW'|'SQL']]

xid: gtrid [, bqual [, formatID ]]
```

The interface to XA transactions is a set of SQL statements starting with `XA`. Each statement changes a transaction's state, determining which actions it can perform. A transaction which does not exist is in the `NON-EXISTING` state.

`XA START` (or `BEGIN`) starts a transaction and defines its `xid` (a transaction identifier). The `JOIN` or `RESUME` keywords have no effect. The new transaction will be in `ACTIVE` state.

The `xid` can have 3 components, though only the first one is mandatory. `gtrid` is a quoted string representing a global transaction identifier. `bqual` is a quoted string representing a local transaction identifier. `formatID` is an unsigned integer indicating the format used for the first two components; if not specified, defaults to 1. MariaDB does not interpret in any way these components, and only uses them to identify a transaction. `xid`s of transactions in effect must be unique.

`XA END` declares that the specified `ACTIVE` transaction is finished and it changes its state to `IDLE`. `SUSPEND [FOR MIGRATE]` has no effect.

`XA PREPARE` prepares an `IDLE` transaction for commit, changing its state to `PREPARED`. This is the first commit.

`XA COMMIT` definitely commits and terminates a transaction which has already been `PREPARED`. If the `ONE PHASE` clause is specified, this statements performs a 1-phase commit on an `IDLE` transaction.

`XA ROLLBACK` rolls back and terminates an `IDLE` or `PREPARED` transaction.

`XA RECOVER` shows information about all `PREPARED` transactions.

When trying to execute an operation which is not allowed for the transaction's current state, an error is produced:

```sql
XA COMMIT 'test' ONE PHASE;
ERROR 1399 (XAE07): XAER_RMFAIL: The command cannot be executed when global transaction is in the  ACTIVE state

XA COMMIT 'test2';
ERROR 1399 (XAE07): XAER_RMFAIL: The command cannot be executed when global transaction is in the  NON-EXISTING state
```

## `XA RECOVER`

The `XA RECOVER` statement shows information about all transactions which are in the `PREPARED` state. It does not matter which connection created the transaction: if it has been `PREPARED`, it appears. But this does not mean that a connection can commit or rollback a transaction which was started by another connection. Note that transactions using a 1-phase commit are never in the `PREPARED` state, so they cannot be shown by `XA RECOVER`.

`XA RECOVER` produces four columns:

```sql
XA RECOVER;
+----------+--------------+--------------+------+
| formatID | gtrid_length | bqual_length | data |
+----------+--------------+--------------+------+
|        1 |            4 |            0 | test |
+----------+--------------+--------------+------+
```

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

You can use `XA RECOVER FORMAT='SQL'` to get the data in a human readable
form that can be directly copy-pasted into `XA COMMIT` or `XA ROLLBACK`. This is particularly useful for binary `xid` generated by some transaction coordinators.

`formatID` is the `formatID` part of `xid`.

`data` are the `gtrid` and `bqual` parts of `xid`, concatenated.

`gtrid_length` and `bqual_length` are the lengths of `gtrid` and `bqual`, respectevely.

## Examples

2-phases commit:

```sql
XA START 'test';

INSERT INTO t VALUES (1,2);

XA END 'test';

XA PREPARE 'test';

XA COMMIT 'test';
```

1-phase commit:

```sql
XA START 'test';

INSERT INTO t VALUES (1,2);

XA END 'test';

XA COMMIT 'test' ONE PHASE;
```

Human-readable:

```sql
xa start '12\r34\t67\v78', 'abc\ndef', 3;

insert t1 values (40);

xa end '12\r34\t67\v78', 'abc\ndef', 3;

xa prepare '12\r34\t67\v78', 'abc\ndef', 3;

xa recover format='RAW';
+----------+--------------+--------------+--------------------+
| formatID | gtrid_length | bqual_length | data               |
+----------+--------------+--------------+--------------------+
34      67v78abc       11 |            7 | 12
def |
+----------+--------------+--------------+--------------------+

xa recover format='SQL';
+----------+--------------+--------------+-----------------------------------------------+
| formatID | gtrid_length | bqual_length | data                                          |
+----------+--------------+--------------+-----------------------------------------------+
|        3 |           11 |            7 | X'31320d3334093637763738',X'6162630a646566',3 |
+----------+--------------+--------------+-----------------------------------------------+

xa rollback X'31320d3334093637763738',X'6162630a646566',3;
```

## Known Issues

### MariaDB Galera Cluster

[MariaDB Galera Cluster](/replication/galera-cluster/) does not support XA transactions. See [MDEV-10532](https://jira.mariadb.org/browse/MDEV-10532) for more information on that. The request to implement that feature is being tracked at [MDEV-17099](https://jira.mariadb.org/browse/MDEV-17099).

However, [MariaDB Galera Cluster](/replication/galera-cluster/) builds include a built-in plugin called `wsrep`. Prior to [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/), this plugin was internally considered an [XA-capable](/sql-statements-structure/sql-statements/transactions/xa-transactions/) [storage engine](/columns-storage-engines-and-plugins/storage-engines/). Consequently, these [MariaDB Galera Cluster](/replication/galera-cluster/) builds have multiple XA-capable storage engines by default, even if the only "real" storage engine that supports external [XA transactions](/sql-statements-structure/sql-statements/transactions/xa-transactions/) enabled on these builds by default is [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/). Therefore, when using one these builds MariaDB would be forced to use a [transaction coordinator log](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log/) by default, which could have performance implications.

See [Transaction Coordinator Log Overview: MariaDB Galera Cluster](/kb/en/transaction-coordinator-log-overview/#mariadb-galera-cluster) for more information.