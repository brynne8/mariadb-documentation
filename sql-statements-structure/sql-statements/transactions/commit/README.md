# COMMIT

The `COMMIT`  statement ends a transaction, saving any changes to the data so that they become visible to subsequent transactions. Also, [unlocks metadata](/sql-statements-structure/sql-statements/transactions/metadata-locking/) changed by current transaction. If [autocommit](/kb/en/server-system-variables/#autocommit) is set to 1, an implicit commit is performed after each statement. Otherwise, all transactions which don't end with an explicit `COMMIT` are implicitly rollbacked and the changes are lost. The [ROLLBACK](/sql-statements-structure/sql-statements/transactions/rollback/) statement can be used to do this explicitly.

The required syntax for the `COMMIT`  statement is as follows:

```sql
COMMIT [WORK] [AND [NO] CHAIN] [[NO] RELEASE]
```

`COMMIT` is the more important transaction terminator, as well as the more interesting one. The basic form of the `COMMIT` statement is simply the keyword `COMMIT`  (the keyword `WORK` is simply noise and can be omitted without changing the effect).

The optional `AND CHAIN` clause is a convenience for initiating a new transaction as soon as the old transaction terminates. If `AND CHAIN` is specified, then there is effectively nothing between the old and new transactions, although they remain separate. The characteristics of the new transaction will be the same as the characteristics of the old one — that is, the new transaction will have the same access mode, isolation level and diagnostics area size (we'll discuss all of these shortly) as the transaction just terminated.

`RELEASE` tells the server to disconnect the client immediately after the current transaction.

There are `NO RELEASE` and `AND NO CHAIN` options. By default, commits do not `RELEASE` or `CHAIN`, but it's possible to change this default behavior with the [completion_type](/kb/en/server-system-variables/#completion_type) server system variable. In this case, the `AND NO CHAIN` and `NO RELEASE` options override the server default.

## See Also

- [autocommit](/kb/en/server-system-variables/#autocommit) - server system variable that determines whether statements are automatically committed.
- [completion_type](/kb/en/server-system-variables/#completion_type) - server system variable that determines whether COMMIT's are standard, COMMIT AND CHAIN or COMMIT RELEASE.
- [SQL statements that cause an implicit commit](/sql-statements-structure/sql-statements/transactions/sql-statements-that-cause-an-implicit-commit/)