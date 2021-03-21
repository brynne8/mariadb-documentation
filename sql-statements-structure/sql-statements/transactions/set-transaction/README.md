# SET TRANSACTION

## Syntax

```sql
SET [GLOBAL | SESSION] TRANSACTION
    transaction_property [, transaction_property] ...

transaction_property:
    ISOLATION LEVEL level
  | READ WRITE
  | READ ONLY

level:
     REPEATABLE READ
   | READ COMMITTED
   | READ UNCOMMITTED
   | SERIALIZABLE
```

## Description

This statement sets the transaction isolation level or the transaction access mode globally, for the current session, or for the next transaction:

- With the <code class="highlight fixed" style="white-space:pre-wrap">GLOBAL</code> keyword, the statement sets the default
  transaction level globally for all subsequent sessions. Existing sessions are
  unaffected.
- With the <code class="highlight fixed" style="white-space:pre-wrap">SESSION</code> keyword, the statement sets the default
  transaction level for all subsequent transactions performed within the
  current session.
- Without any <code class="highlight fixed" style="white-space:pre-wrap">SESSION</code> or <code class="highlight fixed" style="white-space:pre-wrap">GLOBAL</code> keyword,
  the statement sets the isolation level for the next (not started) transaction
  performed within the current session.

A change to the global default isolation level requires the 
<code class="highlight fixed" style="white-space:pre-wrap">[SUPER](/sql-statements-structure/sql-statements/account-management-sql-commands/grant)</code> privilege. Any session is free to change its
session isolation level (even in the middle of a transaction), or the isolation
level for its next transaction.

### Isolation Level

To set the global default isolation level at server startup, use the
<a undefined>--transaction-isolation=level</a> option on the command line or in an option file. Values of level for this option use dashes
rather than spaces, so the allowable values are <code class="highlight fixed" style="white-space:pre-wrap">READ-UNCOMMITTED</code>,
<code class="highlight fixed" style="white-space:pre-wrap">READ-COMMITTED</code>, <code class="highlight fixed" style="white-space:pre-wrap">REPEATABLE-READ</code>, or
<code class="highlight fixed" style="white-space:pre-wrap">SERIALIZABLE</code>. For example, to set the default isolation
level to <code class="highlight fixed" style="white-space:pre-wrap">REPEATABLE READ</code>, use these lines in the `[mysqld]`
section of an option file:

```sql
[mysqld]
transaction-isolation = REPEATABLE-READ```

To determine the global and session transaction isolation levels at
runtime, check the value of the <a undefined>tx_isolation</a> system variable:

```sql
SELECT @@GLOBAL.tx_isolation, @@tx_isolation;
```

InnoDB supports each of the translation isolation levels described here
using different locking strategies. The default level is 
<code class="highlight fixed" style="white-space:pre-wrap">REPEATABLE READ</code>. For additional information about InnoDB
record-level locks and how it uses them to execute various types of statements,
see [XtraDB/InnoDB Lock Modes](/kb/en/xtradbinnodb-lock-modes/),
and [http://dev.mysql.com/doc/refman/en/innodb-locks-set.html](http://dev.mysql.com/doc/refman/en/innodb-locks-set.html).

### Isolation Levels

The following sections describe how MariaDB supports the different transaction levels.

#### READ UNCOMMITTED

<code class="highlight fixed" style="white-space:pre-wrap">SELECT</code> statements are performed in a non-locking fashion,
but a possible earlier version of a row might be used. Thus, using this
isolation level, such reads are not consistent. This is also called a "dirty
read." Otherwise, this isolation level works like 
<code class="highlight fixed" style="white-space:pre-wrap">READ COMMITTED</code>.

#### READ COMMITTED

A somewhat Oracle-like isolation level with respect to consistent
(non-locking) reads: Each consistent read, even within the same
transaction, sets and reads its own fresh snapshot. See
[http://dev.mysql.com/doc/refman/en/innodb-consistent-read.html](http://dev.mysql.com/doc/refman/en/innodb-consistent-read.html).

For locking reads (<code class="highlight fixed" style="white-space:pre-wrap">SELECT</code> with <code class="highlight fixed" style="white-space:pre-wrap">FOR UPDATE</code>
or <code class="highlight fixed" style="white-space:pre-wrap">LOCK IN SHARE MODE</code>), InnoDB locks only index records, not
the gaps before them, and thus allows the free insertion of new records next to
locked records. For <code class="highlight fixed" style="white-space:pre-wrap">UPDATE</code> and <code class="highlight fixed" style="white-space:pre-wrap">DELETE</code>
statements, locking depends on whether the statement uses a unique index with a
unique search condition (such as <code class="highlight fixed" style="white-space:pre-wrap">WHERE id = 100</code>), or a
range-type search condition (such as <code class="highlight fixed" style="white-space:pre-wrap">WHERE id &gt; 100</code>). For a
unique index with a unique search condition, InnoDB locks only the index record
found, not the gap before it. For range-type searches, InnoDB locks the index
range scanned, using gap locks or next-key (gap plus index-record) locks to
block insertions by other sessions into the gaps covered by the range. This is
necessary because "phantom rows" must be blocked for MySQL replication and
recovery to work.

<strong>Note:</strong> If the <code class="highlight fixed" style="white-space:pre-wrap">READ COMMITTED</code> isolation
level is used or the [innodb_locks_unsafe_for_binlog](/kb/en/innodb-system-variables/#innodb_locks_unsafe_for_binlog) system variable is enabled,
there is no InnoDB gap locking except for [foreign-key](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys) constraint checking and
duplicate-key checking. Also, record locks for non-matching rows are released
after MariaDB has evaluated the <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> condition.If you use <code class="highlight fixed" style="white-space:pre-wrap">READ COMMITTED</code> or enable innodb_locks_unsafe_for_binlog, you must use row-based binary logging.

#### REPEATABLE READ

This is the default isolation level for InnoDB. For consistent reads,
there is an important difference from the <code class="highlight fixed" style="white-space:pre-wrap">READ COMMITTED</code>
isolation level: All consistent reads within the same transaction read the
snapshot established by the first read. This convention means that if you issue
several plain (non-locking) <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code> statements within the
same transaction, these <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code> statements are consistent
also with respect to each other. See
[http://dev.mysql.com/doc/refman/en/innodb-consistent-read.html](http://dev.mysql.com/doc/refman/en/innodb-consistent-read.html).

For locking reads (SELECT with FOR UPDATE or LOCK IN SHARE MODE),
UPDATE, and DELETE statements, locking depends on whether the
statement uses a unique index with a unique search condition, or a
range-type search condition. For a unique index with a unique search
condition, InnoDB locks only the index record found, not the gap
before it. For other search conditions, InnoDB locks the index range
scanned, using gap locks or next-key (gap plus index-record) locks to
block insertions by other sessions into the gaps covered by the range.

This is the minimum isolation level for non-distributed [XA transactions](/sql-statements-structure/sql-statements/transactions/xa-transactions).

#### SERIALIZABLE

This level is like REPEATABLE READ, but InnoDB implicitly converts all
plain SELECT statements to <a undefined>SELECT ... LOCK IN SHARE MODE</a> if <a undefined>autocommit</a>
is disabled. If autocommit is enabled, the SELECT is its own
transaction. It therefore is known to be read only and can be
serialized if performed as a consistent (non-locking) read and need
not block for other transactions. (This means that to force a plain
SELECT to block if other transactions have modified the selected rows,
you should disable autocommit.)

Distributed [XA transactions](/sql-statements-structure/sql-statements/transactions/xa-transactions) should always use this isolation level.

### Access Mode

These clauses appeared in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

The access mode specifies whether the transaction is allowed to write data or not. By default, transactions are in `READ WRITE` mode (see the <a undefined>tx_read_only</a> system variable). `READ ONLY` mode allows the storage engine to apply optimizations that cannot be used for transactions which write data. The only exception to this rule is that read only transactions can perform DDL statements on temporary tables.

It is not permitted to specify both `READ WRITE` and `READ ONLY` in the same statement.

`READ WRITE` and `READ ONLY` can also be specified in the [START TRANSACTION](/sql-statements-structure/sql-statements/transactions/start-transaction) statement, in which case the specified mode is only valid for one transaction.

## Examples

```sql
SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

Attempting to set the isolation level within an existing transaction without specifying `GLOBAL` or `SESSION`.

```sql
START TRANSACTION;

SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
ERROR 1568 (25001): Transaction characteristics can't be changed while a transaction is in progress
```