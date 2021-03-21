# mysqlshow

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-show` is a symlink to `mysqlshow`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mysqlshow` is the symlink, and `mariadb-show` the binary name.

Shows the structure of a MariaDB database (databases, tables, columns and indexes). You can also use [SHOW DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases/), [SHOW TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-tables/), [SHOW COLUMNS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-columns/), [SHOW INDEX](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-index/) and [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status/), as well as the [Information Schema](/kb/en/information_schema/) tables ([TABLES](/kb/en/information-schema-tables-table/), [COLUMNS](/kb/en/information-schema-columns-table/), [STATISTICS](/kb/en/information-schema-statistics-table/)), to get similar functionality.

## Using mysqlshow

```sql
mysqlshow [OPTIONS] [database [table [column]]]
```

The output displays only the names of those databases, tables, or columns for which you have some privileges.

If no database is given then all matching databases are shown. If no table is given, then all matching tables in database are shown. If no column is given, then all matching columns and column types in table are shown.

If the last argument contains a shell or SQL wildcard (*,?,% or _) then only
what's matched by the wildcard is shown.  If a database name contains any underscores, those should be escaped with a backslash (some Unix shells require two) to get a list of the proper tables or columns.  “*” and “?”  characters are converted into SQL “%” and “_” wildcard characters. This might cause some confusion when you try to display the columns for a table with a “_” in the name, because in this case, mysqlshow shows you only the table names that match the pattern. This is easily fixed by adding an extra “%” last on the command line as a separate argument.

### Options

`mysqlshow` supports the following options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-c name</code>, <code>--character-sets-dir=name</code></td><td>Directory for <a href="/kb/en/data-types-character-sets-and-collations/">character set</a> files.</td></tr>
<tr><td><code>-C</code>, <code>--compress</code></td><td>Use compression in server/client protocol if both support it.</td></tr>
<tr><td><code>--count</code></td><td>Show number of rows per table (may be slow for non-<a href="/kb/en/myisam/">MyISAM</a> tables).</td></tr>
<tr><td><code>-# [name]</code>, <code>--debug[=name]</code></td><td>Output debug log. Typical is <code>d:t:o,filename</code>, the default is <code>d:t:o</code>.</td></tr>
<tr><td><code>--debug-check</code></td><td>Check memory and open file usage at exit.</td></tr>
<tr><td><code>--debug-info</code></td><td>Print some debug info at exit.</td></tr>
<tr><td><code>--default-auth=name</code></td><td>Default authentication client-side plugin to use.</td></tr>
<tr><td><code>--default-character-set=name</code></td><td>Set the default <a href="/kb/en/data-types-character-sets-and-collations/">character set</a>.</td></tr>
<tr><td><code>--defaults-extra-file=name</code></td><td>Read the file <em>name</em> after the global files are read. Must be given as the first option.</td></tr>
<tr><td><code>--defaults-file=name</code></td><td>Only read default options from the given file <em>name</em>. Must be given as the first option.</td></tr>
<tr><td><code>--defaults-group-suffix=suffix</code></td><td>In addition to the given groups, also read groups with this suffix.</td></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Display help and exit.</td></tr>
<tr><td><code>-h name</code>, <code>--host=name</code></td><td>Connect to the MariaDB server on the given host.</td></tr>
<tr><td><code>-k</code>, <code>--keys</code></td><td>Show indexes for table.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file. Must be given as the first option.</td></tr>
<tr><td><code>-p[password]</code>, <code>--password[=password]</code></td><td>Password to use when connecting to server. If password is not given, it's solicited on the command line. Specifying a password on the command line should be considered insecure. You can use an option file to avoid giving the password on the command line.</td></tr>
<tr><td><code>-W</code>, <code>--pipe</code></td><td>On Windows, connect to the server via a named pipe. This option applies only if the server supports named-pipe connections.</td></tr>
<tr><td><code>--plugin-dir=name</code></td><td>Directory for client-side plugins.</td></tr>
<tr><td><code>-P num</code>, <code>--port=num</code></td><td>Port number to use for connection or 0 for default to, in order of preference, my.cnf, $MYSQL_TCP_PORT, /etc/services, built-in default (3306).</td></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit. Must be given as the first option.</td></tr>
<tr><td><code>--protocol=name</code></td><td>The protocol to use for connection (tcp, socket, pipe, memory).</td></tr>
<tr><td><code>--shared-memory-base-name=name</code></td><td>On Windows, the shared-memory name to use, for connections made using shared memory to a local server. The default value is <code>MYSQL</code>. The shared-memory name is case sensitive. The server must be started with the <code>--shared-memory</code> option to enable shared-memory connections.</td></tr>
<tr><td><code>-t</code>, <code>--show-table-type</code></td><td>Show table type column, as in <a href="/kb/en/show-tables/">SHOW FULL TABLES</a>. The type is BASE TABLE or VIEW.</td></tr>
<tr><td><code>-S name</code>, <code>--socket=name</code></td><td>For connections to localhost, the Unix socket file to use, or, on Windows, the name of the named pipe to use.</td></tr>
<tr><td><code>--ssl</code></td><td>Enables <a href="/kb/en/data-in-transit-encryption/">TLS</a>. TLS is also enabled even without setting this option when certain other TLS options are set. Starting with <a href="/kb/en/what-is-mariadb-102/">MariaDB 10.2</a>, the <code>--ssl</code> option will not enable <a href="/kb/en/secure-connections-overview/#server-certificate-verification">verifying the server certificate</a> by default. In order to verify the server certificate, the user must specify the <code>--ssl-verify-server-cert</code> option.</td></tr>
<tr><td><code>--ssl-ca=name</code></td><td>Defines a path to a PEM file that should contain one or more X509 certificates for trusted Certificate Authorities (CAs) to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. See <a href="/kb/en/secure-connections-overview/#certificate-authorities-cas">Secure Connections Overview: Certificate Authorities (CAs)</a> for more information. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-capath=name</code></td><td>Defines a path to a directory that contains one or more PEM files that should each contain one X509 certificate for a trusted Certificate Authority (CA) to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the <code><a href="https://www.openssl.org/docs/man1.1.1/man1/rehash.html">openssl rehash</a></code> command. See <a href="/kb/en/secure-connections-overview/#certificate-authorities-cas">Secure Connections Overview: Certificate Authorities (CAs)</a> for more information. This option is only supported if the client was built with OpenSSL. If the client was built with yaSSL, GnuTLS, or Schannel, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-cert=name</code></td><td>Defines a path to the X509 certificate file to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-cipher=name</code></td><td>List of permitted ciphers or cipher suites to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-key=name</code></td><td>Defines a path to a private key file to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-crl=name</code></td><td>Defines a path to a PEM file that should contain one or more revoked X509 certificates to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. See <a href="/kb/en/secure-connections-overview/#certificate-revocation-lists-crls">Secure Connections Overview: Certificate Revocation Lists (CRLs)</a> for more information. This option is only supported if the client was built with OpenSSL or Schannel. If the client was built with yaSSL or GnuTLS, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-crlpath=name</code></td><td>Defines a path to a directory that contains one or more PEM files that should each contain one revoked X509 certificate to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the <code><a href="https://www.openssl.org/docs/man1.1.1/man1/rehash.html">openssl rehash</a></code> command. See <a href="/kb/en/secure-connections-overview/#certificate-revocation-lists-crls">Secure Connections Overview: Certificate Revocation Lists (CRLs)</a> for more information. This option is only supported if the client was built with OpenSSL. If the client was built with yaSSL, GnuTLS, or Schannel, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-verify-server-cert</code></td><td>Enables (or disables) <a href="/kb/en/secure-connections-overview/#server-certificate-verification">server certificate verification</a>. This option is disabled by default.</td></tr>
<tr><td><code>-i</code>, <code>--status</code></td><td>Shows a lot of extra information about each table. See the  <a href="/kb/en/information-schema-tables-table/">INFORMATION_SCHEMA.TABLES</a> table for more details on the returned information.</td></tr>
<tr><td><code>--tls-version=name</code></td><td>This option accepts a comma-separated list of TLS protocol versions. A TLS protocol version will only be enabled if it is present in this list. All other TLS protocol versions will not be permitted. See <a href="/kb/en/secure-connections-overview/#tls-protocol-versions">Secure Connections Overview: TLS Protocol Versions</a> for more information. This option was added in <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>-u</code>, <code>--user=name</code></td><td>User for login if not current user.</td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>More verbose output; you can use this multiple times to get even more verbose output.</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Output version information and exit.</td></tr>
</tbody></table>

### Option Files

In addition to reading options from the command-line, `mysqlshow` can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). If an unknown option is provided to `mysqlshow` in an option file, then it is ignored.

The following options relate to how MariaDB command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
<tr><td><code>--defaults-group-suffix=# </code></td><td>In addition to the default option groups, also read option groups with this suffix.</td></tr>
</tbody></table>

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, `mysqlshow` is linked with [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/). However, MariaDB Connector/C does not yet handle the parsing of option files for this client. That is still performed by the server option file parsing code. See  [MDEV-19035](https://jira.mariadb.org/browse/MDEV-19035) for more information.

#### Option Groups

`mysqlshow` reads options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysqlshow]</code></td><td>&nbsp;Options read by <code>mysqlshow</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-show]</code></td><td>Options read by <code>mysqlshow</code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[client]</code></td><td>&nbsp;Options read by all MariaDB and MySQL <a href="/kb/en/clients-utilities/">client programs</a>, which includes both MariaDB and MySQL clients. For example, <code>mysqldump</code>.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[client-mariadb]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a>.</td></tr>
</tbody></table>

## Examples

Getting a list of databases:

```sql
bin/mysqlshow
+--------------------+
|     Databases      |
+--------------------+
| information_schema |
| test               |
+--------------------+
```

Getting a list of tables in the `test` database:

```sql
bin/mysqlshow test
Database: test
+---------+
| Tables  |
+---------+
| author  |
| book    |
| city    |
| country |
+---------+
```

Getting a list of columns in the `test`.`book` table:

```sql
bin/mysqlshow test book
Database: test  Table: book
+-----------+-----------------------+-------------------+------+-----+---------+----------------+--------------------------------+---------+
| Field     | Type                  | Collation         | Null | Key | Default | Extra          | Privileges                      | Comment |
+-----------+-----------------------+-------------------+------+-----+---------+----------------+--------------------------------+---------+
| id        | mediumint(8) unsigned |                   | NO   | PRI |         | auto_increment | select,insert,update,references |         |
| title     | varchar(200)          | latin1_swedish_ci | NO   |     |         |                | select,insert,update,references |         |
| author_id | smallint(5) unsigned  |                   | NO   | MUL |         |                | select,insert,update,references |         |
+-----------+-----------------------+-------------------+------+-----+---------+----------------+--------------------------------+---------+

```