# mysqladmin

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/),  `mariadb-admin` is a symlink to `mysqladmin`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadb-admin` is the name of the script, with `mysqladmin` a symlink .

`mysqladmin` is an administration program for the mysqld daemon. It can be used to:

- Monitor what the MariaDB clients are doing (processlist)
- Get usage statistics and variables from the MariaDB server
- Create/drop databases
- Flush (reset) logs, statistics and tables
- Kill running queries.
- Stop the server (shutdown)
- Start/stop slaves
- Check if the server is alive (ping)

## Using mysqladmin

The command to use `mysqladmin` and the general syntax is:

```sql
mysqladmin [options] command [command-arg] [command [command-arg]] ...
```

### Options

`mysqladmin` supports the following options:

<table><tbody><tr><th>Option</th><th>Description</th><th>Added</th></tr>
<tr><td><code>--character-sets-dir=name</code></td><td>Directory where the <a href="/kb/en/data-types-character-sets-and-collations/">character set</a> files are located.</td><td></td></tr>
<tr><td><code>-C</code>, <code>--compress</code></td><td>Compress all information sent between the client and the server if both support compression.</td><td></td></tr>
<tr><td><code>--connect_timeout=val</code></td><td>Maximum time in seconds before connection timeout. The default value is 43200 (12 hours).</td><td></td></tr>
<tr><td><code>-c val</code>, <code>--count=val</code></td><td>Number of iterations to make. This works with <code>-i</code> (<code>--sleep</code>) only.</td><td></td></tr>
<tr><td><code>--debug[=debug_options]</code>, <code>-# [debug_options]</code></td><td>Write a debugging log. A typical debug_options string is <code>d:t:o,file_name</code>. The default is <code>d:t:o,/tmp/mysqladmin.trace</code>.</td><td></td></tr>
<tr><td><code>--debug-check</code></td><td>Check memory and open file usage at exit.</td><td></td></tr>
<tr><td><code>--debug-info</code></td><td>Print debugging information and memory and CPU usage statistics when the program exits.</td><td></td></tr>
<tr><td><code>--default-auth=plugin</code></td><td>Default authentication client-side plugin to use.</td><td></td></tr>
<tr><td><code>--default-character-set=name</code></td><td>Set the default character set.</td><td></td></tr>
<tr><td><code>-f</code>, <code>--force</code></td><td>Don't ask for confirmation on drop database; with multiple commands, continue even if an error occurs.</td><td></td></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Display this help and exit.</td><td></td></tr>
<tr><td><code>-h name</code>, <code>--host=name</code></td><td>Hostname to connect to.</td><td></td></tr>
<tr><td><code>-l</code>, <code>--local</code></td><td>Suppress the SQL command(s) from being written to the <a href="/kb/en/binary-log/">binary log</a> by enabling <a href="/kb/en/replication-and-binary-log-server-system-variables/#sql_log_bin">sql_log_bin=0</a> for the session, or, from <a href="/kb/en/mariadb-1027-release-notes/">MariaDB 10.2.7</a> and <a href="/kb/en/mariadb-10124-release-notes/">MariaDB 10.1.24</a>, for flush commands only, using <code>FLUSH LOCAL</code>  rather than <code>SET sql_log_bin=0</code>, so the privilege requirement is RELOAD rather than SUPER.</td><td><a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a>, <a href="/kb/en/mariadb-10122-release-notes/">MariaDB 10.1.22</a>, <a href="/kb/en/mariadb-10030-release-notes/">MariaDB 10.0.30</a></td></tr>
<tr><td><code>-b</code>, <code>--no-beep</code></td><td>Turn off beep on error.</td><td></td></tr>
<tr><td><code>-p[password]</code>, <code>--password[=password]</code></td><td>Password to use when connecting to server. If password is not given it's asked from the terminal.</td><td></td></tr>
<tr><td><code>--pipe</code>, <code>-W</code></td><td>On Windows, connect to the server via a named pipe. This option applies only if the server supports named-pipe connections.</td><td></td></tr>
<tr><td><code>-P portnum</code>, <code>--port=portnum</code></td><td>Port number to use for connection, or 0 for default to, in order of preference, my.cnf, $MYSQL_TCP_PORT, /etc/services, built-in default (3306).</td><td></td></tr>
<tr><td><code>--protocol=name</code></td><td>The protocol to use for connection (tcp, socket, pipe, memory).</td><td></td></tr>
<tr><td><code>-r</code>, <code>--relative</code></td><td>Show difference between current and previous values when used with <code>-i</code>. Currently only works with extended-status.</td><td></td></tr>
<tr><td><code>-O value</code>, <code>--set-variable=vaue</code></td><td>Change the value of a variable. Please note that this option is deprecated; you can set variables directly with <code>--variable-name=value</code>.</td><td></td></tr>
<tr><td><code>--shutdown_timeout=val</code></td><td>Maximum number of seconds to wait for server shutdown. The default value is 3600 (1 hour).</td><td></td></tr>
<tr><td><code>-s</code>, <code>--silent</code></td><td>Silently exit if one can't connect to server.</td><td></td></tr>
<tr><td><code>-i delay</code>, <code>--sleep=delay </code></td><td>Execute commands repeatedly, sleeping for <em>delay</em> seconds in between. The <code>--count</code> option determines the number of iterations. If <code>--count</code> is not given, <em>mysqladmin</em> executes commands indefinitely until interrupted.</td><td></td></tr>
<tr><td><code>-S name</code>, <code>--socket=name</code></td><td>For connections to localhost, the Unix socket file to use, or, on Windows, the name of the named pipe to use.</td><td></td></tr>
<tr><td><code>--ssl</code></td><td>Enables <a href="/kb/en/data-in-transit-encryption/">TLS</a>. TLS is also enabled even without setting this option when certain other TLS options are set. Starting with <a href="/kb/en/what-is-mariadb-102/">MariaDB 10.2</a>, the <code>--ssl</code> option will not enable <a href="/kb/en/secure-connections-overview/#server-certificate-verification">verifying the server certificate</a> by default. In order to verify the server certificate, the user must specify the <code>--ssl-verify-server-cert</code> option.</td><td></td></tr>
<tr><td><code>--ssl-ca=name</code></td><td>Defines a path to a PEM file that should contain one or more X509 certificates for trusted Certificate Authorities (CAs) to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. See <a href="/kb/en/secure-connections-overview/#certificate-authorities-cas">Secure Connections Overview: Certificate Authorities (CAs)</a> for more information. This option implies the <code>--ssl</code> option.</td><td></td></tr>
<tr><td><code>--ssl-capath=name</code></td><td>Defines a path to a directory that contains one or more PEM files that should each contain one X509 certificate for a trusted Certificate Authority (CA) to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the <code><a href="https://www.openssl.org/docs/man1.1.1/man1/rehash.html">openssl rehash</a></code> command. See <a href="/kb/en/secure-connections-overview/#certificate-authorities-cas">Secure Connections Overview: Certificate Authorities (CAs)</a> for more information. This option is only supported if the client was built with OpenSSL or yaSSL. If the client was built with GnuTLS or Schannel, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms. This option implies the <code>--ssl</code> option.</td><td></td></tr>
<tr><td><code>--ssl-cert=name</code></td><td>Defines a path to the X509 certificate file to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. This option implies the <code>--ssl</code> option.</td><td></td></tr>
<tr><td><code>--ssl-cipher=name</code></td><td>List of permitted ciphers or cipher suites to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option implies the <code>--ssl</code> option.</td><td></td></tr>
<tr><td><code>--ssl-crl=name</code></td><td>Defines a path to a PEM file that should contain one or more revoked X509 certificates to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. See <a href="/kb/en/secure-connections-overview/#certificate-revocation-lists-crls">Secure Connections Overview: Certificate Revocation Lists (CRLs)</a> for more information. This option is only supported if the client was built with OpenSSL or Schannel. If the client was built with yaSSL or GnuTLS, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms.</td><td></td></tr>
<tr><td><code>--ssl-crlpath=name</code></td><td>Defines a path to a directory that contains one or more PEM files that should each contain one revoked X509 certificate to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the <code><a href="https://www.openssl.org/docs/man1.1.1/man1/rehash.html">openssl rehash</a></code> command. See <a href="/kb/en/secure-connections-overview/#certificate-revocation-lists-crls">Secure Connections Overview: Certificate Revocation Lists (CRLs)</a> for more information. This option is only supported if the client was built with OpenSSL. If the client was built with yaSSL, GnuTLS, or Schannel, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms.</td><td></td></tr>
<tr><td><code>--ssl-key=name</code></td><td>Defines a path to a private key file to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. This option implies the <code>--ssl</code> option.</td><td></td></tr>
<tr><td><code>--ssl-verify-server-cert</code></td><td>Enables <a href="/kb/en/secure-connections-overview/#server-certificate-verification">server certificate verification</a>. This option is disabled by default.</td><td></td></tr>
<tr><td><code>--tls-version=name</code></td><td>This option accepts a comma-separated list of TLS protocol versions. A TLS protocol version will only be enabled if it is present in this list. All other TLS protocol versions will not be permitted. See <a href="/kb/en/secure-connections-overview/#tls-protocol-versions">Secure Connections Overview: TLS Protocol Versions</a> for more information.</td><td><a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a></td></tr>
<tr><td><code>-u</code>, <code>--user=name</code></td><td>User for login if not current user.</td><td></td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>Write more information.</td><td></td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Output version information and exit.</td><td></td></tr>
<tr><td><code>-E</code>, <code>--vertical</code></td><td>Print output vertically. Is similar to '<code>--relative</code>', but prints output vertically.</td><td></td></tr>
<tr><td><code>-w[count]</code>, <code>--wait[=count]</code></td><td>If the connection cannot be established, wait and retry instead of aborting. If a <em>count</em> value is given, it indicates the number of times to retry. The default is one time.</td><td></td></tr>
<tr><td><code>--wait-for-all-slaves</code></td><td>Wait for the last binlog event to be sent to all connected slaves before shutting down. This option is off by default.</td><td><a href="/kb/en/mariadb-1044-release-notes/">MariaDB 10.4.4</a></td></tr>
</tbody></table>

### Option Files

In addition to reading options from the command-line, `mysqladmin` can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). If an unknown option is provided to `mysqladmin` in an option file, then it is ignored.

The following options relate to how MariaDB command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
<tr><td><code>--defaults-group-suffix=# </code></td><td>In addition to the default option groups, also read option groups with this suffix.</td></tr>
</tbody></table>

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, `mysqladmin` is linked with [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/). However, MariaDB Connector/C does not yet handle the parsing of option files for this client. That is still performed by the server option file parsing code. See  [MDEV-19035](https://jira.mariadb.org/browse/MDEV-19035) for more information.

#### Option Groups

`mysqladmin` reads options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysqladmin]</code></td><td>&nbsp;Options read by <code>mysqladmin</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-admin]</code></td><td>Options read by <code>mysqladmin</code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[client]</code></td><td>&nbsp;Options read by all MariaDB and MySQL <a href="/kb/en/clients-utilities/">client programs</a>, which includes both MariaDB and MySQL clients. For example, <code>mysqldump</code>.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[client-mariadb]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a>.</td></tr>
</tbody></table>

## mysqladmin Variables

Variables can be set with `--variable-name=value`.

<table><tbody><tr><th>Variables and boolean options</th><th>Value</th></tr>
<tr><td><code>count</code></td><td><code>0</code></td></tr>
<tr><td><code>debug-check</code></td><td><code>FALSE</code></td></tr>
<tr><td><code>debug-info</code></td><td><code>FALSE</code></td></tr>
<tr><td><code>force</code></td><td><code>FALSE</code></td></tr>
<tr><td><code>compress</code></td><td><code>FALSE</code></td></tr>
<tr><td><code>character-sets-dir</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>default-character-set</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>host</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>no-beep</code></td><td><code>FALSE</code></td></tr>
<tr><td><code>port</code></td><td><code>3306</code></td></tr>
<tr><td><code>relative</code></td><td><code>FALSE</code></td></tr>
<tr><td><code>socket</code></td><td><code>/var/run/mysqld/mysqld.sock</code></td></tr>
<tr><td><code>sleep</code></td><td><code>0</code></td></tr>
<tr><td><code>ssl</code></td><td><code>FALSE</code></td></tr>
<tr><td><code>ssl-ca</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>ssl-capath</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>ssl-cert</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>ssl-cipher</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>ssl-key</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>ssl-verify-server-cert</code></td><td><code>FALSE</code></td></tr>
<tr><td><code>user</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>verbose</code></td><td><code>FALSE</code></td></tr>
<tr><td><code>vertical</code></td><td><code>FALSE</code></td></tr>
<tr><td><code>connect_timeout</code></td><td><code>43200</code></td></tr>
<tr><td><code>shutdown_timeout</code></td><td><code>3600</code></td></tr>
</tbody></table>

## mysqladmin Commands

```sql
mysqladmin [options] command [command-arg] [command [command-arg]] ...
```

<em>Command</em> is one or more of the following. Commands may be shortened to a unique prefix.

<table><tbody><tr><th>Command</th><th>Description</th><th>Added</th></tr>
<tr><td><code>create databasename</code></td><td>Create a new database.</td><td></td></tr>
<tr><td><code>debug</code></td><td>Instruct server to write debug information to log.</td><td></td></tr>
<tr><td><code>drop databasename</code></td><td>Delete a database and all its tables.</td><td></td></tr>
<tr><td><code>extended-status</code></td><td>Return all <a href="/kb/en/server-status-variables/">status variables</a> and their values.</td><td></td></tr>
<tr><td><code>flush-all-statistics</code></td><td>Flush all statistics tables</td><td></td></tr>
<tr><td><code>flush-all-status</code></td><td>Flush status and statistics.</td><td></td></tr>
<tr><td><code>flush-binary-log</code></td><td>Flush <a href="/kb/en/binary-log/">binary log</a>.</td><td><a href="/kb/en/mariadb-10030-release-notes/">MariaDB 10.0.30</a>,<br> <a href="/kb/en/mariadb-10125-release-notes/">MariaDB 10.1.25</a>,<br> <a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a></td></tr>
<tr><td><code>flush-client-statistics</code></td><td>Flush client statistics.</td><td></td></tr>
<tr><td><code>flush-engine-log</code></td><td>Flush engine log.</td><td><a href="/kb/en/mariadb-10030-release-notes/">MariaDB 10.0.30</a>,<br> <a href="/kb/en/mariadb-10125-release-notes/">MariaDB 10.1.25</a>,<br> <a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a></td></tr>
<tr><td><code>flush-error-log</code></td><td>Flush <a href="/kb/en/error-log/">error log</a>.</td><td><a href="/kb/en/mariadb-10030-release-notes/">MariaDB 10.0.30</a>,<br> <a href="/kb/en/mariadb-10125-release-notes/">MariaDB 10.1.25</a>,<br> <a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a></td></tr>
<tr><td><code>flush-general-log</code></td><td>Flush <a href="/kb/en/general-query-log/">general query log</a>.</td><td><a href="/kb/en/mariadb-10030-release-notes/">MariaDB 10.0.30</a>,<br> <a href="/kb/en/mariadb-10125-release-notes/">MariaDB 10.1.25</a>,<br> <a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a></td></tr>
<tr><td><code>flush-hosts</code></td><td>Flush all cached hosts.</td><td></td></tr>
<tr><td><code>flush-index-statistics</code></td><td>Flush index statistics.</td><td></td></tr>
<tr><td><code>flush-logs</code></td><td>Flush all logs.</td><td></td></tr>
<tr><td><code>flush-privileges</code></td><td>Reload grant tables (same as reload).</td><td></td></tr>
<tr><td><code>flush-relay-log</code></td><td>Flush <a href="/kb/en/relay-log/">relay log</a>.</td><td><a href="/kb/en/mariadb-10030-release-notes/">MariaDB 10.0.30</a>,<br> <a href="/kb/en/mariadb-10125-release-notes/">MariaDB 10.1.25</a>,<br> <a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a></td></tr>
<tr><td><code>flush-slow-log</code></td><td>Flush slow query log.</td><td></td></tr>
<tr><td><code>flush-ssl</code></td><td>Flush SSL certificates.</td><td>(unreleased)</td></tr>
<tr><td><code>flush-status</code></td><td>Clear <a href="/kb/en/server-status-variables/">status variables</a>.</td><td></td></tr>
<tr><td><code>flush-table-statistics</code></td><td>Clear table statistics.</td><td></td></tr>
<tr><td><code>flush-tables</code></td><td>Flush all tables.</td><td></td></tr>
<tr><td><code>flush-threads</code></td><td>Flush the thread cache.</td><td></td></tr>
<tr><td><code>flush-user-resources</code></td><td>Flush user resources.</td><td><a href="/kb/en/mariadb-10030-release-notes/">MariaDB 10.0.30</a>,<br> <a href="/kb/en/mariadb-10125-release-notes/">MariaDB 10.1.25</a>,<br> <a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a></td></tr>
<tr><td><code>flush-user-statistics</code></td><td>Flush user statistics.</td><td></td></tr>
<tr><td><code>kill id,id,...</code></td><td>Kill mysql threads.</td><td></td></tr>
<tr><td><code>password new-password</code></td><td>Change old password to new-password. The new password can be passed on the commandline as the next argument (for example, <code>mysqladmin password "new_password"</code>, or, from <a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a>, can be omitted (as long as no other command follows), in which case the user will be prompted for a password. If the password contains special characters, it needs to be enclosed in quotation marks. In Windows, the quotes can only be double quotes, as single quotes are assumed to be part of the password. If the server was started with the <a href="/kb/en/mysqld-options/#-skip-grant-tables">--skip-grant-tables</a> option, changing the password in this way will have no effect.</td><td></td></tr>
<tr><td><code>old-password new-password</code></td><td>Change old password to new-password using the old pre-MySQL 4.1 format.</td><td></td></tr>
<tr><td><code>ping</code></td><td>Check if mysqld is alive. Return status is 0 if the server is running (even in the case of an error such as access denied), 1 if it is not.</td><td></td></tr>
<tr><td><code>processlist</code></td><td>Show list of active threads in server, equivalent to <a href="/kb/en/show-processlist/">SHOW PROCESSLIST</a>. With <code>--verbose</code>, equivalent to <a href="/kb/en/show-processlist/">SHOW FULL PROCESSLIST</a>.</td><td></td></tr>
<tr><td><code>reload</code></td><td>Reload grant tables.</td><td></td></tr>
<tr><td><code>refresh</code></td><td>Flush all tables and close and open log files.</td><td></td></tr>
<tr><td><code>shutdown</code></td><td>Take server down by executing the <a href="/kb/en/shutdown/">SHUTDOWN</a> command on the server. If connected to a local server using a Unix socket file, mysqladmin waits until the server's process ID file has been removed to ensure that the server has stopped properly. See also the <code>--wait-for-all-slaves</code> option.</td></tr>
<tr><td><code>status</code></td><td>Gives a short status message from the server.</td><td></td></tr>
<tr><td><code>start-all-slaves</code></td><td>Start all slaves.</td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><code>start-slave</code></td><td>Start replication on a slave server.</td><td></td></tr>
<tr><td><code>stop-all-slaves</code></td><td>Stop all slaves.</td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><code>stop-slave</code></td><td>Stop replication on a slave server.</td><td></td></tr>
<tr><td><code>variables</code></td><td>Prints variables available.</td><td></td></tr>
<tr><td><code>version</code></td><td>Returns version as well as status info from the server.</td><td></td></tr>
</tbody></table>

## The shutdown Command and the --wait-for-all-slaves Option

##### MariaDB starting with [10.4.4](/kb/en/mariadb-1044-release-notes/)

The `--wait-for-all-slaves` option was first added in [MariaDB 10.4.4](/kb/en/mariadb-1044-release-notes/).

When a master server is shutdown and it goes through the normal shutdown process, the master kills client threads in random order. By default, the master also considers its binary log dump threads to be regular client threads. As a consequence, the binary log dump threads can be killed while client threads still exist, and this means that data can be written on the master during a normal shutdown that won't be replicated. This is true even if [semi-synchronous replication](/replication/standard-replication/semisynchronous-replication) is being used.

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this problem can be solved by shutting down the server with the [mysqladmin](/clients-utilities/mysqladmin) utility and by providing the `--wait-for-all-slaves` option to the utility and by executing the `shutdown` command with the utility. For example:

```sql
mysqladmin --wait-for-all-slaves shutdown
```

When the `--wait-for-all-slaves` option is provided, the server only kills its binary log dump threads after all client threads have been killed, and it only completes the shutdown after the last [binary log](/mariadb-administration/server-monitoring-logs/binary-log) has been sent to all connected slaves.

See [Replication Threads: Binary Log Dump Threads and the Shutdown Process](/kb/en/replication-threads/#binary-log-dump-threads-and-the-shutdown-process) for more information.

## Examples

Quick check of what the server is doing:

```sql
shell> mysqladmin status
Uptime: 8023 Threads: 1 Questions: 14 Slow queries: 0 Opens: 15 Flush tables: 1 Open tables: 8 Queries per second avg: 0.1
shell> mysqladmin processlist
+----+-------+-----------+----+---------+------+-------+------------------+
| Id | User | Host | db | Command | Time | State | Info |
+----+-------+-----------+----+---------+------+-------+------------------+
....
+----+-------+-----------+----+---------+------+-------+------------------+
```

More extensive information of what is happening 'just now' changing
(great for troubleshooting a slow server):

```sql
shell> mysqladmin --relative --sleep=1 extended-status | grep -v " 0 "
```

Check the variables for a running server:

```sql
shell> mysqladmin variables | grep datadir
| datadir | /my/data/ |
```

Using a shortened prefix for the `version` command:

```sql
shell> mysqladmin ver
mysqladmin Ver 9.1 Distrib 10.1.6-MariaDB, for debian-linux-gnu on x86_64
Copyright (c) 2000, 2015, Oracle, MariaDB Corporation Ab and others.

Server version		10.1.6-MariaDB-1~trusty-log
Protocol version	10
Connection		Localhost via UNIX socket
UNIX socket		/var/run/mysqld/mysqld.sock
Uptime:			1 hour 33 min 33 sec

Threads: 1 Questions: 281 Slow queries: 0 Opens: 64 Flush tables: 1 Open tables: 76 Queries per second avg: 0.050
```

### Other Ways To Stop mysqld (Unix)

If you get the error:

```sql
mysqladmin: shutdown failed; error: 'Access denied; you need (at least one of) the SHUTDOWN privilege(s) for this operation'
```

It means that you didn't use <code class="fixed" style="white-space:pre-wrap">mysqladmin</code> with a user that has the SUPER or SHUTDOWN privilege.

If you don't know the user password, you can still take the mysqld process down with a system <code class="highlight fixed" style="white-space:pre-wrap">kill</code> command:

```sql
kill -SIGTERM pid-of-mysqld-process
```

The above is identical to <code class="highlight fixed" style="white-space:pre-wrap">mysqladmin shutdown</code>.

On windows you should use:

```sql
NET STOP MySQL
```

With [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and newer you can use the [SHUTDOWN](/sql-statements-structure/sql-statements/administrative-sql-statements/shutdown) command from any client.

## See Also

- [SHUTDOWN command](/sql-statements-structure/sql-statements/administrative-sql-statements/shutdown)
- [mytop](http://www.mysqlfanboy.com/mytop-3/), a 'top' like program for
 MariaDB/MySQL that allows you to see what the server is doing. A mytop
 optimized for MariaDB is included in [MariaDB 5.3](/kb/en/what-is-mariadb-53/)