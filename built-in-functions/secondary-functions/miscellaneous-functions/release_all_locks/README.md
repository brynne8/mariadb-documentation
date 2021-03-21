# RELEASE_ALL_LOCKS

##### MariaDB until [10.5.2](/kb/en/mariadb-1052-release-notes/)

RELEASE_ALL_LOCKS was added in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

## Syntax

```sql
RELEASE_ALL_LOCKS()
```

## Description

Releases all named locks held by the current session. Returns the number of locks released, or `0` if none were held.

Statements using the `RELEASE_ALL_LOCKS` function are [not safe for statement-based replication](/replication/standard-replication/unsafe-statements-for-statement-based-replication/).

## Examples

```sql
SELECT RELEASE_ALL_LOCKS();
+---------------------+
| RELEASE_ALL_LOCKS() | 
+---------------------+
|                   0 |
+---------------------+

SELECT GET_LOCK('lock1',10);
+----------------------+
| GET_LOCK('lock1',10) |
+----------------------+
|                    1 |
+----------------------+

SELECT RELEASE_ALL_LOCKS();
+---------------------+
| RELEASE_ALL_LOCKS() | 
+---------------------+
|                   1 |
+---------------------+
```

## See Also

- [GET_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/get_lock/)
- [IS_FREE_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/is_free_lock/)
- [IS_USED_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/is_used_lock/)
- [RELEASE_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/release_lock/)