# UNLOCK TABLES

## Syntax

```sql
UNLOCK TABLES
```

## Description

`UNLOCK TABLES` explicitly releases any table locks held by the
current session. See [LOCK TABLES](/sql-statements-structure/sql-statements/transactions/lock-tables) for more information.

In addition to releasing table locks acquired by the [LOCK TABLES](/sql-statements-structure/sql-statements/transactions/lock-tables) statement, the `UNLOCK TABLES` statement also releases the global read lock acquired by the `FLUSH TABLES WITH READ LOCK` statement.  The `FLUSH TABLES WITH READ LOCK` statement is very useful for performing backups. See [FLUSH](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) for more information about `FLUSH TABLES WITH READ LOCK`.