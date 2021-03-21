# SHOW USER_STATISTICS

##### MariaDB starting with [5.2](/kb/en/what-is-mariadb-52/)

[MariaDB 5.2](/kb/en/what-is-mariadb-52/) introduced the [User Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics/) feature.

## Syntax

```sql
SHOW USER_STATISTICS
```

## Description

The `SHOW USER_STATISTICS` statement was introduced in [MariaDB 5.2](/kb/en/what-is-mariadb-52/) as part of the [User Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics/) feature. It was removed as a separate statement in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), but effectively replaced by the generic <a undefined>SHOW information_schema_table</a> statement. The <a undefined>information_schema.USER_STATISTICS</a> table holds statistics about user activity. You can use this table to find out such things as which user is causing the most load and which users are being abusive. You can also use this table to measure how close to capacity the server may be.

The <a undefined>userstat</a> system variable must be set to 1 to activate this feature. See the [User Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics/) and <a undefined>information_schema.USER_STATISTICS</a> table for more information.

## Example

From [MariaDB 10.0](/kb/en/what-is-mariadb-100/):

```sql
SHOW USER_STATISTICS\G
*************************** 1. row ***************************
                  User: root
     Total_connections: 1
Concurrent_connections: 0
        Connected_time: 3297
             Busy_time: 0.14113400000000006
              Cpu_time: 0.017637000000000003
        Bytes_received: 969
            Bytes_sent: 22355
  Binlog_bytes_written: 0
             Rows_read: 10
             Rows_sent: 67
          Rows_deleted: 0
         Rows_inserted: 0
          Rows_updated: 0
       Select_commands: 7
       Update_commands: 0
        Other_commands: 0
   Commit_transactions: 1
 Rollback_transactions: 0
    Denied_connections: 0
      Lost_connections: 0
         Access_denied: 0
         Empty_queries: 7
```