# SQL statements That Cause an Implicit Commit

Some SQL statements cause an implicit commit. As a rule of thumb, such statements are DDL statements. The same statements (except for [SHUTDOWN](/sql-statements-structure/sql-statements/administrative-sql-statements/shutdown/)) produce a 1400 error ([SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate/) 'XAE09') if a XA transaction is in effect.

Here is the list:

```sql
ALTER DATABASE ... UPGRADE DATA DIRECTORY NAME
ALTER EVENT
ALTER FUNCTION
ALTER PROCEDURE
ALTER SERVER
ALTER TABLE
ALTER VIEW
ANALYZE TABLE
BEGIN
CACHE INDEX
CHANGE MASTER TO
CHECK TABLE
CREATE DATABASE
CREATE EVENT
CREATE FUNCTION
CREATE INDEX
CREATE PROCEDURE
CREATE ROLE
CREATE SERVER
CREATE TABLE
CREATE TRIGGER
CREATE USER
CREATE VIEW
DROP DATABASE
DROP EVENT
DROP FUNCTION
DROP INDEX
DROP PROCEDURE
DROP ROLE
DROP SERVER
DROP TABLE
DROP TRIGGER
DROP USER
DROP VIEW
FLUSH
GRANT
LOAD INDEX INTO CACHE
LOCK TABLES
OPTIMIZE TABLE
RENAME TABLE
RENAME USER
REPAIR TABLE
RESET
REVOKE
SET PASSWORD
SHUTDOWN
START SLAVE
START TRANSACTION
STOP SLAVE
TRUNCATE TABLE
```

`SET autocommit = 1` causes an implicit commit if the value was 0.

All these statements cause an implicit commit before execution. This means that, even if the statement fails with an error, the transaction is committed. Some of them, like `CREATE TABLE ... SELECT`, also cause a commit immediatly after execution. Such statements couldn't be rollbacked in any case.

If you are not sure whether a statement has implicitly committed the current transaction, you can query the [in_transaction](/kb/en/server-system-variables/#in_transaction) server system variable.

Note that when a transaction starts (not in autocommit mode), all locks acquired with [LOCK TABLES](/kb/en/lock-tables-and-unlock-tables/) are released. And acquiring such locks always commits the current transaction. To preserve the data integrity between transactional and non-transactional tables, the [GET_LOCK()](/built-in-functions/secondary-functions/miscellaneous-functions/get_lock/) function can be used.

## Exceptions

These statements do not cause an implicit commit in the following cases:

- `CREATE TABLE` and `DROP TABLE`, when the `TEMPORARY` keyword is used.
<ul start="1"><li>However, TRUNCATE TABLE causes an implicit commit even when used on a temporary table.
</li></ul>
- `CREATE FUNCTION` and `DROP FUNCTION`, when used to create a UDF (instead of a stored function). However, `CREATE INDEX` and `DROP INDEX` cause commits even when used with temporary tables.
- `UNLOCK TABLES` causes a commit only if a `LOCK TABLES` was used on non-transactional tables.
- `START SLAVE`, `STOP SLAVE`, `RESET SLAVE` and `CHANGE MASTER TO` only cause implicit commit since [MariaDB 10.0](/kb/en/what-is-mariadb-100/).