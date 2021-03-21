# mysql.slow_log Table

The `mysql.slow_log` table stores the contents of the [Slow Query Log](/mariadb-administration/server-monitoring-logs/slow-query-log/) if slow logging is active and the output is being written to table (see [Writing logs into tables](/mariadb-administration/server-monitoring-logs/writing-logs-into-tables/)).

It contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>start_time</code></td><td><code>timestamp(6)</code></td><td>NO</td><td></td><td><code>CURRENT_TIMESTAMP(6)</code></td><td>Time the query began.</td></tr>
<tr><td><code>user_host</code></td><td><code>mediumtext</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>User and host combination.</td></tr>
<tr><td><code>query_time</code></td><td><code>time(6)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Total time the query took to execute.</td></tr>
<tr><td><code>lock_time</code></td><td><code>time(6)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Total time the query was locked.</td></tr>
<tr><td><code>rows_sent</code></td><td><code>int(11)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Number of rows sent.</td></tr>
<tr><td><code>rows_examined</code></td><td><code>int(11)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Number of rows examined.</td></tr>
<tr><td><code>db</code></td><td><code>varchar(512)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Default database.</td></tr>
<tr><td><code>last_insert_id</code></td><td><code>int(11)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td><a href="/kb/en/last_insert_id/">last_insert_id</a>.</td></tr>
<tr><td><code>insert_id</code></td><td><code>int(11)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Insert id.</td></tr>
<tr><td><code>server_id</code></td><td><code>int(10) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>The server's id.</td></tr>
<tr><td><code>sql_text</code></td><td><code>mediumtext</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Full query.</td></tr>
<tr><td><code>thread_id</code></td><td><code>bigint(21) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Thread id.</td></tr>
<tr><td><code>rows_affected</code></td><td><code>int(11)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Number of rows affected by an <a href="/kb/en/update/">UPDATE</a> or <a href="/kb/en/delete/">DELETE</a> (from <a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a>)</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM mysql.slow_log\G
...
*************************** 2. row ***************************
    start_time: 2014-11-11 07:56:28.721519
     user_host: root[root] @ localhost []
    query_time: 00:00:12.000215
     lock_time: 00:00:00.000000
     rows_sent: 1
 rows_examined: 0
            db: test
last_insert_id: 0
     insert_id: 0
     server_id: 1
      sql_text: SELECT SLEEP(12)
     thread_id: 74
...
```