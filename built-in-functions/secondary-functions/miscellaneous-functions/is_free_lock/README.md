# IS_FREE_LOCK

## Syntax

```sql
IS_FREE_LOCK(str)
```

## Description

Checks whether the lock named `str` is free to use (that is, not locked).
Returns <code class="highlight fixed" style="white-space:pre-wrap">1</code> if the lock is free (no one is using the lock),
 <code class="highlight fixed" style="white-space:pre-wrap">0</code> if the lock is in use, and <code class="highlight fixed" style="white-space:pre-wrap">NULL</code> if an
error occurs (such as an incorrect argument, like an empty string or <code class="highlight fixed" style="white-space:pre-wrap">NULL</code>). <code class="highlight fixed" style="white-space:pre-wrap">str</code> is case insensitive.

If the [metadata_lock_info](/kb/en/metadata_lock_info/) plugin is installed, the [Information Schema](/kb/en/information_schema/) [metadata_lock_info](/kb/en/information-schema-metadata_lock_info-table/) table contains information about locks of this kind (as well as [metadata locks](/sql-statements-structure/sql-statements/transactions/metadata-locking/)).

Statements using the `IS_FREE_LOCK` function are [not safe for statement-based replication](/replication/standard-replication/unsafe-statements-for-statement-based-replication/).

## See Also

- [GET_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/get_lock/)
- [RELEASE_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/release_lock/)
- [IS_USED_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/is_used_lock/)
- [RELEASE_ALL_LOCKS](/built-in-functions/secondary-functions/miscellaneous-functions/release_all_locks/)