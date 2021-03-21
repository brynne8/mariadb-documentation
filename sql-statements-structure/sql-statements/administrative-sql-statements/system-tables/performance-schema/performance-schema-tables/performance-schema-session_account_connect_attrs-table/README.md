# Performance Schema session_account_connect_attrs Table

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

The `session_account_connect_attrs:` table was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/) (along with many other new
[Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables/)).

## Description

The `session_account_connect_attrs` table shows connection attributes for the current session.

In [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/), applications can pass key/value connection attributes to the server when a connection is made. The [session_connect_attrs](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-session_connect_attrs-table/) and `session_account_connect_attrs` tables provide access to this information, for all sessions and the current session respectively.

The C API functions [mysql_options()](/kb/en/mysql_options/) and [mysql_optionsv()](/kb/en/mysql_optionsv/) are used for passing connection attributes to the server.

`session_account_connect_attrs` contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>PROCESSLIST_ID</code></td><td>Session connection identifier.</td></tr>
<tr><td><code>ATTR_NAME</code></td><td>Attribute name.</td></tr>
<tr><td><code>ATTR_VALUE</code></td><td>Attribute value.</td></tr>
<tr><td><code>ORDINAL_POSITION</code></td><td>Order in which attribute was added to the connection attributes.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM performance_schema.session_account_connect_attrs;
+----------------+-----------------+------------------+------------------+
| PROCESSLIST_ID | ATTR_NAME       | ATTR_VALUE       | ORDINAL_POSITION |
+----------------+-----------------+------------------+------------------+
|             45 | _os             | debian-linux-gnu |                0 |
|             45 | _client_name    | libmysql         |                1 |
|             45 | _pid            | 7711             |                2 |
|             45 | _client_version | 10.0.5           |                3 |
|             45 | _platform       | x86_64           |                4 |
|             45 | program_name    | mysql            |                5 |
+----------------+-----------------+------------------+------------------+
```