# Performance Schema hosts Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `hosts` table was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/) (along with many other new
[Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables)).

## Description

The `hosts` table contains a row for each host used by clients to connect to the server, containing current and total connections.

The size is determined by the [performance_schema_hosts_size](/kb/en/performance-schema-system-variables/#performance_schema_hosts_size) system variable, which, if set to zero, will disable connection statistics in the hosts table.

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>HOST</code></td><td>Host name used by the client to connect, <code>NULL</code> for internal threads or user sessions that failed to authenticate.</td></tr>
<tr><td><code>CURRENT_CONNECTIONS</code></td><td>Current number of the host's connections.</td></tr>
<tr><td><code>TOTAL_CONNECTIONS</code></td><td>Total number of the host's connections</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM hosts;
+-----------+---------------------+-------------------+
| HOST      | CURRENT_CONNECTIONS | TOTAL_CONNECTIONS |
+-----------+---------------------+-------------------+
| localhost |                   1 |                45 |
| NULL      |                  20 |                23 |
+-----------+---------------------+-------------------+
```