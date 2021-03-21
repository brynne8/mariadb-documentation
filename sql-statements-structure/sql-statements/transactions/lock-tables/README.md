# LOCK TABLES

## Syntax

```sql
LOCK TABLE[S]
    tbl_name [[AS] alias] lock_type
    [, tbl_name [[AS] alias] lock_type] ...
    [WAIT n|NOWAIT]

lock_type:
    READ [LOCAL]
  | [LOW_PRIORITY] WRITE
  | WRITE CONCURRENT

UNLOCK TABLES
```

## Description

The <em>lock_type</em> can be one of:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td>READ</td><td>Read lock, no writes allowed</td></tr>
<tr><td>READ LOCAL</td><td>Read lock, but allow <a href="/kb/en/concurrent-inserts/">concurrent inserts</a></td></tr>
<tr><td>WRITE</td><td>Exclusive write lock. No other connections can read or write to this table</td></tr>
<tr><td>LOW_PRIORITY WRITE</td><td>Exclusive write lock, but allow new read locks on the table until we get the write lock.</td></tr>
<tr><td>WRITE CONCURRENT</td><td>Exclusive write lock, but allow READ LOCAL locks to the table.</td></tr>
</tbody></table>

MariaDB enables client sessions to acquire table locks explicitly for the
purpose of cooperating with other sessions for access to tables, or to
prevent other sessions from modifying tables during periods when a
session requires exclusive access to them. A session can acquire or
release locks only for itself. One session cannot acquire locks for
another session or release locks held by another session.

Locks may be used to emulate transactions or to get more speed when
updating tables.

`LOCK TABLES` explicitly acquires table locks for the current client session.
Table locks can be acquired for base tables or views. To use `LOCK TABLES`,
you must have the `LOCK TABLES` privilege, and the `SELECT` privilege for
each object to be locked. See [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/)

For view locking, `LOCK TABLES` adds all base tables used in the view to the
set of tables to be locked and locks them automatically. If you lock a table
explicitly with `LOCK TABLES`, any tables used in triggers are also locked
implicitly, as described in [Triggers and Implicit Locks](/programming-customizing-mariadb/triggers-events/triggers/triggers-and-implicit-locks/).

[UNLOCK TABLES](/sql-statements-structure/sql-statements/transactions/transactions-unlock-tables/) explicitly releases any table locks held by the
current session.
<br><br>

##### MariaDB starting with [10.3.0](/kb/en/mariadb-1030-release-notes/)

### WAIT/NOWAIT

Set the lock wait timeout. See [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait/).

## Limitations

LOCK TABLES [doesn't work when using Galera cluster](/replication/galera-cluster/mariadb-galera-cluster-known-limitations/).   You may experience crashes or locks when used with Galera.

LOCK TABLES works on XtraDB/InnoDB tables only if the [innodb_table_locks](/kb/en/xtradbinnodb-server-system-variables/#innodb_table_locks) system variable is set to 1 (the default) and [autocommit](/kb/en/server-system-variables/#autocommit) is set to 0 (1 is default). Please note that no error message will be returned on LOCK TABLES with innodb_table_locks = 0.

`LOCK TABLES` [implicitly commits](/sql-statements-structure/sql-statements/transactions/sql-statements-that-cause-an-implicit-commit/) the active transaction, if any. Also, starting a transaction always releases all table locks acquired with LOCK TABLES. This means that there is no way to have table locks and an active transaction at the same time. The only exceptions are the transactions in [autocommit](/kb/en/start-transaction/#autocommit) mode. To preserve the data integrity between transactional and non-transactional tables, the [GET_LOCK()](/built-in-functions/secondary-functions/miscellaneous-functions/get_lock/) function can be used.

While a connection holds an explicit read lock on a table, it cannot modify it. If you try, the following error will be produced:

```sql
ERROR 1099 (HY000): Table 'tab_name' was locked with a READ lock and can't be updated
```

While a connection holds an explicit lock on a table, it cannot access a non-locked table. If you try, the following error will be produced:

```sql
ERROR 1100 (HY000): Table 'tab_name' was not locked with LOCK TABLES
```

While a connection holds an explicit lock on a table, it cannot issue the following: INSERT DELAYED, CREATE TABLE, CREATE TABLE ... LIKE, and DDL statements involving stored programs and views (except for triggers). If you try, the following error will be produced:

```sql
ERROR 1192 (HY000): Can't execute the given command because you have active locked tables or an active transaction
```

`LOCK TABLES` can not be used in stored routines - if you try, the following error will be produced on creation:

```sql
ERROR 1314 (0A000): LOCK is not allowed in stored procedures
```

## See Also

- [UNLOCK TABLES](/sql-statements-structure/sql-statements/transactions/transactions-unlock-tables/)