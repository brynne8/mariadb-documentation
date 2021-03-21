# Performance Schema host_cache Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `host_cache` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables/), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

The `host_cache` table contains host and IP information from the host_cache, used for avoiding DNS lookups for new client connections.

The host_cache table contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>IP</code></td><td>Client IP address.</td></tr>
<tr><td><code>HOST</code></td><td>IP's resolved DNS host name, or <code>NULL</code> if unknown.</td></tr>
<tr><td><code>HOST_VALIDATED</code></td><td>YES if the IP-to-host DNS lookup was successful, and the <code>HOST</code> column can be used to avoid DNS calls, or NO if unsuccessful, in which case DNS lookup is performed for each connect until either successful or a permanent error.</td></tr>
<tr><td><code>SUM_CONNECT_ERRORS</code></td><td>Number of connection errors. Counts only protocol handshake errors for hosts that passed validation. These errors count towards <a href="/kb/en/server-system-variables/#max_connect_errors">max_connect_errors</a>.</td></tr>
<tr><td><code>COUNT_HOST_BLOCKED_ERRORS</code></td><td>Number of blocked connections because <code>SUM_CONNECT_ERRORS</code> exceeded the <a href="/kb/en/server-system-variables/#max_connect_errors">max_connect_errors</a> system variable.</td></tr>
<tr><td><code>COUNT_NAMEINFO_TRANSIENT_ERRORS</code></td><td>Number of transient errors during IP-to-host DNS lookups.</td></tr>
<tr><td><code>COUNT_NAMEINFO_PERMANENT_ERRORS</code></td><td>Number of permanent errors during IP-to-host DNS lookups.</td></tr>
<tr><td><code>COUNT_FORMAT_ERRORS</code></td><td>Number of host name format errors, for example a numeric host column.</td></tr>
<tr><td><code>COUNT_ADDRINFO_TRANSIENT_ERRORS</code></td><td>Number of transient errors during host-to-IP reverse DNS lookups.</td></tr>
<tr><td><code>COUNT_ADDRINFO_PERMANENT_ERRORS</code></td><td>Number of permanent errors during host-to-IP reverse DNS lookups.</td></tr>
<tr><td><code>COUNT_FCRDNS_ERRORS</code></td><td>Number of forward-confirmed reverse DNS errors, which occur when IP-to-host DNS lookup does not match the originating IP address.</td></tr>
<tr><td><code>COUNT_HOST_ACL_ERRORS</code></td><td>Number of errors occurring because no user from the host is permitted to log in. These attempts return <a href="/kb/en/mariadb-error-codes/">error code</a> <code>1130 ER_HOST_NOT_PRIVILEGED</code> and do not proceed to username and password authentication.</td></tr>
<tr><td><code>COUNT_NO_AUTH_PLUGIN_ERRORS</code></td><td>Number of errors due to requesting an authentication plugin that was not available. This can be due to the plugin never having been loaded, or the load attempt failing.</td></tr>
<tr><td><code>COUNT_AUTH_PLUGIN_ERRORS</code></td><td>Number of errors reported by an authentication plugin. Plugins can increment <code>COUNT_AUTHENTICATION_ERRORS</code> or <code>COUNT_HANDSHAKE_ERRORS</code> instead, but, if specified or the error is unknown, this column is incremented.</td></tr>
<tr><td><code>COUNT_HANDSHAKE_ERRORS</code></td><td>Number of errors detected at the wire protocol level.</td></tr>
<tr><td><code>COUNT_PROXY_USER_ERRORS</code></td><td>Number of errors detected when a proxy user is proxied to a user that does not exist.</td></tr>
<tr><td><code>COUNT_PROXY_USER_ACL_ERRORS</code></td><td>Number of errors detected when a proxy user is proxied to a user that exists, but the proxy user doesn't have the PROXY privilege.</td></tr>
<tr><td><code>COUNT_AUTHENTICATION_ERRORS</code></td><td>Number of errors where authentication failed.</td></tr>
<tr><td><code>COUNT_SSL_ERRORS</code></td><td>Number of errors due to TLS problems.</td></tr>
<tr><td><code>COUNT_MAX_USER_CONNECTIONS_ERRORS</code></td><td>Number of errors due to the per-user quota being exceeded.</td></tr>
<tr><td><code>COUNT_MAX_USER_CONNECTIONS_PER_HOUR_ERRORS</code></td><td>Number of errors due to the per-hour quota being exceeded.</td></tr>
<tr><td><code>COUNT_DEFAULT_DATABASE_ERRORS</code></td><td>Number of errors due to the user not having permission to access the specified default database, or it not existing.</td></tr>
<tr><td><code>COUNT_INIT_CONNECT_ERRORS</code></td><td>Number of errors due to statements in the <a href="/kb/en/server-system-variables/#init_connect">init_connect</a> system variable.</td></tr>
<tr><td><code>COUNT_LOCAL_ERRORS</code></td><td>Number of local server errors, such as out-of-memory errors, unrelated to network, authentication, or authorization.</td></tr>
<tr><td><code>COUNT_UNKNOWN_ERRORS</code></td><td>Number of unknown errors that cannot be allocated to another column.</td></tr>
<tr><td><code>FIRST_SEEN</code></td><td>Timestamp of the first connection attempt by the IP.</td></tr>
<tr><td><code>LAST_SEEN</code></td><td>Timestamp of the most recent connection attempt by the IP.</td></tr>
<tr><td><code>FIRST_ERROR_SEEN</code></td><td>Timestamp of the first error seen from the IP.</td></tr>
<tr><td><code>LAST_ERROR_SEEN</code></td><td>Timestamp of the most recent error seen from the IP.</td></tr>
</tbody></table>

The `host_cache` table, along with the `host_cache`, is cleared with [FLUSH HOSTS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/), [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table/) `host_cache` or by setting the [host_cache_size](/kb/en/server-system-variables/#host_cache_size) system variable at runtime.