# RESET

## Syntax

```sql
RESET reset_option [, reset_option] ...
```

## Description

The `RESET` statement is used to clear the state of various server
operations. You must have the [RELOAD privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) to execute
`RESET`.

`RESET` acts as a stronger version of the [FLUSH](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) statement.

The different `RESET` options are:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><a href="/kb/en/reset-slave/">SLAVE ["connection_name"] [ALL</a>]</td><td>Deletes all <a href="/kb/en/relay-log/">relay logs</a> from the slave and reset the replication position in the master <a href="/kb/en/binary-log/">binary log</a>.</td></tr>
<tr><td><a href="/kb/en/reset-master/">MASTER</a></td><td>Deletes all old binary logs, makes the binary index file (<a href="/kb/en/mysqld-options-full-list/">--log-bin-index</a>) empty and creates a new binary log file.  This is useful when you want to reset the master to an initial state. If you want to just delete old, not used binary logs, you should use the <a href="/kb/en/sql-commands-purge-logs/">PURGE BINARY LOGS</a> command.</td></tr>
<tr><td>QUERY CACHE</td><td>Removes all queries from <a href="/kb/en/the-query-cache/">the query cache</a>. See also <a href="/kb/en/flush-query-cache/">FLUSH QUERY CACHE</a>.</td></tr>
</tbody></table>