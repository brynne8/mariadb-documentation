# mysqlcheck

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-check` is a symlink to `mysqlcheck`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadb-check` is the name of the tool, with `mysqlcheck` a symlink .

`mysqlcheck` is a maintenance tool that allows you to check, repair, analyze and optimize multiple tables from the command line.

It is essentially a commandline interface to the [CHECK TABLE](/sql-statements-structure/sql-statements/table-statements/check-table/), [REPAIR TABLE](/sql-statements-structure/sql-statements/table-statements/repair-table/), [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) and [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/) commands, and so, unlike [myisamchk](/clients-utilities/myisam-clients-and-utilities/myisamchk/) and [aria_chk](/clients-utilities/aria-clients-and-utilities/aria_chk/), requires the server to be running.

This tool does not work with partitioned tables.

## Using mysqlcheck

```sql
./client/mysqlcheck [OPTIONS] database [tables]
```

OR

```sql
./client/mysqlcheck [OPTIONS] --databases DB1 [DB2 DB3...]
```

OR

```sql
./client/mysqlcheck [OPTIONS] --all-databases
```

`mysqlcheck` can be used to CHECK (-c, -m, -C), REPAIR (-r), ANALYZE (-a),
or OPTIMIZE (-o) tables. Some of the options (like -e or -q) can be
used at the same time. Not all options are supported by all storage engines.

The -c, -r, -a and -o options are exclusive to each other.

The option `--check` will be used by default, if no other options were specified.
You can change the default behavior by making a symbolic link to the binary, or
copying it somewhere with another name, the alternatives are:

<table><tbody><tr><td><code>mysqlrepair</code></td><td>The default option will be <code>-r</code> (<code>--repair</code>)</td></tr>
<tr><td><code>mysqlanalyze</code></td><td>The default option will be <code>-a</code> (<code>--analyze</code>)</td></tr>
<tr><td><code>mysqloptimize</code></td><td>The default option will be <code>-o</code> (<code>--optimize</code>)</td></tr>
</tbody></table>

### Options

`mysqlcheck` supports the following options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-A</code>, <code>--all-databases</code></td><td>Check all the databases. This is the same as <code>--databases</code> with all databases selected.</td></tr>
<tr><td><code>-1</code>, <code>--all-in-1</code></td><td>Instead of issuing one query for each table, use one query per database, naming all tables in the database in a comma-separated list.</td></tr>
<tr><td><code>-a</code>, <code>--analyze</code></td><td>Analyze given tables.</td></tr>
<tr><td><code>--auto-repair</code></td><td>If a checked table is corrupted, automatically fix it. Repairing will be done after all tables have been checked.</td></tr>
<tr><td><code>--character-sets-dir=name</code></td><td>Directory where <a href="/kb/en/data-types-character-sets-and-collations/">character set</a> files are installed.</td></tr>
<tr><td><code>-c</code>, <code>--check</code></td><td>Check table for errors.</td></tr>
<tr><td><code>-C</code>, <code>--check-only-changed</code></td><td>Check only tables that have changed since last check or haven't been closed properly.</td></tr>
<tr><td><code>-g</code>, <code>--check-upgrade</code></td><td>Check tables for version-dependent changes. May be used with <code>--auto-repair</code> to correct tables requiring version-dependent updates. Automatically enables the <code>--fix-db-names</code> and <code>--fix-table-names</code> options. Used <a href="/kb/en/upgrading-to-mariadb-from-mysql/">when upgrading</a></td></tr>
<tr><td><code>--compress</code></td><td>Compress all information sent between the client and server if both support compression.</td></tr>
<tr><td><code>-B</code>, <code>--databases</code></td><td>Check several databases. Note that normally <em>mysqlcheck</em> treats the first argument as a database name, and following arguments as table names. With this option, no tables are given, and all name arguments are regarded as database names.</td></tr>
<tr><td><code>-# </code>, <code>--debug[=name]</code></td><td>Output debug log. Often this is 'd:t:o,filename'.</td></tr>
<tr><td><code>--debug-check</code></td><td>Check memory and open file usage at exit.</td></tr>
<tr><td><code>--debug-info</code></td><td>Print some debug info at exit.</td></tr>
<tr><td><code>--default-auth=plugin</code></td><td>Default authentication client-side plugin to use.</td></tr>
<tr><td><code>--default-character-set=name</code></td><td>Set the default <a href="/kb/en/data-types-character-sets-and-collations/">character set</a>.</td></tr>
<tr><td><code>-e</code>, <code>--extended</code></td><td>If you are using this option with <code>--check</code>, it will ensure that the table is 100 percent consistent, but will take a long time. If you are using this option with <code>--repair</code>, it will force using the old, slow, repair with keycache method, instead of the much faster repair by sorting.</td></tr>
<tr><td><code>-F</code>, <code>--fast</code></td><td>Check only tables that haven't been closed properly.</td></tr>
<tr><td><code>--fix-db-names</code></td><td>Convert database names to the format used since MySQL 5.1. Only database names that contain special characters are affected. Used <a href="/kb/en/upgrading-to-mariadb-from-mysql/">when upgrading</a> from an old MySQL version.</td></tr>
<tr><td><code>--fix-table-names</code></td><td>Convert table names (including <a href="/kb/en/views/">views</a>) to the format used since MySQL 5.1. Only table names that contain special characters are affected. Used <a href="/kb/en/upgrading-to-mariadb-from-mysql/">when upgrading</a> from an old MySQL version.</td></tr>
<tr><td><code>--flush</code></td><td>Flush each table after check. This is useful if you don't want to have the checked tables take up space in the caches after the check.</td></tr>
<tr><td><code>-f</code>, <code>--force</code></td><td>Continue even if we get an SQL error.</td></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Display this help message and exit.</td></tr>
<tr><td><code>-h name</code>, <code>--host=name</code></td><td>Connect to the given host.</td></tr>
<tr><td><code>-m</code>, <code>--medium-check</code></td><td>Faster than extended-check, but only finds 99.99 percent of all errors. Should be good enough for most cases.</td></tr>
<tr><td><code>-o</code>, <code>--optimize</code></td><td>Optimize tables.</td></tr>
<tr><td><code>-p</code>, <code>--password[=name]</code></td><td>Password to use when connecting to the server. If you use the short option form (<code>-p</code>), you cannot have a space between the option and the password. If you omit the password value following the <code>--password</code> or <code>-p</code> option on the command line, mysqlcheck prompts for one. Specifying a password on the command line should be considered insecure. You can use an option file to avoid giving the password on the command line.</td></tr>
<tr><td><code>-Z</code>, <code>--persistent</code></td><td>When using ANALYZE TABLE (<code>--analyze</code>), uses the PERSISTENT FOR ALL option, which forces <a href="/kb/en/engine-independent-table-statistics/">Engine-independent Statistics</a> for this table to be updated. Added in <a href="/kb/en/mariadb-10110-release-notes/">MariaDB 10.1.10</a></td></tr>
<tr><td><code>-W</code>, <code>--pipe</code></td><td>On Windows, connect to the server via a named pipe. This option applies only if the server supports named-pipe connections.</td></tr>
<tr><td><code>--plugin-dir</code></td><td>Directory for client-side plugins.</td></tr>
<tr><td><code>-P num</code>, <code>--port=num</code></td><td>Port number to use for connection or 0 for default to, in order of preference, my.cnf, $MYSQL_TCP_PORT, /etc/services, built-in default (3306).</td></tr>
<tr><td><code>--process-tables</code></td><td>Perform the requested operation (check, repair, analyze, optimize) on tables. Enabled by default. Use  <code>--skip-process-tables</code> to disable. Added in <a href="/kb/en/mariadb-10018-release-notes/">MariaDB 10.0.18</a> and <a href="/kb/en/mariadb-5543-release-notes/">MariaDB 5.5.43</a>.</td></tr>
<tr><td><code>--process-views[=val]</code></td><td>Perform the requested operation (only <a href="/kb/en/check-view/">CHECK VIEW</a> or <a href="/kb/en/repair-view/">REPAIR VIEW</a>). Possible values are NO, YES (correct the checksum, if necessary, add the mariadb-version field), UPGRADE_FROM_MYSQL (same as YES and toggle the algorithm MERGE&lt;-&gt;TEMPTABLE. Added in <a href="/kb/en/mariadb-10018-release-notes/">MariaDB 10.0.18</a> and <a href="/kb/en/mariadb-5543-release-notes/">MariaDB 5.5.43</a>.</td></tr>
<tr><td><code>--protocol=name</code></td><td>The connection protocol (tcp, socket, pipe, memory) to use for connecting to the server. Useful when other connection parameters would cause a protocol to be used other than the one you want.</td></tr>
<tr><td><code>-q</code>, <code>--quick</code></td><td>If you are using this option with CHECK TABLE, it prevents the check from scanning the rows to check for wrong links. This is the fastest check. If you are using this option with REPAIR TABLE, it will try to repair only the index tree. This is the fastest repair method for a table.</td></tr>
<tr><td><code>-r</code>, <code>--repair</code></td><td>Can fix almost anything except unique keys that aren't unique.</td></tr>
<tr><td><code>--shared-memory-base-name</code></td><td>Shared-memory name to use for Windows connections using shared memory to a local server (started with the <code>--shared-memory</code> option). Case-sensitive.</td></tr>
<tr><td><code>-s</code>, <code>--silent</code></td><td>Print only error messages.</td></tr>
<tr><td><code>--skip-database</code></td><td>Don't process the database (case-sensitive) specified as argument.</td></tr>
<tr><td><code>-S name</code>, <code>--socket=name</code></td><td>For connections to localhost, the Unix socket file to use, or, on Windows, the name of the named pipe to use.</td></tr>
<tr><td><code>--ssl</code></td><td>Enables <a href="/kb/en/data-in-transit-encryption/">TLS</a>. TLS is also enabled even without setting this option when certain other TLS options are set. Starting with <a href="/kb/en/what-is-mariadb-102/">MariaDB 10.2</a>, the <code>--ssl</code> option will not enable <a href="/kb/en/secure-connections-overview/#server-certificate-verification">verifying the server certificate</a> by default. In order to verify the server certificate, the user must specify the <code>--ssl-verify-server-cert</code> option.</td></tr>
<tr><td><code>--ssl-ca=name</code></td><td>Defines a path to a PEM file that should contain one or more X509 certificates for trusted Certificate Authorities (CAs) to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. See <a href="/kb/en/secure-connections-overview/#certificate-authorities-cas">Secure Connections Overview: Certificate Authorities (CAs)</a> for more information. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-capath=name</code></td><td>Defines a path to a directory that contains one or more PEM files that should each contain one X509 certificate for a trusted Certificate Authority (CA) to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the <code><a href="https://www.openssl.org/docs/man1.1.1/man1/rehash.html">openssl rehash</a></code> command. See <a href="/kb/en/secure-connections-overview/#certificate-authorities-cas">Secure Connections Overview: Certificate Authorities (CAs)</a> for more information. This option is only supported if the client was built with OpenSSL or yaSSL. If the client was built with GnuTLS or Schannel, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-cert=name</code></td><td>Defines a path to the X509 certificate file to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-cipher=name</code></td><td>List of permitted ciphers or cipher suites to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-crl=name</code></td><td>Defines a path to a PEM file that should contain one or more revoked X509 certificates to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. See <a href="/kb/en/secure-connections-overview/#certificate-revocation-lists-crls">Secure Connections Overview: Certificate Revocation Lists (CRLs)</a> for more information. This option is only supported if the client was built with OpenSSL or Schannel. If the client was built with yaSSL or GnuTLS, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms.</td></tr>
<tr><td><code>--ssl-crlpath=name</code></td><td>Defines a path to a directory that contains one or more PEM files that should each contain one revoked X509 certificate to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the <code><a href="https://www.openssl.org/docs/man1.1.1/man1/rehash.html">openssl rehash</a></code> command. See <a href="/kb/en/secure-connections-overview/#certificate-revocation-lists-crls">Secure Connections Overview: Certificate Revocation Lists (CRLs)</a> for more information. This option is only supported if the client was built with OpenSSL. If the client was built with yaSSL, GnuTLS, or Schannel, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms.</td></tr>
<tr><td><code>--ssl-key=name</code></td><td>Defines a path to a private key file to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-verify-server-cert</code></td><td>Enables <a href="/kb/en/secure-connections-overview/#server-certificate-verification">server certificate verification</a>. This option is disabled by default.</td></tr>
<tr><td><code>--tables</code></td><td>Overrides the <code>--databases</code> or <code>-B</code> option such that all name arguments following the option are regarded as table names.</td></tr>
<tr><td><code>--use-frm</code></td><td>For repair operations on MyISAM tables, get table structure from .frm file, so the table can be repaired even if the .MYI header is corrupted.</td></tr>
<tr><td><code>-u</code>, <code>--user=name</code></td><td>User for login if not current user.</td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>Print info about the various stages. You can give this option several times to get even more information. See <a href="#mysqlcheck-and-verbose">mysqlcheck and verbose</a>, below.</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Output version information and exit.</td></tr>
<tr><td><code>--write-binlog</code></td><td>Write ANALYZE, OPTIMIZE and REPAIR TABLE commands to the <a href="/kb/en/binary-log/">binary log</a>. Enabled by default; use <code>--skip-write-binlog</code> when commands should not be sent to replication slaves.</td></tr>
</tbody></table>

### Option Files

In addition to reading options from the command-line, `mysqlcheck` can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). If an unknown option is provided to `mysqlcheck` in an option file, then it is ignored.

The following options relate to how MariaDB command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
<tr><td><code>--defaults-group-suffix=# </code></td><td>In addition to the default option groups, also read option groups with this suffix.</td></tr>
</tbody></table>

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, `mysqlcheck` is linked with [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/). However, MariaDB Connector/C does not yet handle the parsing of option files for this client. That is still performed by the server option file parsing code. See  [MDEV-19035](https://jira.mariadb.org/browse/MDEV-19035) for more information.

#### Option Groups

`mysqlcheck` reads options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysqlcheck]</code></td><td>&nbsp;Options read by <code>mysqlcheck</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-check]</code></td><td>Options read by <code>mysqlcheck</code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[client]</code></td><td>&nbsp;Options read by all MariaDB and MySQL <a href="/kb/en/clients-utilities/">client programs</a>, which includes both MariaDB and MySQL clients. For example, <code>mysqldump</code>.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[client-mariadb]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a>.</td></tr>
</tbody></table>

## Notes

### Default Values

To see the default values for the options and also to see the arguments you get
from configuration files you can do:

```sql
./client/mysqlcheck --print-defaults
./client/mysqlcheck --help
```

### mysqlcheck and auto-repair

When running `mysqlcheck` with `--auto-repair` (as done by
[mysql_upgrade](/kb/en/upgrading-to-mariadb-from-mysql/)), `mysqlcheck` will first
check all tables and then in a separate phase repair those that failed the
check.

### mysqlcheck and all-databases

`mysqlcheck --all-databases` will ignore the internal log tables [general_log](/kb/en/mysqlgeneral_log-table/) and [slow_log](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlslow_log-table/) as these can't be checked, repaired or optimized.

### mysqlcheck and verbose

Using one `--verbose` option will give you more information about what mysqlcheck is doing.

Using two `--verbose` options will also give you connection information.

##### MariaDB starting with [10.0.14](/kb/en/mariadb-10014-release-notes/)

If you use three `--verbose` options you will also get, on stdout, all [ALTER](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/), [RENAME](/sql-statements-structure/sql-statements/data-definition/rename-table/), and [CHECK](/sql-statements-structure/sql-statements/table-statements/check-table/) commands that mysqlcheck executes.