# Transactions

"An SQL-transaction (transaction) is a sequence of executions of SQL-statements that is atomic with respect to recovery. That is to say: either the execution result is completely successful, or it has no effect on any SQL-schemas or SQL-data."

— The SQL Standard

The [InnoDB](/kb/en/xtradb-and-innodb/) storage engine supports [ACID](/kb/en/acid-concurrency-control-with-transactions/)-compliant transactions.

## Transaction Articles

- [START TRANSACTION](/sql-statements-structure/sql-statements/transactions/start-transaction/) — Basic transaction control statements.
- [COMMIT](/sql-statements-structure/sql-statements/transactions/commit/) — Ends a transaction, making changes visible to subsequent transactions
- [ROLLBACK](/sql-statements-structure/sql-statements/transactions/rollback/) — Cancel current transaction and the changes to data
- [SET TRANSACTION](/sql-statements-structure/sql-statements/transactions/set-transaction/) — Sets the transaction isolation level.
- [LOCK TABLES](/sql-statements-structure/sql-statements/transactions/lock-tables/) — Explicitly lock tables.
- [SAVEPOINT](/sql-statements-structure/sql-statements/transactions/savepoint/) — SAVEPOINT for a ROLLBACK.
- [Metadata Locking](/sql-statements-structure/sql-statements/transactions/metadata-locking/) — A lock which protects each transaction from external DDL statements.
- [SQL statements That Cause an Implicit Commit](/sql-statements-structure/sql-statements/transactions/sql-statements-that-cause-an-implicit-commit/) — List of statements which implicitly commit the current transaction
- [Transaction Timeouts](/sql-statements-structure/sql-statements/transactions/transaction-timeouts/) — Timing out idle transactions
- [UNLOCK TABLES](/sql-statements-structure/sql-statements/transactions/transactions-unlock-tables/) — Explicitly releases any table locks held by the current session.
- [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait/) — Extended syntax so that it is possible to set lock wait timeout for certain statements.
- [XA Transactions](/sql-statements-structure/sql-statements/transactions/xa-transactions/) — Transactions designed to allow distributed transactions.