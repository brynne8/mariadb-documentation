# GET_LOCK

## Syntax

```sql
GET_LOCK(str,timeout)
```

## Description

Tries to obtain a lock with a name given by the string `str`, using a timeout of `timeout` seconds. Returns `1` if the lock was obtained successfully, `0` if the attempt timed out (for example, because another client has previously locked the name), or  `NULL` if an error occurred (such as running out of memory or the thread was killed with [mysqladmin](/clients-utilities/mysqladmin/) kill).

A lock is released with [RELEASE_LOCK()](/built-in-functions/secondary-functions/miscellaneous-functions/release_lock/), when the connection terminates (either normally or abnormally), or before [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/), when the connection executes another `GET_LOCK` statement. From [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/), a connection can hold multiple locks at the same time, so a lock that is no longer needed needs to be explicitly released.

The [IS_FREE_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/is_free_lock/) function returns whether a specified lock a free or not, and the [IS_USED_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/is_used_lock/) whether the function is in use or not.

Locks obtained with `GET_LOCK()` do not interact with transactions. That is, committing a transaction does not release any such locks obtained during the transaction.

From [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/), it is also possible to recursively set the same lock. If a lock with the same name is set `n` times, it needs to be released `n` times as well.

`str` is case insensitive for `GET_LOCK()` and related functions. If `str` is an empty string or `NULL`, `GET_LOCK()` returns `NULL` and does nothing. From [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), `timeout` supports microseconds. Before then, it was rounded to the closest integer.

If the [metadata_lock_info](/kb/en/metadata-lock-info/) plugin is installed, locks acquired with this function are visible in the [Information Schema](/kb/en/information_schema/) [METADATA_LOCK_INFO](/kb/en/information-schema-metadata_lock_info-table/) table.

This function can be used to implement application locks or to simulate record locks. Names are locked on a server-wide basis. If a name has been locked by one client, `GET_LOCK()` blocks any request by another client for a lock with the same name. This allows clients that agree on a given lock name to use the name to perform cooperative advisory locking. But be aware that it also allows a client that is not among the set of cooperating clients to lock a name, either inadvertently or deliberately, and thus prevent any of the cooperating clients from locking that name. One way to reduce the likelihood of this is to use lock names that are database-specific or application-specific. For example, use lock names of the form `db_name.str` or `app_name.str`.

Statements using the `GET_LOCK` function are [not safe for statement-based replication](/replication/standard-replication/unsafe-statements-for-statement-based-replication/).

The patch to permit multiple locks was [contributed by Konstantin "Kostja" Osipov](http://kostja-osipov.livejournal.com/46410.html) ([MDEV-3917](https://jira.mariadb.org/browse/MDEV-3917)).

## Examples

```sql
SELECT GET_LOCK('lock1',10);
+----------------------+
| GET_LOCK('lock1',10) |
+----------------------+
|                    1 |
+----------------------+

SELECT IS_FREE_LOCK('lock1'), IS_USED_LOCK('lock1');
+-----------------------+-----------------------+
| IS_FREE_LOCK('lock1') | IS_USED_LOCK('lock1') |
+-----------------------+-----------------------+
|                     0 |                    46 |
+-----------------------+-----------------------+

SELECT IS_FREE_LOCK('lock2'), IS_USED_LOCK('lock2');
+-----------------------+-----------------------+
| IS_FREE_LOCK('lock2') | IS_USED_LOCK('lock2') |
+-----------------------+-----------------------+
|                     1 |                  NULL |
+-----------------------+-----------------------+
```

From [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/), multiple locks can be held:

```sql
SELECT GET_LOCK('lock2',10);
+----------------------+
| GET_LOCK('lock2',10) |
+----------------------+
|                    1 |
+----------------------+

SELECT IS_FREE_LOCK('lock1'), IS_FREE_LOCK('lock2');
+-----------------------+-----------------------+
| IS_FREE_LOCK('lock1') | IS_FREE_LOCK('lock2') |
+-----------------------+-----------------------+
|                     0 |                     0 |
+-----------------------+-----------------------+

SELECT RELEASE_LOCK('lock1'), RELEASE_LOCK('lock2');
+-----------------------+-----------------------+
| RELEASE_LOCK('lock1') | RELEASE_LOCK('lock2') |
+-----------------------+-----------------------+
|                     1 |                     1 |
+-----------------------+-----------------------+
```

Before [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/), a connection could only hold a single lock:

```sql
SELECT GET_LOCK('lock2',10);
+----------------------+
| GET_LOCK('lock2',10) |
+----------------------+
|                    1 |
+----------------------+

SELECT IS_FREE_LOCK('lock1'), IS_FREE_LOCK('lock2');
+-----------------------+-----------------------+
| IS_FREE_LOCK('lock1') | IS_FREE_LOCK('lock2') |
+-----------------------+-----------------------+
|                     1 |                     0 |
+-----------------------+-----------------------+

SELECT RELEASE_LOCK('lock1'), RELEASE_LOCK('lock2');
+-----------------------+-----------------------+
| RELEASE_LOCK('lock1') | RELEASE_LOCK('lock2') |
+-----------------------+-----------------------+
|                  NULL |                     1 |
+-----------------------+-----------------------+
```

From [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/), it is possible to hold the same lock recursively. This example is viewed using the [metadata_lock_info](/kb/en/metadata-lock-info/) plugin:

```sql
SELECT GET_LOCK('lock3',10);
+----------------------+
| GET_LOCK('lock3',10) |
+----------------------+
|                    1 |
+----------------------+

SELECT GET_LOCK('lock3',10);
+----------------------+
| GET_LOCK('lock3',10) |
+----------------------+
|                    1 |
+----------------------+

SELECT * FROM INFORMATION_SCHEMA.METADATA_LOCK_INFO;
+-----------+---------------------+---------------+-----------+--------------+------------+
| THREAD_ID | LOCK_MODE           | LOCK_DURATION | LOCK_TYPE | TABLE_SCHEMA | TABLE_NAME |
+-----------+---------------------+---------------+-----------+--------------+------------+
|        46 | MDL_SHARED_NO_WRITE | NULL          | User lock | lock3        |            |
+-----------+---------------------+---------------+-----------+--------------+------------+

SELECT RELEASE_LOCK('lock3');
+-----------------------+
| RELEASE_LOCK('lock3') |
+-----------------------+
|                     1 |
+-----------------------+

SELECT * FROM INFORMATION_SCHEMA.METADATA_LOCK_INFO;
+-----------+---------------------+---------------+-----------+--------------+------------+
| THREAD_ID | LOCK_MODE           | LOCK_DURATION | LOCK_TYPE | TABLE_SCHEMA | TABLE_NAME |
+-----------+---------------------+---------------+-----------+--------------+------------+
|        46 | MDL_SHARED_NO_WRITE | NULL          | User lock | lock3        |            |
+-----------+---------------------+---------------+-----------+--------------+------------+

SELECT RELEASE_LOCK('lock3');
+-----------------------+
| RELEASE_LOCK('lock3') |
+-----------------------+
|                     1 |
+-----------------------+

SELECT * FROM INFORMATION_SCHEMA.METADATA_LOCK_INFO;
Empty set (0.000 sec)
```

Timeout example: Connection 1:

```sql
SELECT GET_LOCK('lock4',10);
+----------------------+
| GET_LOCK('lock4',10) |
+----------------------+
|                    1 |
+----------------------+
```

Connection 2:

```sql
SELECT GET_LOCK('lock4',10);
```

After 10 seconds...

```sql
+----------------------+
| GET_LOCK('lock4',10) |
+----------------------+
|                    0 |
+----------------------+
```

Deadlocks are automatically detected and resolved. Connection 1:

```sql
SELECT GET_LOCK('lock5',10); 
+----------------------+
| GET_LOCK('lock5',10) |
+----------------------+
|                    1 |
+----------------------+
```

Connection 2:

```sql
SELECT GET_LOCK('lock6',10);
+----------------------+
| GET_LOCK('lock6',10) |
+----------------------+
|                    1 |
+----------------------+
```

Connection 1:

```sql
SELECT GET_LOCK('lock6',10); 
+----------------------+
| GET_LOCK('lock6',10) |
+----------------------+
|                    0 |
+----------------------+
```

Connection 2:

```sql
SELECT GET_LOCK('lock5',10);
ERROR 1213 (40001): Deadlock found when trying to get lock; try restarting transaction
```

## See Also

- [RELEASE_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/release_lock/)
- [IS_FREE_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/is_free_lock/)
- [IS_USED_LOCK](/built-in-functions/secondary-functions/miscellaneous-functions/is_used_lock/)
- [RELEASE_ALL_LOCKS](/built-in-functions/secondary-functions/miscellaneous-functions/release_all_locks/)