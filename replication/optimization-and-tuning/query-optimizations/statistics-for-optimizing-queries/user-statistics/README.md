# User Statistics

The User Statistics feature was first released in [MariaDB 5.2.0](/kb/en/mariadb-520-release-notes/), and moved to the `userstat` plugin in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/).

The `userstat` plugin creates the [USER_STATISTICS](/kb/en/information-schema-user_statistics-table/), [CLIENT_STATISTICS](/kb/en/information-schema-client_statistics-table/), the [INDEX_STATISTICS](/kb/en/information-schema-index_statistics-table/), and the [TABLE_STATISTICS](/kb/en/information-schema-table_statistics-table/) tables in the [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database. As an alternative to these tables, the plugin also adds the [SHOW USER_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-user-statistics/), the [SHOW CLIENT_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-client-statistics/), the [SHOW INDEX_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-index-statistics/), and the [SHOW TABLE_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-statistics/) statements.

These tables and commands can be used to understand the server activity better and to identify the sources of your database's load.

The plugin also adds the [FLUSH USER_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/), [FLUSH CLIENT_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/), [FLUSH INDEX_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/), and [FLUSH TABLE_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) statements.

The MariaDB implementation of this plugin is based on the [userstatv2 patch](http://www.percona.com/docs/wiki/patches:userstatv2) from Percona and Ourdelta. The original code comes from Google (Mark Callaghan's team) with additional work from Percona, Ourdelta, and Weldon Whipple. The MariaDB implementation provides the same functionality as the userstatv2 patch but a lot of changes have been made to make it faster and to better fit the MariaDB infrastructure.

## How it Works

The `userstat` plugin works by keeping several hash tables in memory.  All variables are incremented while the query is running. At the end of each statement the global values are updated.

## Enabling the Plugin

By default statistics are not collected. This is to ensure that statistics collection does not cause any extra load on the server unless desired.

Set the [userstat=ON](#userstat) system variable in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) to enable the plugin. For example:

```sql
[mariadb]
...
userstat = 1
```

The value can also be changed dynamically. For example:

```sql
SET GLOBAL userstat=1;
```

## Using the Plugin

### Using the Information Schema Table

The `userstat` plugin creates the [USER_STATISTICS](/kb/en/information-schema-user_statistics-table/), [CLIENT_STATISTICS](/kb/en/information-schema-client_statistics-table/), the [INDEX_STATISTICS](/kb/en/information-schema-index_statistics-table/), and the [TABLE_STATISTICS](/kb/en/information-schema-table_statistics-table/) tables in the [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database.

```sql
SELECT * FROM INFORMATION_SCHEMA.USER_STATISTICS\G
*************************** 1. row ***************************
                  USER: root
     TOTAL_CONNECTIONS: 1
CONCURRENT_CONNECTIONS: 0
        CONNECTED_TIME: 297
             BUSY_TIME: 0.001725
              CPU_TIME: 0.001982
        BYTES_RECEIVED: 388
            BYTES_SENT: 2327
  BINLOG_BYTES_WRITTEN: 0
             ROWS_READ: 0
             ROWS_SENT: 12
          ROWS_DELETED: 0
         ROWS_INSERTED: 13
          ROWS_UPDATED: 0
       SELECT_COMMANDS: 4
       UPDATE_COMMANDS: 0
        OTHER_COMMANDS: 3
   COMMIT_TRANSACTIONS: 0
 ROLLBACK_TRANSACTIONS: 0
    DENIED_CONNECTIONS: 0
      LOST_CONNECTIONS: 0
         ACCESS_DENIED: 0
         EMPTY_QUERIES: 1
```

```sql
SELECT * FROM INFORMATION_SCHEMA.CLIENT_STATISTICS\G
*************************** 1. row ***************************
                CLIENT: localhost
     TOTAL_CONNECTIONS: 3
CONCURRENT_CONNECTIONS: 0
        CONNECTED_TIME: 4883
             BUSY_TIME: 0.009722
              CPU_TIME: 0.0102131
        BYTES_RECEIVED: 841
            BYTES_SENT: 13897
  BINLOG_BYTES_WRITTEN: 0
             ROWS_READ: 0
             ROWS_SENT: 214
          ROWS_DELETED: 0
         ROWS_INSERTED: 207
          ROWS_UPDATED: 0
       SELECT_COMMANDS: 10
       UPDATE_COMMANDS: 0
        OTHER_COMMANDS: 13
   COMMIT_TRANSACTIONS: 0
 ROLLBACK_TRANSACTIONS: 0
    DENIED_CONNECTIONS: 0
      LOST_CONNECTIONS: 0
         ACCESS_DENIED: 0
         EMPTY_QUERIES: 1
1 row in set (0.00 sec)
```

```sql
SELECT * FROM INFORMATION_SCHEMA.INDEX_STATISTICS WHERE TABLE_NAME = "author";
+--------------+------------+------------+-----------+
| TABLE_SCHEMA | TABLE_NAME | INDEX_NAME | ROWS_READ |
+--------------+------------+------------+-----------+
| books        | author     | by_name    |        15 |
+--------------+------------+------------+-----------+
```

```sql
SELECT * FROM INFORMATION_SCHEMA.TABLE_STATISTICS WHERE TABLE_NAME='user';
+--------------+------------+-----------+--------------+------------------------+
| TABLE_SCHEMA | TABLE_NAME | ROWS_READ | ROWS_CHANGED | ROWS_CHANGED_X_INDEXES |
+--------------+------------+-----------+--------------+------------------------+
| mysql        | user       |         5 |            2 |                      2 |
+--------------+------------+-----------+--------------+------------------------+
```

### Using the SHOW Statements

As an alternative to the [INFORMATION_SCHEMA](/kb/en/information_schema/) tables, the `userstat` plugin also adds the [SHOW USER_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-user-statistics/), the [SHOW CLIENT_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-client-statistics/), the [SHOW INDEX_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-index-statistics/), and the [SHOW TABLE_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-statistics/) statements.

These commands are another way to display the information stored in the information schema tables. WHERE clauses are accepted. LIKE clauses are accepted but ignored.

```sql
SHOW USER_STATISTICS
SHOW CLIENT_STATISTICS
SHOW INDEX_STATISTICS
SHOW TABLE_STATISTICS
```

### Flushing Plugin Data

The `userstat` plugin also adds the [FLUSH USER_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/), [FLUSH CLIENT_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/), [FLUSH INDEX_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/), and [FLUSH TABLE_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) statements, which discard the information stored in the specified information schema table.

```sql
FLUSH USER_STATISTICS
FLUSH CLIENT_STATISTICS
FLUSH INDEX_STATISTICS
FLUSH TABLE_STATISTICS
```

## Versions

### USER_STATISTICS

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>2.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10118-release-notes/">MariaDB 10.1.18</a></td></tr>
<tr><td>2.0</td><td>Gamma</td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
</tbody></table>

### CLIENT_STATISTICS

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>2.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a></td></tr>
<tr><td>2.0</td><td>Gamma</td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
</tbody></table>

### INDEX_STATISTICS

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>2.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a></td></tr>
<tr><td>2.0</td><td>Gamma</td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
</tbody></table>

### TABLE_STATISTICS

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>2.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10118-release-notes/">MariaDB 10.1.18</a></td></tr>
<tr><td>2.0</td><td>Gamma</td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
</tbody></table>

## System Variables

### `userstat`

- <strong>Description:</strong> If set to `1`, [user statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics/) will be activated.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--userstat=1</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---