# IS_USED_LOCK

## Syntax

```sql
IS_USED_LOCK(str)
```

## Description

Checks whether the lock named `str` is in use (that is, locked). If so,
it returns the connection identifier of the client that holds the
lock. Otherwise, it returns <code class="highlight fixed" style="white-space:pre-wrap">NULL</code>. <code class="highlight fixed" style="white-space:pre-wrap">str</code> is case insensitive.

If the [metadata_lock_info](/kb/en/metadata_lock_info/) plugin is installed, the [Information Schema](/kb/en/information_schema/) [metadata_lock_info](/kb/en/information-schema-metadata_lock_info-table/) table contains information about locks of this kind (as well as [metadata locks](/sql-statements-structure/sql-statements/transactions/metadata-locking)).

Statements using the `IS_USED_LOCK` function are [not safe for statement-based replication](/replication/standard-replication/unsafe-statements-for-statement-based-replication).

## See Also

- [GET_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/get_lock)
- [RELEASE_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/release_lock)
- [IS_FREE_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/is_free_lock)
- [RELEASE_ALL_LOCKS](/built-in-functions/secondary-functions/miscellaneous-functions/release_all_locks)