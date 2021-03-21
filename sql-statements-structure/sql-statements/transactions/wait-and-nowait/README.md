# WAIT and NOWAIT

##### MariaDB starting with [10.3.0](/kb/en/mariadb-1030-release-notes/)

[MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/) introduced extended syntax so that it is possible to set [innodb_lock_wait_timeout](/kb/en/xtradbinnodb-server-system-variables/#innodb_lock_wait_timeout) and [lock_wait_timeout](/kb/en/server-system-variables/#lock_wait_timeout) for the following statements:

## Syntax

```sql
ALTER TABLE tbl_name [WAIT n|NOWAIT] ...
CREATE ... INDEX ON tbl_name (index_col_name, ...) [WAIT n|NOWAIT] ...
DROP INDEX ... [WAIT n|NOWAIT]
DROP TABLE tbl_name [WAIT n|NOWAIT] ...
LOCK TABLE ... [WAIT n|NOWAIT]
OPTIMIZE TABLE tbl_name [WAIT n|NOWAIT]
RENAME TABLE tbl_name [WAIT n|NOWAIT] ...
SELECT ... FOR UPDATE [WAIT n|NOWAIT]
SELECT ... LOCK IN SHARE MODE [WAIT n|NOWAIT]
TRUNCATE TABLE tbl_name [WAIT n|NOWAIT]
```

## Description

The lock wait timeout can be explicitly set in the statement by using either `WAIT n` (to set the wait in seconds) or `NOWAIT`, in which case the statement will immediately fail if the lock cannot be obtained. `WAIT 0` is equivalent to `NOWAIT`.

## See Also

- [Query Limits and Timeouts](/replication/optimization-and-tuning/query-optimizations/query-limits-and-timeouts/)
- [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/)
- [CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index/)
- [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index/)
- [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/)
- [LOCK TABLES and UNLOCK TABLES](/kb/en/lock-tables-and-unlock-tables/)
- [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/)
- [RENAME TABLE](/sql-statements-structure/sql-statements/data-definition/rename-table/)
- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
- [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table/)