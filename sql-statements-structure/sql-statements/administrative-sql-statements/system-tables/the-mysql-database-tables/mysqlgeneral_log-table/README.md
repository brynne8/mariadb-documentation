# mysql.general_log Table

The `mysql.general_log` table stores the contents of the [General Query Log](/mariadb-administration/server-monitoring-logs/general-query-log/) if general logging is active and the output is being written to table (see [Writing logs into tables](/mariadb-administration/server-monitoring-logs/writing-logs-into-tables/)).

It contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>event_time</code></td><td><code>timestamp(6)</code></td><td>NO</td><td></td><td><code>CURRENT_TIMESTAMP(6)</code></td><td>Time the query was executed.</td></tr>
<tr><td><code>user_host</code></td><td><code>mediumtext</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>User and host combination.</td></tr>
<tr><td><code>thread_id</code></td><td><code>int(11)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Thread id.</td></tr>
<tr><td><code>server_id</code></td><td><code>int(10) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Server id.</td></tr>
<tr><td><code>command_type</code></td><td><code>varchar(64)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Type of command.</td></tr>
<tr><td><code>argument</code></td><td><code>mediumtext</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Full query.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM mysql.general_log\G
*************************** 1. row ***************************
  event_time: 2014-11-11 08:40:04.117177
   user_host: root[root] @ localhost []
   thread_id: 74
   server_id: 1
command_type: Query
    argument: SELECT * FROM test.s
*************************** 2. row ***************************
  event_time: 2014-11-11 08:40:10.501131
   user_host: root[root] @ localhost []
   thread_id: 74
   server_id: 1
command_type: Query
    argument: SELECT * FROM mysql.general_log
...
```