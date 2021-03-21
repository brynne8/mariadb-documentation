# Metadata Locking

MariaDB supports metadata locking. This means that when a transaction (including [XA transactions](/sql-statements-structure/sql-statements/transactions/xa-transactions)) uses a table, it locks its metadata until the end of transaction. Non-transactional tables are also locked, as well as views and objects which are related to locked tables/views (stored functions, triggers, etc). When a connection tries to use a DDL statement (like an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table)) which modifies a table that is locked, that connection is queued, and has to wait until it's unlocked. Using savepoints and performing a partial rollback does not release metadata locks.

[LOCK TABLES ... WRITE](/kb/en/transactions-lock/) are also queued. Some wrong statements which produce an error may not need to wait for the lock to be freed.

The metadata lock's timeout is determined by the value of the [lock_wait_timeout](/kb/en/server-system-variables/#lock_wait_timeout) server system variable (in seconds). However, note that its default value is 31536000 (1 year, MariaDB &lt;= 10.2.3), or 86400 (1 day, MariaDB &gt;= 10.2.4). If this timeout is exceeded, the following error is returned:

```sql
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
```

If the [metadata_lock_info](/kb/en/metadata-lock-info/) plugin is installed, the [Information Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema) [metadata_lock_info](/kb/en/information-schema-metadata_lock_info-table/) table stores information about existing metadata locks.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5](/kb/en/what-is-mariadb-105/), the [Performance Schema metadata_locks](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-metadata_locks-table) table contains metadata lock information.

## Example

Let's use the following MEMORY (non-transactional) table:

```sql
CREATE TABLE t (a INT) ENGINE = MEMORY;
```

Connection 1 starts a transaction, and INSERTs a row into t:

```sql
START TRANSACTION;

INSERT INTO t SET a=1;
```

`t`'s metadata is now locked by connection 1. Connection 2 tries to alter `t`, but has to wait:

```sql
ALTER TABLE t ADD COLUMN b INT;
```

Connection 2's prompt is blocked now.

Now connection 1 ends the transaction:

```sql
COMMIT;
```

...and connection 2 finally gets the output of its command:

```sql
Query OK, 1 row affected (35.23 sec)
Records: 1  Duplicates: 0  Warnings: 0
```