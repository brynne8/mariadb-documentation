# Performance Schema users Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `users` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables/), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

## Description

Each user that connects to the server is stored as a row in the `users` table, along with current and total connections.

The table size is determined at startup by the value of the [performance_schema_users_size](/kb/en/performance-schema-system-variables/#performance_schema_users_size) system variable. If this is set to `0`, user statistics will be disabled.

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>USER</code></td><td>The connection's client user name for the connection, or <code>NULL</code> if an internal thread.</td></tr>
<tr><td><code>CURRENT_CONNECTIONS</code></td><td>Current connections for the user.</td></tr>
<tr><td><code>TOTAL_CONNECTIONS</code></td><td>Total connections for the user.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM performance_schema.users;
+------------------+---------------------+-------------------+
| USER             | CURRENT_CONNECTIONS | TOTAL_CONNECTIONS |
+------------------+---------------------+-------------------+
| debian-sys-maint |                   0 |                35 |
| NULL             |                  20 |                23 |
| root             |                   1 |                 2 |
+------------------+---------------------+-------------------+
```