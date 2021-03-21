# mysql Command-line Client

## About the mysql Command-Line Client

<strong>mysql</strong> (from [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), [also called mariadb](/clients-utilities/mysql-client/mariadb-command-line-client)) is a simple SQL shell (with GNU readline
capabilities). It supports interactive and non-interactive use. When used
interactively, query results are presented in an ASCII-table format. When used
non-interactively (for example, as a filter), the result is presented in
tab-separated format. The output format can be changed using command options.

If you have problems due to insufficient memory for large result sets, use the
`--quick` option. This forces mysql to retrieve results from
the server a row at a time rather than retrieving the entire result set and
buffering it in memory before displaying it.  This is done by returning the
result set using the `mysql_use_result()` C API function in the
client/server library rather than `mysql_store_result()`.

Using mysql is very easy. Invoke it from the prompt of your command interpreter
as follows:

```sql
mysql db_name
```

Or:

```sql
mysql --user=user_name --password=your_password db_name
```

Then type an SQL statement, end it with “;”, \g, or \G and press Enter.

Typing Control-C causes mysql to attempt to kill the
current statement. If this cannot be done, or Control-C is typed again before
the statement is killed, mysql exits.

You can execute SQL statements in a script file (batch file) like this:

```sql
mysql db_name < script.sql > output.tab
```

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb` is available as a symlink to `mysql`.

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mysql` is the symlink, and `mariadb` the binary name.

## Using mysql

The command to use `mysql` and the general syntax is:

```sql
mysql <options>
```

### Options

`mysql` supports the following options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Display this help and exit.</td></tr>
<tr><td><code>-I</code>, <code>--help</code></td><td>Synonym for <code>-?</code></td></tr>
<tr><td><code>--abort-source-on-error</code></td><td>Abort 'source filename' operations in case of errors.</td></tr>
<tr><td><code>--auto-rehash</code></td><td>Enable automatic rehashing. This option is on by default, which enables database, table, and column name completion. Use <code>--disable-auto-rehash</code>, <code>--no-auto-rehash</code> or <code>skip-auto-rehash</code> to disable rehashing. That causes mysql to start faster, but you must issue the rehash command if you want to use name completion. To complete a name, enter the first part and press Tab. If the name is unambiguous, mysql completes it. Otherwise, you can press Tab again to see the possible names that begin with what you have typed so far. Completion does not occur if there is no default database.</td></tr>
<tr><td><code>-A</code>, <code>--no-auto-rehash</code></td><td>No automatic rehashing. One has to use 'rehash' to get table and field completion. This gives a quicker start of mysql and disables rehashing on reconnect.</td></tr>
<tr><td><code>--auto-vertical-output</code></td><td>Automatically switch to vertical output mode if the result is wider than the terminal width.</td></tr>
<tr><td><code>-B</code>, <code>--batch</code></td><td>Print results using tab as the column separator, with each row on a new line. With this option, mysql does not use the history file. Batch mode results in nontabular output format and escaping of special characters. Escaping may be disabled by using raw mode; see the description for the <code>--raw</code> option. (Enables <code>--silent</code>.)</td></tr>
<tr><td><code>--binary-mode</code></td><td>By default, ASCII '\0' is disallowed and '\r\n' is translated to '\n'. This switch turns off both features, and also turns off parsing of all client commands except \C and DELIMITER, in non-interactive mode (for input piped to mysql or loaded using the 'source' command). This is necessary when processing output from <a href="/kb/en/mysqlbinlog/">mysqlbinlog</a> that may contain blobs.</td></tr>
<tr><td><code>--character-sets-dir=name</code></td><td>Directory for <a href="/kb/en/data-types-character-sets-and-collations/">character set</a> files.</td></tr>
<tr><td><code>--column-names</code></td><td>Write column names in results. (Defaults to on; use <code>--skip-column-names</code> to disable.)</td></tr>
<tr><td><code>--column-type-info</code></td><td>Display column type information.</td></tr>
<tr><td><code>-c</code>, <code>--comments</code></td><td>Preserve comments. Send comments to the server. The default is <code>--skip-comments</code> (discard comments), enable with <code>--comments</code>.</td></tr>
<tr><td><code>-C</code>, <code>--compress</code></td><td>Compress all information sent between the client and the server if both support compression.</td></tr>
<tr><td><code>--connect-expired-password</code></td><td>Notify the server that this client is prepared to handle <a href="/kb/en/user-password-expiry/">expired password sandbox mode</a> even if <code>--batch</code> was specified. From <a href="/kb/en/mariadb-1043-release-notes/">MariaDB 10.4.3</a>.</td></tr>
<tr><td><code>--connect-timeout=num</code></td><td>Number of seconds before connection timeout. Defaults to zero.</td></tr>
<tr><td><code>-D</code>, <code>--database=name</code></td><td>Database to use.</td></tr>
<tr><td><code>-# [options]</code>, <code>--debug[=options]</code></td><td>On debugging builds, write a debugging log. A typical debug_options string is <code>d:t:o,file_name</code>. The default is <code>d:t:o,/tmp/mysql.trace</code>.</td></tr>
<tr><td><code>--debug-check</code></td><td>Check memory and open file usage at exit.</td></tr>
<tr><td><code>-T</code>, <code>--debug-info</code></td><td>Print some debug info at exit.</td></tr>
<tr><td><code>--default-auth=plugin</code></td><td>Default authentication client-side plugin to use.</td></tr>
<tr><td><code>--default-character-set=name</code></td><td>Set the default <a href="/kb/en/data-types-character-sets-and-collations/">character set</a>. A common issue that can occur when the operating system uses utf8 or another multibyte character set is that output from the mysql client is formatted incorrectly, due to the fact that the MariaDB client uses the latin1 character set by default. You can usually fix such issues by using this option to force the client to use the system character set instead.  If set to <code>auto</code> the character set is taken from the client environment (<code>LC_CTYPE</code> on Unix).</td></tr>
<tr><td><code>--defaults-extra-file=file</code></td><td>Read this file after the global files are read. Must be given as the first option.</td></tr>
<tr><td><code>--defaults-file=file</code></td><td>Only read default options from the given <em>file</em>. Must be given as the first option.</td></tr>
<tr><td><code>--defaults-group-suffix=suffix</code></td><td>In addition to the given groups, also read groups with this suffix.</td></tr>
<tr><td><code>--delimiter=name</code></td><td>Delimiter to be used. The default is the semicolon character (“;”).</td></tr>
<tr><td><code>-e</code>, <code>--execute=name</code></td><td>Execute statement and quit. Disables <code>--force</code> and history file. The default output format is like that produced with <code>--batch</code>.</td></tr>
<tr><td><code>-f</code>, <code>--force</code></td><td>Continue even if we get an SQL error. Sets <code>--abort-source-on-error</code> to 0.</td></tr>
<tr><td><code>-h</code>, <code>--host=name</code></td><td>Connect to host.</td></tr>
<tr><td><code>-H</code>, <code>--html</code></td><td>Produce HTML output.</td></tr>
<tr><td><code>-U</code>, <code>--i-am-a-dummy</code></td><td>Synonym for option <code>--safe-updates</code>, <code>-U</code>.</td></tr>
<tr><td><code>-i</code>, <code>--ignore-spaces</code></td><td>Ignore space after function names. Allows one to have spaces (including tab characters and new line characters) between function name and '('. The drawback is that this causes built in functions to become reserved words.</td></tr>
<tr><td><code>--init-command=str</code></td><td>SQL Command to execute when connecting to the MariaDB server. Will automatically be re-executed when reconnecting.</td></tr>
<tr><td><code>--line-numbers</code></td><td>Write line numbers for errors. (Defaults to on; use <code>--skip-line-numbers</code> to disable.)</td></tr>
<tr><td><code>--local-infile</code></td><td>Enable or disable LOCAL capability for <a href="/kb/en/load-data-infile/">LOAD DATA INFILE</a>. With no value, the option enables LOCAL. The option may be given as<code>--local-infile=0</code> or <code>--local-infile=1</code> to explicitly disable or enable LOCAL. Enabling LOCAL has no effect if the server does not also support it.</td></tr>
<tr><td><code>--max-allowed-packet=num</code></td><td>The maximum packet length to send to or receive from server. The default is 16MB, the maximum 1GB.</td></tr>
<tr><td><code>--max-join-size=num</code></td><td>Automatic limit for rows in a join when using <code>--safe-updates</code>. Default is 1000000.</td></tr>
<tr><td><code>-G</code>, <code>--named-commands</code></td><td>Enable named commands. Named commands mean mysql's internal commands (see below) . When enabled, the named commands can be used from any line of the query, otherwise only from the first line, before an enter. Long-format commands are allowed, not just short-format commands. For example, <code>quit</code> and <code>\q</code> are both recognized. Disable with <code>--disable-named-commands</code>. This option is disabled by default.</td></tr>
<tr><td><code>--net-buffer-length=num</code></td><td>The buffer size for TCP/IP and socket communication. Default is 16KB.</td></tr>
<tr><td><code>-b</code>, <code>--no-beep</code></td><td>Turn off beep on error.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file. Must be given as the first option.</td></tr>
<tr><td><code>-o</code>, <code>--one-database</code></td><td>Ignore statements except those those that occur while the default database is the one named on the command line. This filtering is limited, and based only on <a href="/kb/en/use/">USE</a> statements. This is useful for skipping updates to other databases in the binary log.</td></tr>
<tr><td><code>--pager[=name]</code></td><td>Pager to use to display results (Unix only). If you don't supply an option, the default pager is taken from your ENV variable PAGER. Valid pagers are <em>less</em>, <em>more</em>, <em>cat [&gt; filename]</em>, etc. See interactive help (\h) also. This option does not work in batch mode. Disable with <code>--disable-pager</code>. This option is disabled by default.</td></tr>
<tr><td><code>-p</code>, <code>--password[=name]</code></td><td>Password to use when connecting to server. If you use the short option form (-p), you cannot have a space between the option and the password. If you omit the password value following the <code>--password</code> or <code>-p</code> option on the command line, mysql prompts for one. Specifying a password on the command line should be considered insecure. You can use an option file to avoid giving the password on the command line.</td></tr>
<tr><td><code>--plugin-dir=name</code></td><td>Directory for client-side plugins.</td></tr>
<tr><td><code>-P</code>, <code>--port=num</code></td><td>Port number to use for connection or 0 for default to, in order of preference, my.cnf, $MYSQL_TCP_PORT, /etc/services, built-in default (3306).</td></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit. Must be given as the first option.</td></tr>
<tr><td><code>--progress-reports</code></td><td>Get <a href="/kb/en/progress-reporting/">progress reports</a> for long running commands (such as <a href="/kb/en/alter-table/">ALTER TABLE</a>). (Defaults to on; use <code>--skip-progress-reports</code> to disable.)</td></tr>
<tr><td><code>--prompt=name</code></td><td>Set the mysql prompt to this value. See <a href="#prompt-command">prompt command</a> for options.</td></tr>
<tr><td><code>--protocol=name</code></td><td>The protocol to use for connection (tcp, socket, pipe, memory).</td></tr>
<tr><td><code>-q</code>, <code>--quick</code></td><td>Don't cache result, print it row by row. This may slow down the server if the output is suspended. Doesn't use history file.</td></tr>
<tr><td><code>-r</code>, <code>--raw</code></td><td>For tabular output, the “boxing” around columns enables one column value to be distinguished from another. For nontabular output (such as is produced in batch mode or when the <code>--batch</code> or <code>--silent</code> option is given), special characters are escaped in the output so they can be identified easily. Newline, tab, NUL, and backslash are written as <code>\n</code>, <code>\t</code>, <code>\0</code>, and <code><br></code>. The <code>--raw</code> option disables this character escaping.</td></tr>
<tr><td><code>--reconnect</code></td><td>Reconnect if the connection is lost. This option is enabled by default. Disable with <code>--disable-reconnect</code> or <code>skip-reconnect</code>.</td></tr>
<tr><td><code>-U</code>, <code>--safe-updates</code></td><td>Allow only those <a href="/kb/en/update/">UPDATE</a> and <a href="/kb/en/delete/">DELETE</a> statements that specify which rows to modify by using key values. If you have set this option in an option file, you can override it by using <code>--safe-updates</code> on the command line. See <a href="#using-the-safe-updates-option">using the --safe-updates option</a> for more.</td></tr>
<tr><td><code>--secure-auth</code></td><td>Refuse client connecting to server if it uses old (pre-MySQL4.1.1) protocol. Defaults to false (unlike MySQL since 5,6, which defaults to true).</td></tr>
<tr><td><code>--select-limit=num</code></td><td>Automatic limit for SELECT when using --safe-updates. Default 1000.</td></tr>
<tr><td><code>--server-arg=name</code></td><td>Send embedded server this as a parameter.</td></tr>
<tr><td><code>--shared-memory-base-name=name</code></td><td>Shared-memory name to use for Windows connections using shared memory to a local server (started with the --shared-memory option). Case-sensitive.</td></tr>
<tr><td><code>--show-warnings</code></td><td>Show warnings after every statement. Applies to interactive and batch mode.</td></tr>
<tr><td><code>--sigint-ignore</code></td><td>Ignore SIGINT signals (usually CTRL-C).</td></tr>
<tr><td><code>-s</code>, <code>--silent</code></td><td>Be more silent. This option can be given multiple times to produce less and less output. This option results in nontabular output format and escaping of special characters. Escaping may be disabled by using raw mode; see the description for the <code>--raw</code> option.</td></tr>
<tr><td><code>-L</code>, <code>--skip-auto-rehash</code></td><td>Disable automatic rehashing. See <code>--auto-rehash</code>.</td></tr>
<tr><td><code>-N</code>, <code>--skip-column-names</code></td><td>Don't write column names in results. See <code>--column-names</code>.</td></tr>
<tr><td><code>-L</code>, <code>--skip-comments</code></td><td>Discard comments. Set by default, see <code>--comments</code> to enable.</td></tr>
<tr><td><code>-L</code>, <code>--skip-line-numbers</code></td><td>Don't write line number for errors. See <code>--line-numbers</code>.</td></tr>
<tr><td><code>-L</code>, <code>--skip-progress-reports</code></td><td>Disables getting <a href="/kb/en/progress-reporting/">progress reports</a> for long running commands. See <code>--progress-reports</code>.</td></tr>
<tr><td><code>-L</code>, <code>--skip-reconnect</code></td><td>Don't reconnect if the connection is lost. See <code>--reconnect</code>.</td></tr>
<tr><td><code>-S</code>, <code>--socket=name</code></td><td>For connections to localhost, the Unix socket file to use, or, on Windows, the name of the named pipe to use.</td></tr>
<tr><td><code>--ssl</code></td><td>Enables <a href="/kb/en/data-in-transit-encryption/">TLS</a>. TLS is also enabled even without setting this option when certain other TLS options are set. Starting with <a href="/kb/en/what-is-mariadb-102/">MariaDB 10.2</a>, the <code>--ssl</code> option will not enable <a href="/kb/en/secure-connections-overview/#server-certificate-verification">verifying the server certificate</a> by default. In order to verify the server certificate, the user must specify the <code>--ssl-verify-server-cert</code> option.</td></tr>
<tr><td><code>--ssl-ca=name</code></td><td>Defines a path to a PEM file that should contain one or more X509 certificates for trusted Certificate Authorities (CAs) to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. See <a href="/kb/en/secure-connections-overview/#certificate-authorities-cas">Secure Connections Overview: Certificate Authorities (CAs)</a> for more information. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-capath=name</code></td><td>Defines a path to a directory that contains one or more PEM files that should each contain one X509 certificate for a trusted Certificate Authority (CA) to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the <code><a href="https://www.openssl.org/docs/man1.1.1/man1/rehash.html">openssl rehash</a></code> command. See <a href="/kb/en/secure-connections-overview/#certificate-authorities-cas">Secure Connections Overview: Certificate Authorities (CAs)</a> for more information. This option is only supported if the client was built with OpenSSL or yaSSL. If the client was built with GnuTLS or Schannel, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-cert=name</code></td><td>Defines a path to the X509 certificate file to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-cipher=name</code></td><td>List of permitted ciphers or cipher suites to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-crl=name</code></td><td>Defines a path to a PEM file that should contain one or more revoked X509 certificates to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. See <a href="/kb/en/secure-connections-overview/#certificate-revocation-lists-crls">Secure Connections Overview: Certificate Revocation Lists (CRLs)</a> for more information. This option is only supported if the client was built with OpenSSL or Schannel. If the client was built with yaSSL or GnuTLS, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms.</td></tr>
<tr><td><code>--ssl-crlpath=name</code></td><td>Defines a path to a directory that contains one or more PEM files that should each contain one revoked X509 certificate to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the <code><a href="https://www.openssl.org/docs/man1.1.1/man1/rehash.html">openssl rehash</a></code> command. See <a href="/kb/en/secure-connections-overview/#certificate-revocation-lists-crls">Secure Connections Overview: Certificate Revocation Lists (CRLs)</a> for more information. This option is only supported if the client was built with OpenSSL. If the client was built with yaSSL, GnuTLS, or Schannel, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms.</td></tr>
<tr><td><code>--ssl-key=name</code></td><td>Defines a path to a private key file to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-verify-server-cert</code></td><td>Enables <a href="/kb/en/secure-connections-overview/#server-certificate-verification">server certificate verification</a>. This option is disabled by default.</td></tr>
<tr><td><code>-t</code>, <code>--table</code></td><td>Display output in table format. This is the default for interactive use, but can be used to produce table output in batch mode.</td></tr>
<tr><td><code>--tee=name</code></td><td>Append everything into outfile. See interactive help (\h) also. Does not work in batch mode. Disable with <code>--disable-tee</code>. This option is disabled by default.</td></tr>
<tr><td><code>--tls-version=name</code></td><td>This option accepts a comma-separated list of TLS protocol versions. A TLS protocol version will only be enabled if it is present in this list. All other TLS protocol versions will not be permitted. See <a href="/kb/en/secure-connections-overview/#tls-protocol-versions">Secure Connections Overview: TLS Protocol Versions</a> for more information. This option was added in <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>-n</code>, <code>--unbuffered</code></td><td>Flush buffer after each query.</td></tr>
<tr><td><code>-u</code>, <code>--user=name</code></td><td>User for login if not current user.</td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>Write more. (-v -v -v gives the table output format).</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Output version information and exit.</td></tr>
<tr><td><code>-E</code>, <code>--vertical</code></td><td>Print the output of a query (rows) vertically. Use the <code>\G</code> delimiter to apply to a particular statement if this option is not enabled.</td></tr>
<tr><td><code>-w</code>, <code>--wait</code></td><td>If the connection cannot be established, wait and retry instead of aborting.</td></tr>
<tr><td><code>-X</code>, <code>--xml</code></td><td>Produce XML output. See the <a href="/kb/en/mysqldump/#null-null-and-empty-values-in-xml">mysqldump --xml option</a> for more.</td></tr>
</tbody></table>

### Option Files

In addition to reading options from the command-line, `mysql` can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). If an unknown option is provided to `mysql` in an option file, then it is ignored.

The following options relate to how MariaDB command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
<tr><td><code>--defaults-group-suffix=# </code></td><td>In addition to the default option groups, also read option groups with this suffix.</td></tr>
</tbody></table>

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, `mysql` is linked with [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/). However, MariaDB Connector/C does not yet handle the parsing of option files for this client. That is still performed by the server option file parsing code. See  [MDEV-19035](https://jira.mariadb.org/browse/MDEV-19035) for more information.

#### Option Groups

`mysql` reads options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysql]</code></td><td>&nbsp;Options read by <code>mysql</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-client]</code></td><td>Options read by <code>mysql</code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[client]</code></td><td>&nbsp;Options read by all MariaDB and MySQL <a href="/kb/en/clients-utilities/">client programs</a>, which includes both MariaDB and MySQL clients. For example, <code>mysqldump</code>.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[client-mariadb]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a>.</td></tr>
</tbody></table>

## How to Specify Which Protocol to Use When Connecting to the mysqld Server

The following is true for all MySQL and MariaDB command line clients:

You can force which protocol to be used to connect to the `mysqld` server by giving the `protocol` option one of the following values: `tcp`, `socket`, `pipe` or `memory`.

If `protocol` is not specified, then the following happens:

### Linux/Unix

- If `hostname` is not specified or `hostname` is `localhost`, then Unix sockets are used. Unused connection parameters (such as `port`) will be ignored.
- In other cases (`hostname` is given and it's not `localhost`) then a tcpip connection through the `port` option is used.

Note that `localhost` is a special value. Using 127.0.0.1 is not the same thing. The latter will connect to the mysqld server through tcpip.

### Windows

- If `shared-memory-base-name` is specified and `hostname` is not specified or `hostname` is `localhost`, then the connection will happen through shared memory. Unused connection parameters (such as `port`) will be ignored.
- If `shared-memory-base-name` is not specified and `hostname` is not specified or `hostname` is `localhost`, then the connection will happen through windows named pipes.
- Named pipes will also be used if the `libmysql` / `libmariadb` client library detects that the client doesn't support tcpip.
- In other cases then a tcpip connection through the `port` option is used.

## How to Test Which Protocol is Used

The `status` command shows you information about which protocol is used:

```sql
shell> mysql test

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 10
Server version: 10.2.2-MariaDB-valgrind-max-debug Source distribution

Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [test]> status;
--------------
mysql  Ver 15.1 Distrib 10.0.25-MariaDB, for Linux (x86_64) using readline 5.2

Connection id:          10
Current database:       test
Current user:           monty@localhost
...
Connection:             Localhost via UNIX socket
...
UNIX socket:            /tmp/mysql-dbug.sock
```

## mysql Commands

There are also a number of commands that can be run inside the client. Note that all text commands must be first on line and end with ';'

<table><tbody><tr><th>Command</th><th>Description</th></tr>
<tr><td><code>?</code>, <code>\?</code></td><td>Synonym for `help'.</td></tr>
<tr><td><code>clear</code>, <code>\c</code></td><td>Clear the current input statement.</td></tr>
<tr><td><code>connect</code>, <code>\r</code></td><td>Reconnect to the server. Optional arguments are db and host.</td></tr>
<tr><td><code>delimiter</code>, <code>\d</code></td><td>Set statement delimiter.</td></tr>
<tr><td><code>edit</code>, <code>\e</code></td><td>Edit command with $EDITOR.</td></tr>
<tr><td><code>ego</code>, <code>\G</code></td><td>Send command to mysql server, display result vertically.</td></tr>
<tr><td><code>exit</code>, <code>\q</code></td><td>Exit mysql. Same as quit.</td></tr>
<tr><td><code>go</code>, <code>\g</code></td><td>Send command to mysql server.</td></tr>
<tr><td><code>help</code>, <code>\h</code></td><td>Display this help.</td></tr>
<tr><td><code>nopager</code>, <code>\n</code></td><td>Disable pager, print to stdout.</td></tr>
<tr><td><code>notee</code>, <code>\t</code></td><td>Don't write into outfile.</td></tr>
<tr><td><code>pager</code>, <code>\P</code></td><td>Set PAGER [to_pager]. Print the query results via PAGER.</td></tr>
<tr><td><code>print</code>, <code>\p</code></td><td>Print current command.</td></tr>
<tr><td><code>prompt</code>, <code>\R</code></td><td>Change your mysql prompt. See <a href="#prompt-command">prompt command</a> for options.</td></tr>
<tr><td><code>quit</code>, <code>\q</code></td><td>Quit mysql.</td></tr>
<tr><td><code>rehash</code>, <code>\# </code></td><td>Rebuild completion hash.</td></tr>
<tr><td><code>source</code>, <code>\.</code></td><td>Execute an SQL script file. Takes a file name as an argument.</td></tr>
<tr><td><code>status</code>, <code>\s</code></td><td>Get status information from the server.</td></tr>
<tr><td><code>system</code>, <code>\!</code></td><td>Execute a system shell command. Only works in Unix-like systems.</td></tr>
<tr><td><code>tee</code>, <code>\T</code></td><td>Set outfile [to_outfile]. Append everything into given outfile.</td></tr>
<tr><td><code>use</code>, <code>\u</code></td><td>Use another database. Takes database name as argument.</td></tr>
<tr><td><code>charset</code>, <code>\C</code></td><td>Switch to another charset. Might be needed for processing binlog with multi-byte charsets.</td></tr>
<tr><td><code>warnings</code>, <code>\W</code></td><td>Show warnings after every statement.</td></tr>
<tr><td><code>nowarning</code>, <code>\w</code></td><td>Don't show warnings after every statement.</td></tr>
</tbody></table>

## The mysql_history File

On Unix, the mysql client writes a record of executed statements to a history
file. By default, this file is named `.mysql_history` and is created in your home
directory. To specify a different file, set the value of the MYSQL_HISTFILE
environment variable.

The .mysql_history file should be protected with a restrictive access mode because
sensitive information might be written to it, such as the text of SQL
statements that contain passwords.

If you do not want to maintain a history file, first remove .mysql_history if
it exists, and then use either of the following techniques:

- Set the MYSQL_HISTFILE variable to /dev/null. To cause this setting to take
  effect each time you log in, put the setting in one of your shell's startup
  files.
- Create .mysql_history as a symbolic link to /dev/null:

```sql
shell> ln -s /dev/null $HOME/.mysql_history
```

You need do this only once.

## prompt Command

The prompt command reconfigures the default  prompt `\N [\d]&gt;`. The string for defining the prompt can contain the following special sequences.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>\c</code></td><td>A counter that increments for each statement you issue.</td></tr>
<tr><td><code>\D</code></td><td>The full current date.</td></tr>
<tr><td><code>\d</code></td><td>The default database.</td></tr>
<tr><td><code>\h</code></td><td>The server host.</td></tr>
<tr><td><code>\l</code></td><td>The current delimiter.</td></tr>
<tr><td><code>\m</code></td><td>Minutes of the current time.</td></tr>
<tr><td><code>\n</code></td><td>A newline character.</td></tr>
<tr><td><code>\O</code></td><td>The current month in three-letter format (Jan, Feb, ...).</td></tr>
<tr><td><code>\o</code></td><td>The current month in numeric format.</td></tr>
<tr><td><code>\P</code></td><td>am/pm.</td></tr>
<tr><td><code>\p</code></td><td>The current TCP/IP port or socket file.</td></tr>
<tr><td><code>\R</code></td><td>The current time, in 24-hour military time (0–23).</td></tr>
<tr><td><code>\r</code></td><td>The current time, standard 12-hour time (1–12).</td></tr>
<tr><td><code>\S</code></td><td>Semicolon.</td></tr>
<tr><td><code>\s</code></td><td>Seconds of the current time.</td></tr>
<tr><td><code>\t</code></td><td>A tab character.</td></tr>
<tr><td><code>\U</code></td><td>Your full user_name@host_name account name.</td></tr>
<tr><td><code>\u</code></td><td>Your user name.</td></tr>
<tr><td><code>\v</code></td><td>The server version.</td></tr>
<tr><td><code>\w</code></td><td>The current day of the week in three-letter format (Mon, Tue, ...).</td></tr>
<tr><td><code>\Y</code></td><td>The current year, four digits.</td></tr>
<tr><td><code>\y</code></td><td>The current year, two digits.</td></tr>
<tr><td><code>\_</code></td><td>A space.</td></tr>
<tr><td><code>\</code></td><td>A space (a space follows the backslash).</td></tr>
<tr><td><code>\'</code></td><td>Single quote.</td></tr>
<tr><td><code>\"</code></td><td>Double quote.</td></tr>
<tr><td><code>\ \</code></td><td>A literal “\” backslash character.</td></tr>
<tr><td><code>\x</code></td><td>x, for any “x” not listed above.</td></tr>
</tbody></table>

## mysql Tips

This section describes some techniques that can help you use
<code class="highlight fixed" style="white-space:pre-wrap"><strong>mysql</strong></code> more effectively.

### Displaying Query Results Vertically

Some query results are much more readable when displayed vertically, instead of
in the usual horizontal table format. Queries can be displayed vertically by
terminating the query with \G instead of a semicolon. For example, longer text
values that include newlines often are much easier to read with vertical
output:

```sql
mysql> SELECT * FROM mails WHERE LENGTH(txt) < 300 LIMIT 300,1\G
*************************** 1. row ***************************
  msg_nro: 3068
    date: 2000-03-01 23:29:50
time_zone: +0200
mail_from: Monty
    reply: monty@no.spam.com
  mail_to: "Thimble Smith" <tim@no.spam.com>
      sbj: UTF-8
      txt: >>>>> "Thimble" == Thimble Smith writes:
Thimble> Hi.  I think this is a good idea.  Is anyone familiar
Thimble> with UTF-8 or Unicode? Otherwise, I´ll put this on my
Thimble> TODO list and see what happens.
Yes, please do that.
Regards,
Monty
    file: inbox-jani-1
    hash: 190402944
1 row in set (0.09 sec)
```

### Using the --safe-updates Option

For beginners, a useful startup option is `--safe-updates` (or
`--i-am-a-dummy`, which has the same effect). It is helpful
for cases when you might have issued a 
`DELETE FROM tbl_name` statement but forgotten the
`WHERE` clause.  Normally, such a statement deletes all rows
from the table. With `--safe-updates`, you can delete rows
only by specifying the key values that identify them. This helps prevent
accidents.

When you use the `--safe-updates` option, mysql issues the
following statement when it connects to the MariaDB server:

```sql
SET sql_safe_updates=1, sql_select_limit=1000, sql_max_join_size=1000000;
```

The [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set) statement has the following effects:

- You are not allowed to execute an [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) or [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) statement unless you
  specify a key constraint in the WHERE clause or provide a LIMIT clause (or
  both). For example:

```sql
UPDATE tbl_name SET not_key_column=val WHERE key_column=val;
UPDATE tbl_name SET not_key_column=val LIMIT 1;
```

- The server limits all large`SELECT` results to 1,000 rows
  unless the statement includes a `LIMIT` clause.
- The server aborts multiple-table `SELECT` statements that
  probably need to examine more than 1,000,000 row combinations.

To specify limits different from 1,000 and 1,000,000, you can override the
defaults by using the `--select_limit` and `--max_join_size` options:

```sql
mysql --safe-updates --select_limit=500 --max_join_size=10000
```

### Disabling mysql Auto-Reconnect

If the mysql client loses its connection to the server while sending a
statement, it immediately and automatically tries to reconnect once to the
server and send the statement again. However, even if mysql succeeds in
reconnecting, your first connection has ended and all your previous session
objects and settings are lost: temporary tables, the autocommit mode, and
user-defined and session variables. Also, any current transaction rolls back.
This behavior may be dangerous for you, as in the following example where the
server was shut down and restarted between the first and second statements
without you knowing it:

```sql
mysql> SET @a=1;
Query OK, 0 rows affected (0.05 sec)
mysql> INSERT INTO t VALUES(@a);
ERROR 2006: MySQL server has gone away
No connection. Trying to reconnect...
Connection id:    1
Current database: test
Query OK, 1 row affected (1.30 sec)
mysql> SELECT * FROM t;
+------+
| a    |
+------+
| NULL |
+------+
```

The @a user variable has been lost with the connection, and after the
reconnection it is undefined. If it is important to have mysql terminate with
an error if the connection has been lost, you can start the mysql client with
the `--skip-reconnect` option.

## See Also

- [Troubleshooting Connection Issues](/kb/en/troubleshooting-connection-issues/)
- [Readline commands and configuration](https://docs.freebsd.org/info/readline/readline.pdf)