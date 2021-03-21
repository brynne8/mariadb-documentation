# SET SQL_LOG_BIN

## Syntax

```sql
SET [SESSION] sql_log_bin = {0|1}
```

## Description

Sets the [sql_log_bin](/kb/en/replication-and-binary-log-system-variables/#sql_log_bin) system variable, which disables or enables [binary logging](/mariadb-administration/server-monitoring-logs/binary-log/) for the current connection, if the client has the <code class="highlight fixed" style="white-space:pre-wrap">SUPER</code> [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/). The statement is refused with an
error if the client does not have that privilege.

Before [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and before MySQL 5.6 one could also set `sql_log_bin` as a global variable. This has now been disabled as this was too dangerous as it could damage replication.