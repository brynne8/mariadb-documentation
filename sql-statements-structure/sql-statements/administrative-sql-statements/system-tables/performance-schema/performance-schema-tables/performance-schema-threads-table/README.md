# Performance Schema threads Table

Each server thread is represented as a row in the `threads` table.

The `threads` table contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th><th>Added</th></tr>
<tr><td><code>THREAD_ID</code></td><td>A unique thread identifier.</td><td><a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a></td></tr>
<tr><td><code>NAME</code></td><td>Name associated with the server's thread instrumentation code, for example <code>thread/sql/main</code> for the server's <code>main()</code> function, and <code>thread/sql/one_connection</code> for a user connection.</td><td><a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a></td></tr>
<tr><td><code>TYPE</code></td><td><code>FOREGROUND</code> or <code>BACKGROUND</code>, depending on the thread type. User connection threads are <code>FOREGROUND</code>, internal server threads are <code>BACKGROUND</code>.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><code>PROCESSLIST_ID</code></td><td>The <code>PROCESSLIST.ID</code> value for threads displayed in the <code>INFORMATION_SCHEMA.PROCESSLIST</code> table, or <code>0</code> for background threads. Also corresponds with the <code>CONNECTION_ID()</code> return value for the thread.</td><td><a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a></td></tr>
<tr><td><code>PROCESSLIST_USER</code></td><td>Foreground thread user, or <code>NULL</code> for a background thread.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><code>PROCESSLIST_HOST</code></td><td>Foreground thread host, or <code>NULL</code> for a background thread.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><code>PROCESSLIST_DB</code></td><td>Thread's default database, or <code>NULL</code> if none exists.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><code>PROCESSLIST_COMMAND</code></td><td>Type of command executed by the thread. These correspond to the the <code>COM_xxx</code> client/server protocol commands, and the <code>Com_xxx</code> <a href="/kb/en/server-status-variables/">status variables</a>. See <a href="/kb/en/thread-command-values/">Thread Command Values</a>.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><code>PROCESSLIST_TIME</code></td><td>Time in seconds the thread has been in its current state.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><code>PROCESSLIST_STATE</code></td><td>Action, event or state indicating what the thread is doing.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><code>PROCESSLIST_INFO</code></td><td>Statement being executed by the thread, or <code>NULL</code> if a statement is not being executed. If a statement results in calling other statements, such as for a <a href="/kb/en/stored-procedures/">stored procedure</a>, the innermost statement from the stored procedure is shown here.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><code>PARENT_THREAD_ID</code></td><td><code>THREAD_ID</code> of the parent thread, if any. Subthreads can for example be spawned as a result of <a href="/kb/en/insert-delayed/">INSERT DELAYED</a> statements.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><code>ROLE</code></td><td>Unused.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><code>INSTRUMENTED</code></td><td><code>YES</code> or <code>NO</code> for Whether the thread is instrumented or not. For foreground threads, the initial value is determined by whether there's a user/host match in the <a href="/kb/en/performance-schema-setup_actors-table/">setup_actors</a> table. Subthreads are again matched, while for background threads, this will be set to <code>YES</code> by default. To monitor events that the thread executes, <code>INSTRUMENTED</code> must be <code>YES</code> and the thread_instrumentation consumer in the <a href="/kb/en/performance-schema-setup_consumers-table/">setup_consumers</a> table must also be <code>YES</code>.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM performance_schema.threads\G;
*************************** 1. row ***************************
          THREAD_ID: 1
               NAME: thread/sql/main
               TYPE: BACKGROUND
     PROCESSLIST_ID: NULL
   PROCESSLIST_USER: NULL
   PROCESSLIST_HOST: NULL
     PROCESSLIST_DB: NULL
PROCESSLIST_COMMAND: NULL
   PROCESSLIST_TIME: 215859
  PROCESSLIST_STATE: Table lock
   PROCESSLIST_INFO: INTERNAL DDL LOG RECOVER IN PROGRESS
   PARENT_THREAD_ID: NULL
               ROLE: NULL
       INSTRUMENTED: YES
...
*************************** 21. row ***************************
          THREAD_ID: 64
               NAME: thread/sql/one_connection
               TYPE: FOREGROUND
     PROCESSLIST_ID: 44
   PROCESSLIST_USER: root
   PROCESSLIST_HOST: localhost
     PROCESSLIST_DB: NULL
PROCESSLIST_COMMAND: Query
   PROCESSLIST_TIME: 0
  PROCESSLIST_STATE: Sending data
   PROCESSLIST_INFO: SELECT * FROM performance_schema.threads
   PARENT_THREAD_ID: NULL
               ROLE: NULL
       INSTRUMENTED: YES

```