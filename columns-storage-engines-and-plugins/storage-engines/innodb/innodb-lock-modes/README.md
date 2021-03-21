# InnoDB Lock Modes

Locks are acquired by a transaction to prevent concurrent transactions from modifying, or even reading, some rows or ranges of rows. This is done to make sure that concurrent write operations never collide.

[XtraDB/InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) supports a number of lock modes.

## Shared and Exclusive Locks

The two standard row-level locks are <em>share locks</em>(S) and <em>exclusive locks</em>(X).

A shared lock is obtained to read a row, and allows other transactions to read the locked row, but not to write to the locked row. Other transactions may also acquire their own shared locks.

An exclusive lock is obtained to write to a row, and stops other transactions from locking the same row. It's specific behavior depends on the [isolation level](/kb/en/set-transaction-isolation-level/#isolation-level); the default (REPEATABLE READ), allow other transactions to read from the exclusively locked row.

## Intention Locks

InnoDB also permits table locking, and to allow locking at both table and row level to co-exist gracefully, a series of locks called <em>intention locks</em> exist.

An <em>intention shared lock</em>(IS) indicates that a transaction intends to set a shared lock.

An <em>intention exclusive lock</em>(IX) indicates that a transaction intends to set an exclusive lock.

Whether a lock is granted or not can be summarised as follows:

- An X lock is not granted if any other lock (X, S, IX, IS) is held.
- An S lock is not granted if an X or IX lock is held. It is granted if an S or IS lock is held.
- An IX lock is not granted if in X or S lock is held. It is granted if an IX or IS lock is held.
- An IS lock is not granted if an X lock is held. It is granted if an S, IX or IS lock is held.

## AUTO_INCREMENT Locks

Locks are also required  for auto-increments - see [AUTO_INCREMENT handling in XtraDB/InnoDB](/kb/en/auto_increment-handling-in-xtradbinnodb/).

## Gap Locks

With the default setting of [innodb_locks_unsafe_for_binlog](/kb/en/xtradbinnodb-server-system-variables/#innodb_locks_unsafe_for_binlog) and the default [isolation level](/kb/en/set-transaction-isolation-level/#isolation-level), `REPEATABLE READ`, a method called gap locking is used. When InnoDB sets a shared or exclusive lock on a record, it's actually on the index record. Records will have an internal InnoDB index even if they don't have a unique index defined. At the same time, a lock is held on the gap before the index record, so that another transaction cannot insert a new index record in the gap between the record and the preceding record.

The gap can be a single index value, multiple index values, or not exist at all depending on the contents of the index.

If a statement uses all the columns of a unique index to search for unique row, gap locking is not used.

Similar to the shared and exclusive intention locks described above, there can be a number of types of gap locks. These include the shared gap lock, exclusive gap lock, intention shared gap lock and intention exclusive gap lock.

Gap locks are disabled if the [innodb_locks_unsafe_for_binlog](/kb/en/xtradbinnodb-server-system-variables/#innodb_locks_unsafe_for_binlog) system variable is set, or the [isolation level](/kb/en/set-transaction-isolation-level/#isolation-level) is set to `READ COMMITTED`.