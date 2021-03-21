# mysqlimport

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-import` is a symlink to `mysqlimport`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadb-import` is the name of the script, with `mysqlimport` a symlink .

`mysqlimport` loads tables from text files in various formats.  The base name
of the text file must be the name of the table that should be used.  If one
uses sockets to connect to the MariaDB server, the server will open and read the
text file directly. In other cases the client will open the text file. The SQL
command [LOAD DATA INFILE](/kb/en/load-data-infile/) is used to import the rows.

## Using mysqlimport

The command to use `mysqlimport` and the general syntax is:

```sql
mysqlimport [OPTIONS] database textfile1 [textfile2 ...]
```

### Options

`mysqlimport` supports the following options:

<table><tbody><tr><th>variable</th><th>Description</th></tr>
<tr><td><code>--character-sets-dir=name</code></td><td>Directory for character set files.</td></tr>
<tr><td><code>-c cols</code>, <code>--columns=cols</code></td><td>Use only these columns to import the data to. Give the column names in a comma separated list. This is same as giving columns to <a href="/kb/en/load-data-infile/">LOAD DATA INFILE</a>.</td></tr>
<tr><td><code>-C</code>, <code>--compress</code></td><td>Use compression in server/client protocol.</td></tr>
<tr><td><code>-# [options] </code>, <code>--debug[=options]</code></td><td>Output debug log. Often this is <code>d:t:o,filename</code>. The default is <code>d:t:o</code>.</td></tr>
<tr><td><code>--debug-check</code></td><td>Check memory and open file usage at exit.</td></tr>
<tr><td><code>--debug-info</code></td><td>Print some debug info at exit.</td></tr>
<tr><td><code>--default-auth=plugin</code></td><td>Default authentication client-side plugin to use.</td></tr>
<tr><td><code>--default-character-set=name</code></td><td>Set the default <a href="/kb/en/data-types-character-sets-and-collations/">character set</a>.</td></tr>
<tr><td><code>--defaults-extra-file=name</code></td><td>Read this file after the global files are read. Must be given as the first option.</td></tr>
<tr><td><code>--defaults-file=name</code></td><td>Only read default options from the given file <em>name</em> Must be given as the first option.</td></tr>
<tr><td><code>--defaults-group-suffix=name</code></td><td>In addition to the given groups, also read groups with this suffix.</td></tr>
<tr><td><code>-d</code>, <code>--delete</code></td><td>First delete all rows from table.</td></tr>
<tr><td><code>--fields-terminated-by=name</code></td><td>Fields in the input file are terminated by the given string.</td></tr>
<tr><td><code>--fields-enclosed-by=name</code></td><td>Fields in the import file are enclosed by the given character.</td></tr>
<tr><td><code>--fields-optionally-enclosed-by=name</code></td><td>Fields in the input file are optionally enclosed by the given character.</td></tr>
<tr><td><code>--fields-escaped-by=name</code></td><td>Fields in the input file are escaped by the given character.</td></tr>
<tr><td><code>-f</code>, <code>--force</code></td><td>Continue even if we get an SQL error.</td></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Displays this help and exits.</td></tr>
<tr><td><code>-h name</code>, <code>--host=name</code></td><td>Connect to host.</td></tr>
<tr><td><code>-i</code>, <code>--ignore</code></td><td>If duplicate unique key was found, keep old row.</td></tr>
<tr><td><code>k</code>, <code>--ignore-foreign-keys</code></td><td>Disable foreign key checks while importing the data. From <a href="/kb/en/mariadb-10316-release-notes/">MariaDB 10.3.16</a>, <a href="/kb/en/what-is-mariadb-1025/">MariaDB 10.25</a> and <a href="/kb/en/mariadb-10141-release-notes/">MariaDB 10.1.41</a>.</td></tr>
<tr><td><code>--ignore-lines=n</code></td><td>Ignore first <em>n</em> lines of data infile.</td></tr>
<tr><td><code>--lines-terminated-by=name</code></td><td>Lines in the input file are terminated by the given string.</td></tr>
<tr><td><code>-L</code>, <code>--local</code></td><td>Read all files through the client.</td></tr>
<tr><td><code>-l</code>, <code>--lock-tables</code></td><td>Lock all tables for write (this disables threads).</td></tr>
<tr><td><code>--low-priority</code></td><td>Use LOW_PRIORITY when updating the table.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file. Must be given as the first option.</td></tr>
<tr><td><code>-p[passwd]</code>, <code>--password[=passwd]</code></td><td>Password to use when connecting to server. If password is not given it's asked from the terminal. Specifying a password on the command line should be considered insecure. You can use an option file to avoid giving the password on the command line.</td></tr>
<tr><td><code>--pipe</code>, <code>-W</code></td><td>On Windows, connect to the server via a named pipe. This option applies only if the server supports named-pipe connections.</td></tr>
<tr><td><code>--plugin-dir</code></td><td>Directory for client-side plugins.</td></tr>
<tr><td><code>-P num</code>, <code>--port=num</code></td><td>Port number to use for connection or 0 for default to, in order of preference, my.cnf, the MYSQL_TCP_PORT <a href="/kb/en/mariadb-environment-variables/">environment variable</a>, /etc/services, built-in default (3306).</td></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit. Must be given as the first option.</td></tr>
<tr><td><code>--protocol=name</code></td><td>The protocol to use for connection (tcp, socket, pipe, memory).</td></tr>
<tr><td><code>-r</code>, <code>--replace</code></td><td>If duplicate unique key was found, replace old row.</td></tr>
<tr><td><code>--shared-memory-base-name</code></td><td>Shared-memory name to use for Windows connections using shared memory to a local server (started with the <code>--shared-memory</code> option). Case-sensitive.</td></tr>
<tr><td><code>-s</code>, <code>--silent</code></td><td>Silent mode. Produce output only when errors occur.</td></tr>
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
<tr><td><code>--tls-version=name</code></td><td>This option accepts a comma-separated list of TLS protocol versions. A TLS protocol version will only be enabled if it is present in this list. All other TLS protocol versions will not be permitted. See <a href="/kb/en/secure-connections-overview/#tls-protocol-versions">Secure Connections Overview: TLS Protocol Versions</a> for more information. This option was added in <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>--use-threads=num</code></td><td>Load files in parallel. The argument is the number of threads to use for loading data.</td></tr>
<tr><td><code>-u name</code>, <code>--user=name</code></td><td>User for login if not current user.</td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>Print info about the various stages.</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Output version information and exit.</td></tr>
</tbody></table>

### Option Files

In addition to reading options from the command-line, `mysqlimport` can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). If an unknown option is provided to `mysqlimport` in an option file, then it is ignored.

The following options relate to how MariaDB command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
</tbody></table>

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, `mysqlimport` is linked with [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/). Therefore, it may be helpful to see [Configuring MariaDB Connector/C with Option Files](/kb/en/configuring-mariadb-connectorc-with-option-files/) for more information on how MariaDB Connector/C handles option files.

#### Option Groups

`mysqlimport` reads options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysqlimport]</code></td><td>&nbsp;Options read by <code>mysqlimport</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-import]</code></td><td>Options read by <code>mysqlimport</code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[client]</code></td><td>&nbsp;Options read by all MariaDB and MySQL <a href="/kb/en/clients-utilities/">client programs</a>, which includes both MariaDB and MySQL clients. For example, <code>mysqldump</code>.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[client-mariadb]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a>.</td></tr>
</tbody></table>

### Default Values

<table><tbody><tr><th>Variables (<code><code>--</code>variable-name=value</code>) and boolean options <code>{FALSE<code>|</code>TRUE}</code></th><th>Value (after reading options)</th></tr>
<tr><td><code>character-sets-dir</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>default-character-set</code></td><td>latin1</td></tr>
<tr><td><code>columns</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>compress</code></td><td>FALSE</td></tr>
<tr><td><code>debug-check</code></td><td>FALSE</td></tr>
<tr><td><code>debug-info</code></td><td>FALSE</td></tr>
<tr><td><code>delete</code></td><td>FALSE</td></tr>
<tr><td><code>fields-terminated-by</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>fields-enclosed-by</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>fields-optionally-enclosed-by</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>fields-escaped-by</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>force</code></td><td>FALSE</td></tr>
<tr><td><code>host</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>ignore</code></td><td>FALSE</td></tr>
<tr><td><code>ignore-lines</code></td><td>0</td></tr>
<tr><td><code>lines-terminated-by</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>local</code></td><td>FALSE</td></tr>
<tr><td><code>lock-tables</code></td><td>FALSE</td></tr>
<tr><td><code>low-priority</code></td><td>FALSE</td></tr>
<tr><td><code>port</code></td><td>3306</td></tr>
<tr><td><code>replace</code></td><td>FALSE</td></tr>
<tr><td><code>silent</code></td><td>FALSE</td></tr>
<tr><td><code>socket</code></td><td>/var/run/mysqld/mysqld.sock</td></tr>
<tr><td><code>ssl</code></td><td>FALSE</td></tr>
<tr><td><code>ssl-ca</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>ssl-capath</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>ssl-cert</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>ssl-cipher</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>ssl-key</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>ssl-verify-server-cert</code></td><td>FALSE</td></tr>
<tr><td><code>use-threads</code></td><td>0</td></tr>
<tr><td><code>user</code></td><td><em>(No default value)</em></td></tr>
<tr><td><code>verbose</code></td><td>FALSE</td></tr>
</tbody></table>