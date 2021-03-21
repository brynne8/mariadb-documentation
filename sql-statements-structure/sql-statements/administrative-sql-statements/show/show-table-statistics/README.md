# SHOW TABLE_STATISTICS

##### MariaDB starting with [5.2](/kb/en/what-is-mariadb-52/)

[MariaDB 5.2](/kb/en/what-is-mariadb-52/) introduced the [User Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics) feature.

## Syntax

```sql
SHOW TABLE_STATISTICS
```

## Description

The `SHOW TABLE_STATISTICS` statement was introduced in [MariaDB 5.2](/kb/en/what-is-mariadb-52/) as part of the [User Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics) feature. It was removed as a separate statement in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), but effectively replaced by the generic <a undefined>SHOW information_schema_table</a> statement. The <a undefined>information_schema.TABLE_STATISTICS</a> table shows statistics on table usage

The <a undefined>userstat</a> system variable must be set to 1 to activate this feature. See the [User Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics) and <a undefined>information_schema.TABLE_STATISTICS</a> articles for more information.

## Example

From [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

```sql
SHOW TABLE_STATISTICS\G
*************************** 1. row ***************************
           Table_schema: mysql
             Table_name: proxies_priv
              Rows_read: 2
           Rows_changed: 0
Rows_changed_x_#indexes: 0
*************************** 2. row ***************************
           Table_schema: test
             Table_name: employees_example
              Rows_read: 7
           Rows_changed: 0
Rows_changed_x_#indexes: 0
*************************** 3. row ***************************
           Table_schema: mysql
             Table_name: user
              Rows_read: 16
           Rows_changed: 0
Rows_changed_x_#indexes: 0
*************************** 4. row ***************************
           Table_schema: mysql
             Table_name: db
              Rows_read: 2
           Rows_changed: 0
Rows_changed_x_#indexes: 0

```