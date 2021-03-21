# SHOW INDEX_STATISTICS

##### MariaDB starting with [5.2](/kb/en/what-is-mariadb-52/)

[MariaDB 5.2](/kb/en/what-is-mariadb-52/) introduced the [User Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics) feature.

## Syntax

```sql
SHOW INDEX_STATISTICS
```

## Description

The `SHOW INDEX_STATISTICS` statement was introduced in [MariaDB 5.2](/kb/en/what-is-mariadb-52/) as part of the [User Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics) feature. It was removed as a separate statement in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), but effectively replaced by the generic <a undefined>SHOW information_schema_table</a> statement. The <a undefined>information_schmea.INDEX_STATISTICS</a> table shows statistics on index usage and makes it possible to do such things as locating unused indexes and generating the commands to remove them.

The <a undefined>userstat</a> system variable must be set to 1 to activate this feature. See the [User Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/user-statistics) and <a undefined>information_schema.INDEX_STATISTICS</a> table for more information.

## Example

From [MariaDB 10.0](/kb/en/what-is-mariadb-100/):

```sql
SHOW INDEX_STATISTICS;
+--------------+-------------------+------------+-----------+
| Table_schema | Table_name        | Index_name | Rows_read |
+--------------+-------------------+------------+-----------+
| test         | employees_example | PRIMARY    |         1 |
+--------------+-------------------+------------+-----------+
```