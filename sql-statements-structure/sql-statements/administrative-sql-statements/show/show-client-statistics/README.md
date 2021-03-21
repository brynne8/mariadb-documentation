# SHOW CLIENT_STATISTICS

##### MariaDB starting with [5.2](/kb/en/what-is-mariadb-52/)

[MariaDB 5.2](/kb/en/what-is-mariadb-52/) introduced the [User Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics/) feature.

## Syntax

```sql
SHOW CLIENT_STATISTICS
```

## Description

The `SHOW CLIENT_STATISTICS` statement was introduced in [MariaDB 5.2](/kb/en/what-is-mariadb-52/) as part of the [User Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics/) feature. It was removed as a separate statement in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), but effectively replaced by the generic <a undefined>SHOW information_schema_table</a> statement. The <a undefined>information_schema.CLIENT_STATISTICS</a> table holds statistics about client connections.

The <a undefined>userstat</a> system variable must be set to 1 to activate this feature. See the [User Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics/) and <a undefined>information_schema.CLIENT_STATISTICS</a> articles for more information.

## Example

From [MariaDB 10.0](/kb/en/what-is-mariadb-100/):

```sql
SHOW CLIENT_STATISTICS\G
*************************** 1. row ***************************
                Client: localhost
     Total_connections: 35
Concurrent_connections: 0
        Connected_time: 708
             Busy_time: 2.5557979999999985
              Cpu_time: 0.04123740000000002
        Bytes_received: 3883
            Bytes_sent: 21595
  Binlog_bytes_written: 0
             Rows_read: 18
             Rows_sent: 115
          Rows_deleted: 0
         Rows_inserted: 0
          Rows_updated: 0
       Select_commands: 70
       Update_commands: 0
        Other_commands: 0
   Commit_transactions: 1
 Rollback_transactions: 0
    Denied_connections: 0
      Lost_connections: 0
         Access_denied: 0
         Empty_queries: 35
```