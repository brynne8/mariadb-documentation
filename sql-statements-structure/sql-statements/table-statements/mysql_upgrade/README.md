# mysql_upgrade

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-upgrade` is a symlink to `mysql_upgrade`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mysql_upgrade` is the symlink, and `mariadb-upgrade` the binary name.

`mysql_upgrade` is a tool that checks and updates your tables to the latest version.

You should run `mysql_upgrade` after upgrading from one major MySQL/MariaDB release to another, such as [from MySQL 5.0 to MariaDB 10.4](/kb/en/upgrading-to-mariadb-from-mysql/) or [MariaDB 10.4](/kb/en/what-is-mariadb-104/) to [MariaDB 10.5](/kb/en/what-is-mariadb-105/). You also have to use `mysql_upgrade` after a direct "horizontal" migration, for example from MySQL 5.5.40 to [MariaDB 5.5.40](/kb/en/mariadb-5540-release-notes/). It's also safe to run `mysql_upgrade` for minor upgrades, as if there are no incompatibles between versions it changes nothing.

It needs to be run as a user with write access to the data directory.

`mysql_upgrade` is run after starting the new MariaDB server.  Running it before you shut down the old version will not hurt anything and will allow you to make sure it works and figure out authentication for it ahead of time.

It is recommended to make a [backup](/kb/en/backing-up-and-restoring/) of all the databases before running `mysql_upgrade`.

In most cases, `mysql_upgrade` should just take a few seconds.  The main work of `mysql_upgrade` is to:

- Update the system tables in the `mysql` database to the latest version (normally just add new fields to a few tables).
- Check that all tables are up to date (runs [CHECK TABLE table_name FOR UPGRADE](/kb/en/sql-commands-check-table/)). For tables that are not up to date, runs [ALTER TABLE table_name FORCE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) on the table to update it. A table is not up to date if:
<ul start="1"><li>The table uses an index for which there has been a [collation](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets) change (rare)
</li><li>A format change in the storage engine requires an update (very rare)
</li></ul>

## Using mysql_upgrade

```sql
mysql_upgrade [--force] [--user=# --password 
  --host=hostname --port=# --socket=#
  --protocol=tcp|socket|pipe|memory 
  --verbose] OTHER_OPTIONS]
```

`mysql_upgrade` is mainly a framework to call [mysqlcheck](/sql-statements-structure/sql-statements/table-statements/mysqlcheck). `mysql_upgrade` works by doing the following operations:

```sql
# Find out path to datadir
echo "show show variables like 'datadir'" | mysql
mysqlcheck --no-defaults --check-upgrade --auto-repair --databases mysql
mysql_fix_privilege_tables
mysqlcheck --no-defaults --all-databases --fix-db-names --fix-table-names
mysqlcheck --no-defaults --check-upgrade --all-databases --auto-repair
```

The connect options given to `mysql_upgrade` are passed along to [mysqlcheck](/sql-statements-structure/sql-statements/table-statements/mysqlcheck) and [mysql](/clients-utilities/mysql-client/mysql-command-line-client).

The `mysql_fix_privilege_tables` script is not actually called; it's included as part of `mysql_upgrade`

If you have a problem with `mysql_upgrade` try run it in very verbose mode:

```sql
mysql_upgrade --verbose --verbose other-options
```

`mysql_upgrade` also saves the MariaDB version number in a file named `mysql_upgrade_info` in the [data directory](/kb/en/server-system-variables/#datadir). This is used to quickly check whether all tables have been checked for this release so that table-checking can be skipped. For this reason, 
`mysql_upgrade` needs to be run as a user with write access to the data directory. To ignore this file and perform the check regardless, use the `--force` option.

### Options

`mysql_upgrade` supports the following options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Display this help message and exit.</td></tr>
<tr><td><code>--basedir=path</code></td><td>Old option accepted for backward compatibility but ignored.</td></tr>
<tr><td><code>--character-sets-dir=path</code></td><td>Old option accepted for backward compatibility but ignored.</td></tr>
<tr><td><code>--compress=name</code></td><td>Old option accepted for backward compatibility but ignored.</td></tr>
<tr><td><code>--datadir=name</code></td><td>Old option accepted for backward compatibility but ignored.</td></tr>
<tr><td><code>-# [name]</code>, <code>--debug[=name]</code></td><td>For debug builds, output debug log.</td></tr>
<tr><td><code>--debug-check</code></td><td>Check memory and open file usage at exit.</td></tr>
<tr><td><code>-T</code>, <code>--debug-info</code></td><td>Print some debug info at exit.</td></tr>
<tr><td><code>--default-character-set=name</code></td><td>Old option accepted for backward compatibility but ignored.</td></tr>
<tr><td><code>-f</code>, <code>--force</code></td><td>Force execution of mysqlcheck even if <code>mysql_upgrade</code> has already been executed for the current version of MariaDB. Ignores <code>mysql_upgrade_info</code>.</td></tr>
<tr><td><code>-h</code>, <code>--host=name</code></td><td>Connect to MariaDB on the given host.</td></tr>
<tr><td><code>-p</code>, <code>--password[=name]</code></td><td>Password to use when connecting to server. If password is not given, it's solicited on the command line (which should be considered insecure). You can use an option file to avoid giving the password on the command line.</td></tr>
<tr><td><code>-P</code>, <code>--port=name</code></td><td>Port number to use for connection or 0 for default to, in order of preference, my.cnf, the MYSQL_TCP_PORT <a href="/kb/en/mariadb-environment-variables/">environment variable</a>, /etc/services, built-in default (3306).</td></tr>
<tr><td><code>--protocol=name</code></td><td>The protocol to use for connection (tcp, socket, pipe, memory).</td></tr>
<tr><td><code>--silent</code></td><td>Print less information.</td></tr>
<tr><td><code>-S</code>, <code>--socket=name</code></td><td>For connections to localhost, the Unix socket file to use, or, on Windows, the name of the named pipe to use.</td></tr>
<tr><td><code>--ssl</code></td><td>Enables <a href="/kb/en/data-in-transit-encryption/">TLS</a>. TLS is also enabled even without setting this option when certain other TLS options are set. Starting with <a href="/kb/en/what-is-mariadb-102/">MariaDB 10.2</a>, the <code>--ssl</code> option will not enable <a href="/kb/en/secure-connections-overview/#server-certificate-verification">verifying the server certificate</a> by default. In order to verify the server certificate, the user must specify the <code>--ssl-verify-server-cert</code> option.</td></tr>
<tr><td><code>--ssl-ca=name</code></td><td>Defines a path to a PEM file that should contain one or more X509 certificates for trusted Certificate Authorities (CAs) to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. See <a href="/kb/en/secure-connections-overview/#certificate-authorities-cas">Secure Connections Overview: Certificate Authorities (CAs)</a> for more information. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-capath=name</code></td><td>Defines a path to a directory that contains one or more PEM files that should each contain one X509 certificate for a trusted Certificate Authority (CA) to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the <a href="https://www.openssl.org/docs/man1.1.1/man1/rehash.html">openssl rehash</a> command. See <a href="/kb/en/secure-connections-overview/#certificate-authorities-cas">Secure Connections Overview: Certificate Authorities (CAs)</a> for more information. This option is only supported if the client was built with OpenSSL or yaSSL. If the client was built with GnuTLS or Schannel, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-cert=name</code></td><td>Defines a path to the X509 certificate file to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-cipher=name</code></td><td>List of permitted ciphers or cipher suites to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-crl=name</code></td><td>Defines a path to a PEM file that should contain one or more revoked X509 certificates to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. See <a href="/kb/en/secure-connections-overview/#certificate-revocation-lists-crls">Secure Connections Overview: Certificate Revocation Lists (CRLs)</a> for more information. This option is only supported if the client was built with OpenSSL or Schannel. If the client was built with yaSSL or GnuTLS, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms.</td></tr>
<tr><td><code>--ssl-crlpath=name</code></td><td>Defines a path to a directory that contains one or more PEM files that should each contain one revoked X509 certificate to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the <a href="https://www.openssl.org/docs/man1.1.1/man1/rehash.html">openssl rehash</a> command. See <a href="/kb/en/secure-connections-overview/#certificate-revocation-lists-crls">Secure Connections Overview: Certificate Revocation Lists (CRLs)</a> for more information. This option is only supported if the client was built with OpenSSL. If the client was built with yaSSL, GnuTLS, or Schannel, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms.</td></tr>
<tr><td><code>--ssl-key=name</code></td><td>Defines a path to a private key file to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-verify-server-cert</code></td><td>Enables <a href="/kb/en/secure-connections-overview/#server-certificate-verification">server certificate verification</a>. This option is disabled by default.</td></tr>
<tr><td><code>-t</code>, <code>--tmpdir=name</code></td><td>Directory for temporary files.</td></tr>
<tr><td><code>-s</code>, <code>--upgrade-system-tables</code></td><td>Only upgrade the system tables in the mysql database. Tables in other databases are not checked or touched.</td></tr>
<tr><td><code>-u</code>, <code>--user=name</code></td><td>User for login if not current user.</td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>Display more output about the process, using it twice will print connection arguments; using it 3 times will print out all <a href="/kb/en/check-table/">CHECK</a>, <a href="/kb/en/rename-table/">RENAME</a> and <a href="/kb/en/alter-table/">ALTER TABLE</a> commands used during the check phase; using it 4 times (added in <a href="/kb/en/mariadb-10014-release-notes/">MariaDB 10.0.14</a>) will also write out all <a href="/kb/en/mysqlcheck/">mysqlcheck</a> commands used.</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Output version information and exit.</td></tr>
<tr><td><code>-k</code>, <code>--version-check</code></td><td>Run this program only if its 'server version' matches the version of the server to which it's connecting check. Note: the 'server version' of the program is the version of the MariaDB server with which it was built/distributed.  (Defaults to on; use <code>--skip-version-check</code> to disable.)</td></tr>
<tr><td><code>--write-binlog</code></td><td>All commands including those run by <a href="/kb/en/mysqlcheck/">mysqlcheck</a> are written to the <a href="/kb/en/binary-log/">binary log</a>. Disabled by default. Before <a href="/kb/en/mariadb-1006-release-notes/">MariaDB 10.0.6</a> and <a href="/kb/en/mariadb-5534-release-notes/">MariaDB 5.5.34</a>, this was enabled by default, and <code>--skip-write-binlog</code> should be used when commands should not be sent to replication slaves.</td></tr>
</tbody></table>

### Option Files

In addition to reading options from the command-line, `mysql_upgrade` can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). If an unknown option is provided to `mysql_upgrade` in an option file, then it is ignored.

The following options relate to how MariaDB command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
<tr><td><code>--defaults-group-suffix=# </code></td><td>In addition to the default option groups, also read option groups with this suffix.</td></tr>
</tbody></table>

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, `mysql_upgrade` is linked with [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/). However, MariaDB Connector/C does not yet handle the parsing of option files for this client. That is still performed by the server option file parsing code. See  [MDEV-19035](https://jira.mariadb.org/browse/MDEV-19035) for more information.

#### Option Groups

`mysql_upgrade` reads options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysql_upgrade]</code></td><td>&nbsp;Options read by <code>mysql_upgrade</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-upgrade]</code></td><td>Options read by <code>mysql_upgrade</code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[client]</code></td><td>&nbsp;Options read by all MariaDB and MySQL <a href="/kb/en/clients-utilities/">client programs</a>, which includes both MariaDB and MySQL clients. For example, <code>mysqldump</code>.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[client-mariadb]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a>.</td></tr>
</tbody></table>

## Differences Between mysql_upgrade in MariaDB and MySQL

This is as of [MariaDB 5.1.50](/kb/en/mariadb-5150-release-notes/):

- MariaDB will convert long [table names](/sql-statements-structure/sql-language-structure/identifier-names) properly.
- MariaDB will convert [InnoDB](/kb/en/xtradb-and-innodb/) tables (no need to do a dump/restore or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table)).
- MariaDB will convert old archive tables to the new 5.1 format.
- "mysql_upgrade --verbose" will run "mysqlcheck --verbose" so that you get more information of what is happening.  Running with 3 times --verbose will in [MariaDB 10.0](/kb/en/what-is-mariadb-100/) print out all CHECK, RENAME and ALTER TABLE commands executed.
- The [mysql.event table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlevent-table) is upgraded live; no need to restart the server to use events if the event table has changed ([MariaDB 10.0.22](/kb/en/mariadb-10022-release-notes/) and [MariaDB 10.1.9](/kb/en/mariadb-1019-release-notes/)).
- More descriptive output.

## Speeding Up mysql_upgrade

- If you are sure that all your tables are up to date with the current version, then you can run `mysql_upgrade ---upgrade-system-tables`, which will only fix your system tables in the mysql database to be compatible with the latest version.

The main reason to run `mysql_upgrade` on all your tables is to allow it to check that:

- There has not been any change in table formats between versions.
<ul start="1"><li>This has not happened since [MariaDB 5.1](/kb/en/what-is-mariadb-51/).
</li></ul>
- If some of the tables are using an index for which we have changed sort order.
<ul start="1"><li>This has not happened since [MariaDB 5.5](/kb/en/what-is-mariadb-55/).
</li></ul>

If you are 100% sure this applies to you, you can just run `mysql_upgrade` with the `---upgrade-system-tables` option.

## Symptoms of Not Having Run mysql_upgrade When It Was Needed

- Errors in the [error log](/mariadb-administration/server-monitoring-logs/error-log) that some system tables don't have all needed columns.
- Updates or searches may not find the record they are attempting to update or search for.
- [CHECKSUM TABLE](/sql-statements-structure/sql-statements/table-statements/checksum-table) may report the wrong checksum for [MyISAM](/kb/en/myisam/) or [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) tables.

To fix issues like this, run `mysql_upgrade`, [mysqlcheck](/sql-statements-structure/sql-statements/table-statements/mysqlcheck),  [CHECK TABLE](/kb/en/sql-commands-check-table/) and if needed [REPAIR TABLE](/sql-statements-structure/sql-statements/table-statements/repair-table) on the wrong table.

## Other Uses

- `mysql_upgrade` will re-create any missing tables in the [mysql database](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables).  It will not touch any data in existing tables.

## See Also

- [mysqlcheck](/sql-statements-structure/sql-statements/table-statements/mysqlcheck)
- [CHECK TABLE](/kb/en/sql-commands-check-table/)
- [REPAIR TABLE](/sql-statements-structure/sql-statements/table-statements/repair-table)