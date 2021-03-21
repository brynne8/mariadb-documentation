# Performance Schema socket_instances Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `socket_instances` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

The `socket_instances` table lists active server connections, with each record being a Unix socket file or TCP/IP connection.

The `socket_instances` table contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>EVENT_NAME</code></td><td><code>NAME</code> from the <code>setup_instruments</code> table, and the name of the <code>wait/io/socket/*</code> instrument that produced the event.</td></tr>
<tr><td><code>OBJECT_INSTANCE_BEGIN</code></td><td>Memory address of the object.</td></tr>
<tr><td><code>THREAD_ID</code></td><td>Thread identifier that the server assigns to each socket.</td></tr>
<tr><td><code>SOCKET_ID</code></td><td>The socket's internal file handle.</td></tr>
<tr><td><code>IP</code></td><td>Client IP address. Blank for Unix socket file, otherwise an IPv4 or IPv6 address. Together with the PORT identifies the connection.</td></tr>
<tr><td><code>PORT</code></td><td>TCP/IP port number, from 0 to 65535. Together with the IP identifies the connection.</td></tr>
<tr><td><code>STATE</code></td><td>Socket status, either <code>IDLE</code> if waiting to receive a request from a client, or <code>ACTIVE</code></td></tr>
</tbody></table>