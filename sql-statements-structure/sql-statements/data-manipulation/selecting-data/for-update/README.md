# FOR UPDATE

The `FOR UPDATE` clause of [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) applies only when [autocommit](/kb/en/server-system-variables/#autocommit) is set to 0 or the `SELECT` is enclosed in a transaction. A lock is acquired on the rows, and other transactions are prevented from writing the rows, acquire locks, and from reading them (unless their isolation level is <a undefined>READ UNCOMMITTED</a>).

If `autocommit` is set to 1, the [LOCK IN SHARE MODE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/lock-in-share-mode/) and `FOR UPDATE` clauses have no effect.

If the isolation level is set to [SERIALIZABLE](/kb/en/set-transaction-isolation-level/#serializable), all plain `SELECT` statements are converted to `SELECT ... LOCK IN SHARE MODE`.

## Example

```sql
SELECT * FROM trans WHERE period=2001 FOR UPDATE;
```

## See Also

- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
- [LOCK IN SHARE MODE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/lock-in-share-mode/)
- [XtraDB/InnoDB Lock Modes](/kb/en/xtradbinnodb-lock-modes/)