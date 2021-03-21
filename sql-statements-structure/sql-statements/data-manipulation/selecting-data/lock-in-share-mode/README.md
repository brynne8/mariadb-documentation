# LOCK IN SHARE MODE

When `LOCK IN SHARE MODE` is specified in a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statement, MariaDB will wait until all transactions that have modified the rows are committed. Then, a write lock is acquired. All transactions can read the rows, but if they want to modify them, they have to wait until your transaction is committed.

InnoDB/XtraDB supports row-level locking. selected rows can be locked using `LOCK IN SHARE MODE` or `FOR UPDATE`. In both cases, a lock is acquired on the rows read by the query, and it will be released when the current transaction is committed.

If `autocommit` is set to 1, the LOCK IN SHARE MODE and [FOR UPDATE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/for-update/) clauses have no effect.

## See Also

- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
- [FOR UPDATE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/for-update/)
- [XtraDB/InnoDB Lock Modes](/kb/en/xtradbinnodb-lock-modes/)