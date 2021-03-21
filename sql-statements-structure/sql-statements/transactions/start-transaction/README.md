# START TRANSACTION

## Syntax

```sql
START TRANSACTION [transaction_property [, transaction_property] ...] | BEGIN [WORK]
COMMIT [WORK] [AND [NO] CHAIN] [[NO] RELEASE]
ROLLBACK [WORK] [AND [NO] CHAIN] [[NO] RELEASE]
SET autocommit = {0 | 1}

transaction_property:
    WITH CONSISTENT SNAPSHOT
  | READ WRITE
  | READ ONLY
```

## Description

The <code class="highlight fixed" style="white-space:pre-wrap">START TRANSACTION</code> or <code class="highlight fixed" style="white-space:pre-wrap">BEGIN</code> statement
begins a new transaction. [COMMIT](/kb/en/transactions-commit-statement/) commits the current
transaction, making its changes permanent. [ROLLBACK](/kb/en/rollback-statement/) rolls
back the current transaction, canceling its changes. The [SET](/programming-customizing-mariadb/programmatic-compound-statements/set-variable)
[autocommit](/kb/en/server-system-variables/#autocommit) statement disables or enables the default autocommit mode for the current session.

START TRANSACTION and SET autocommit = 1 implicitly commit the current transaction, if any.

The optional <code class="highlight fixed" style="white-space:pre-wrap">WORK</code> keyword is supported for
<code class="highlight fixed" style="white-space:pre-wrap">COMMIT</code> and <code class="highlight fixed" style="white-space:pre-wrap">ROLLBACK</code>, as are the
<code class="highlight fixed" style="white-space:pre-wrap">CHAIN</code> and <code class="highlight fixed" style="white-space:pre-wrap">RELEASE</code> clauses.
<code class="highlight fixed" style="white-space:pre-wrap">CHAIN</code> and <code class="highlight fixed" style="white-space:pre-wrap">RELEASE</code> can be used for
additional control over transaction completion. The value of the
[completion_type](/kb/en/server-system-variables/#completion_type) system variable determines the default completion behavior.

The <code class="highlight fixed" style="white-space:pre-wrap">AND CHAIN</code> clause causes a new transaction to begin as
soon as the current one ends, and the new transaction has the same isolation
level as the just-terminated transaction. The <code class="highlight fixed" style="white-space:pre-wrap">RELEASE</code> clause
causes the server to disconnect the current client session after terminating
the current transaction. Including the <code class="highlight fixed" style="white-space:pre-wrap">NO</code> keyword suppresses
<code class="highlight fixed" style="white-space:pre-wrap">CHAIN</code> or <code class="highlight fixed" style="white-space:pre-wrap">RELEASE</code> completion, which can be
useful if the [completion_type](/kb/en/server-system-variables/#completion_type) system variable is set to cause chaining or release completion by default.

### Access Mode

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

These clauses appeared in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

The access mode specifies whether the transaction is allowed to write data or not. By default, transactions are in `READ WRITE` mode (see the [tx_read_only](/kb/en/server-system-variables/#tx_read_only) system variable). `READ ONLY` mode allows the storage engine to apply optimizations that cannot be used for transactions which write data. The only exception to this rule is that read only transactions can perform DDL statements on temporary tables.

It is not permitted to specify both `READ WRITE` and `READ ONLY` in the same statement.

`READ WRITE` and `READ ONLY` can also be specified in the [SET TRANSACTION](/sql-statements-structure/sql-statements/transactions/set-transaction) statement, in which case the specified mode is valid for all sessions, or for all subsequent transaction used by the current session.

### autocommit

By default, MariaDB runs with [autocommit](/kb/en/server-system-variables/#autocommit) mode enabled. This means that as soon as you execute a statement that updates (modifies) a table, MariaDB stores the update on disk to make it permanent. To disable autocommit mode, use the following statement:

```sql
SET autocommit=0;
```

After disabling autocommit mode by setting the autocommit variable to zero, changes to transaction-safe tables (such as those for InnoDB or
<code class="highlight fixed" style="white-space:pre-wrap">NDBCLUSTER</code>) are not made permanent immediately. You must use <code class="highlight fixed" style="white-space:pre-wrap">COMMIT</code> to store your changes to disk or ROLLBACK to ignore the changes.

To disable autocommit mode for a single series of statements, use the <code class="highlight fixed" style="white-space:pre-wrap">START TRANSACTION</code> statement.

### DDL Statements

DDL statements (CREATE, ALTER, DROP) and administrative statements (`FLUSH`, `RESET`, `OPTIMIZE`, `ANALYZE`, `CHECK`, `REPAIR`, `CACHE INDEX`), and `LOAD DATA INFILE`, cause an implicit `COMMIT` and start a new transaction. An exception to this rule are the DDL that operate on temporary tables: you can `CREATE`, `ALTER` and `DROP` them without causing any `COMMIT`, but those actions cannot be rolled back. This means that if you call `ROLLBACK`, the temporary tables you created in the transaction will remain, while the rest of the transaction will be rolled back.

Transactions cannot be used in Stored Functions or Triggers. In Stored Procedures and Events BEGIN is not allowed, so you should use START TRANSACTION instead.

A transaction acquires a [metadata lock](/sql-statements-structure/sql-statements/transactions/metadata-locking) on every table it accesses to prevent other connections from altering their structure. The lock is released at the end of the transaction. This happens even with non-transactional storage engines (like [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine) or [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect)), so it makes sense to use transactions with non-transactional tables.

### in_transaction

##### MariaDB starting with [5.3](/kb/en/what-is-mariadb-53/)

The [in_transaction](/kb/en/server-system-variables/#in_transaction) system variable appeared in [MariaDB 5.3](/kb/en/what-is-mariadb-53/).

It is a session-only, read-only variable that returns `1` inside a transaction, and `0` if not in a transaction.

### WITH CONSISTENT SNAPSHOT

The `WITH CONSISTENT SNAPSHOT` option starts a consistent read for storage engines such as [XtraDB and InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) that can do so, the same as if a START TRANSACTION followed by a SELECT from any InnoDB table was issued.

##### MariaDB starting with [5.3](/kb/en/what-is-mariadb-53/)

[MariaDB 5.3](/kb/en/what-is-mariadb-53/) introduced enhancements to this feature. See [Enhancements for START TRANSACTION WITH CONSISTENT SNAPSHOT](/replication/standard-replication/enhancements-for-start-transaction-with-consistent-snapshot).

## Examples

```sql
START TRANSACTION;
SELECT @A:=SUM(salary) FROM table1 WHERE type=1;
UPDATE table2 SET summary=@A WHERE type=1;
COMMIT;
```

## See Also

- [Enhancements for START TRANSACTION WITH CONSISTENT SNAPSHOT](/replication/standard-replication/enhancements-for-start-transaction-with-consistent-snapshot)
- [MyRocks and START TRANSACTION WITH CONSISTENT SNAPSHOT](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-and-start-transaction-with-consistent-snapshot)