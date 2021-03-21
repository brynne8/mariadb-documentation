# Performance Schema accounts Table

## Description

Each account that connects to the server is stored as a row in the accounts table, along with current and total connections.

The table size is determined at startup by the value of the [performance_schema_accounts_size](/kb/en/performance-schema-system-variables/#performance_schema_accounts_size) system variable. If this is set to 0, account statistics will be disabled.

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>USER</code></td><td>The connection's client user name for the connection, or NULL if an internal thread.</td></tr>
<tr><td><code>HOST</code></td><td>The connection client's host name, or NULL if an internal thread.</td></tr>
<tr><td><code>CURRENT_CONNECTIONS</code></td><td>Current connections for the account.</td></tr>
<tr><td><code>TOTAL_CONNECTIONS</code></td><td>Total connections for the account.</td></tr>
</tbody></table>

The `USER` and `HOST` values shown here are the username and host used for user connections, not the patterns used to check permissions.

## Example

```sql
SELECT * FROM performance_schema.accounts;
+------------------+-----------+---------------------+-------------------+
| USER             | HOST      | CURRENT_CONNECTIONS | TOTAL_CONNECTIONS |
+------------------+-----------+---------------------+-------------------+
| root             | localhost |                   1 |                 2 |
| NULL             | NULL      |                  20 |                23 |
| debian-sys-maint | localhost |                   0 |                35 |
+------------------+-----------+---------------------+-------------------+
```