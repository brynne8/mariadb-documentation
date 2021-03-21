# Configuring MariaDB with Option Files

You can configure MariaDB to run the way you want by configuring the server with MariaDB's option files. The default MariaDB option file is called `my.cnf` on Unix-like operating systems and `my.ini` on Windows. Depending on how you've [installed](/mariadb-administration/getting-installing-and-upgrading-mariadb/) MariaDB, the default option file may be in a number of places, or it may not exist at all.

## Global Options Related to Option Files

The following options relate to how MariaDB handles option files. These options can be used with most of MariaDB's command-line tools, not just [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/). They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-print-defaults">--print-defaults</a></code></td><td>Read options from option files, print all option values, and then exit the program.</td></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-no-defaults">--no-defaults</a></code></td><td>Don't read options from any option file.</td></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-defaults-file">--defaults-file</a></code> <code>=path</code></td><td>Only read options from the given option file.</td></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-defaults-extra-file">--defaults-extra-file</a></code> <code>=path</code></td><td>Read this extra option file after all other option files are read.</td></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-defaults-group-suffix">--defaults-group-suffix</a></code> <code>=suffix</code></td><td>In addition to the default option groups, also read option groups with the given suffix.</td></tr>
</tbody></table>

## Default Option File Locations

MariaDB reads option files from many different directories by default. See the sections below to find out which directories are checked for which system.

For an exact list of option files read on your system by a specific program, you can execute:

```sql
$program --help --verbose
```

For example:

```sql
$ mysqld --help --verbose
mysqld  Ver 10.3.13-MariaDB-log for Linux on x86_64 (MariaDB Server)
Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Starts the MariaDB database server.

Usage: mysqld [OPTIONS]

Default options are read from the following files in the given order:
/etc/my.cnf ~/.my.cnf
The following groups are read: mysqld server mysqld-10.3 mariadb mariadb-10.3 client-server galera
....
```

The option files are each scanned once, in the order given by `--help --verbose`. The effect of the configuration options are as if they would have been given as command line options in the order they are found.

### Default Option File Locations on Linux, Unix, Mac

On Linux, Unix, or Mac OS X, the default option file is called `my.cnf`. MariaDB looks for the MariaDB option file in the locations and orders listed below.

##### MariaDB starting with [10.0.13](/kb/en/mariadb-10013-release-notes/)

In [MariaDB 10.0.13](/kb/en/mariadb-10013-release-notes/) and later, the locations are dependent on whether the `DEFAULT_SYSCONFDIR` <a undefined>cmake</a> option was defined when MariaDB was built. This option is usually defined as `/etc` when building [RPM packages](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/), but it is usually not defined when building [DEB packages](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files/) or [binary tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs/).

- When the `DEFAULT_SYSCONFDIR` <a undefined>cmake</a> option was <strong>not</strong> defined, MariaDB looks for the MariaDB option file in the following locations in the following order:

<table><tbody><tr><th>Location</th><th>Scope</th></tr>
<tr><td><code>/etc/my.cnf</code></td><td>Global</td></tr>
<tr><td><code>/etc/mysql/my.cnf</code></td><td>Global</td></tr>
<tr><td><code>$MYSQL_HOME/my.cnf</code></td><td>Server</td></tr>
<tr><td>defaults-extra-file</td><td>File specified with <code><a href="/kb/en/mysqld-options/#-defaults-extra-file">--defaults-extra-file</a></code>, if any</td></tr>
<tr><td><code>~/.my.cnf</code></td><td>User</td></tr>
</tbody></table>

- When the `DEFAULT_SYSCONFDIR` <a undefined>cmake</a> option was defined, MariaDB looks for the MariaDB option file in the following locations in the following order:

<table><tbody><tr><th>Location</th><th>Scope</th></tr>
<tr><td><code>DEFAULT_SYSCONFDIR/my.cnf</code></td><td>Global</td></tr>
<tr><td><code>$MYSQL_HOME/my.cnf</code></td><td>Server</td></tr>
<tr><td>defaults-extra-file</td><td>File specified with <code><a href="/kb/en/mysqld-options/#-defaults-extra-file">--defaults-extra-file</a></code>, if any</td></tr>
<tr><td><code>~/.my.cnf</code></td><td>User</td></tr>
</tbody></table>

##### MariaDB until [10.0.12](/kb/en/mariadb-10012-release-notes/)

<table><tbody><tr><th>Location</th><th>Scope</th></tr>
<tr><td><code>/etc/my.cnf</code></td><td>Global</td></tr>
<tr><td><code>/etc/mysql/my.cnf</code></td><td>Global</td></tr>
<tr><td><code>SYSCONFDIR/my.cnf</code></td><td>Global</td></tr>
<tr><td><code>$MYSQL_HOME/my.cnf</code></td><td>Server</td></tr>
<tr><td>defaults-extra-file</td><td>File specified with <code><a href="/kb/en/mysqld-options/#-defaults-extra-file">--defaults-extra-file</a></code>, if any</td></tr>
<tr><td><code>~/.my.cnf</code></td><td>User</td></tr>
</tbody></table>

- `MYSQL_HOME` is the [environment variable](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-environment-variables/) containing the path to the directory holding the server-specific `my.cnf` file. If `MYSQL_HOME` is not set, and the server is started with [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/), `MYSQL_HOME` is set as follows:
<ul start="1" style="list-style: circle"><li>If there is a `my.cnf` file in the MariaDB data directory, but not in the MariaDB base directory, `MYSQL_HOME` is set to the MariaDB data directory.
</li><li>Else, `MYSQL_HOME` is set to the MariaDB base directory.
</li></ul>

### Default Option File Locations on Windows

On Windows, the option file can be called either `my.ini` or `my.cnf`. MariaDB looks for the MariaDB option file in the following locations in the following order:

<table><tbody><tr><th>Location</th><th>Scope</th></tr>
<tr><td><code>System Windows Directory\my.ini</code></td><td>Global</td></tr>
<tr><td><code>System Windows Directory\my.cnf</code></td><td>Global</td></tr>
<tr><td><code>Windows Directory\my.ini</code></td><td>Global</td></tr>
<tr><td><code>Windows Directory\my.cnf</code></td><td>Global</td></tr>
<tr><td><code>C:\my.ini</code></td><td>Global</td></tr>
<tr><td><code>C:\my.cnf</code></td><td>Global</td></tr>
<tr><td><code>INSTALLDIR\my.ini</code></td><td>Server</td></tr>
<tr><td><code>INSTALLDIR\my.cnf</code></td><td>Server</td></tr>
<tr><td><code>INSTALLDIR\data\my.ini</code></td><td>Server</td></tr>
<tr><td><code>INSTALLDIR\data\my.cnf</code></td><td>Server</td></tr>
<tr><td><code>%MYSQL_HOME%\my.ini</code></td><td>Server</td></tr>
<tr><td><code>%MYSQL_HOME%\my.cnf</code></td><td>Server</td></tr>
<tr><td>defaults-extra-file</td><td>File specified with <code><a href="/kb/en/mysqld-options/#-defaults-extra-file">--defaults-extra-file</a></code>, if any</td></tr>
</tbody></table>

- The `System Windows Directory` is the directory returned by the <a undefined>GetSystemWindowsDirectory</a> function. The value is usually `C:\Windows`. To find its specific value on your system, open <a undefined>cmd.exe</a> and execute: <span class="cstm-style" style="display:block; margin-top:3px; margin-bottom:-17px;"><pre class="fixed">echo %WINDIR%</pre></span>
- The `Windows Directory` is the directory returned by the <a undefined>GetWindowsDirectory</a> function. The value may be a private `Windows Directory` for the application, or it may be the same as the `System Windows Directory` returned by the <a undefined>GetSystemWindowsDirectory</a> function.
- `INSTALLDIR` is the parent directory of the directory where `mysqld.exe` is located. For example, if `mysqld.exe` is in C:\Program Files\<a undefined>MariaDB 10.3</a>\bin, then `INSTALLDIR` would be C:\Program Files\<a undefined>MariaDB 10.3</a>.
- `MYSQL_HOME` is the [environment variable](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-environment-variables/) containing the path to the directory holding the server-specific `my.cnf` file.

### Default Option File Hierarchy

MariaDB will look in all of the above locations, in order, even if has already found an option file, and it's possible for more than one option file to exist. For example, you could have an option file in `/etc/my.cnf` with global settings for all servers, and then you could another option file in `~/.my.cnf` (i.e.your user account's home directory) which will specify additional settings (or override previously specified setting) that are specific only to that user.

Option files are usually optional. However, if the <a undefined>--defaults-file</a> option is set, and if the file does not exist, then MariaDB will raise an error. If the <a undefined>--defaults-file</a> option is set, then MariaDB will <em>only</em> read the option file referred to by this option.

If an option or system variable is not explicitly set, then it will be set to its default value. See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a full list of all server system variables and their default values.

## Custom Option File Locations

MariaDB can be configured to read options from custom options files with the following command-line arguments. These command-line arguments can be used with most of MariaDB's command-line tools, not just [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/). They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-defaults-file">--defaults-file</a></code> <code>=path</code></td><td>Only read options from the given option file.</td></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-defaults-extra-file">--defaults-extra-file</a></code> <code>=path</code></td><td>Read this extra option file after all other option files are read.</td></tr>
</tbody></table>

## Option File Syntax

The syntax of the MariaDB option files are:

- Lines starting with # are comments.
- Empty lines are ignored.
- Option groups use the syntax `[group-name]`. See the [Option Groups](#option-groups) section below for more information on available option groups.
- The same option group can appear multiple times.
- The `!include` directive can be used to include other option files. See the [Including Option Files](#including-option-files) section below for more information on this syntax.
- The `!includedir` directive can be used to include all `.cnf` files (and potentially `.ini` files) in a given directory. The option files within the directory are read in alphabetical order. See the [Including Option File Directories](#including-option-file-directories) section below for more information on this syntax.
- Dashes (`-`) and underscores (`_`) in options are interchangeable.
- Double quotes can be used to quote values
- `\n`, `\r`, `\t`, `\b`, `\s`, `\"`, `\'`, and <code class="fixed" style="white-space:pre-wrap">\\</code> are recognized as character escapes for new line, carriage return, tab, backspace, space, double quote, single quote, and backslash respectively.
- Certain option prefixes are supported. See the [Option Prefixes](#option-prefixes) section below for information about available option prefixes.
- See the [Options](#options) section below for information about available options.

## Option Groups

A MariaDB program can read options from one or many option groups. For an exact list of option groups read on your system by a specific program, you can execute:

```sql
$program --help --verbose
```

For example:

```sql
$ mysqld --help --verbose
mysqld  Ver 10.3.13-MariaDB-log for Linux on x86_64 (MariaDB Server)
Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Starts the MariaDB database server.

Usage: mysqld [OPTIONS]

Default options are read from the following files in the given order:
/etc/my.cnf ~/.my.cnf
The following groups are read: mysqld server mysqld-10.3 mariadb mariadb-10.3 client-server galera
....
```

### Server Option Groups

MariaDB programs reads server options from the following server option groups:

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[server]</code></td><td>Options read by MariaDB Server.</td></tr>
<tr><td><code>[mysqld]</code></td><td>&nbsp;Options read by <code>mysqld</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mysqld-X.Y]</code></td><td>&nbsp;Options read by a specific version of <code>mysqld</code>, which includes both MariaDB Server and MySQL Server. For example, <code>[mysqld-10.4]</code>.</td></tr>
<tr><td><code>[mariadb]</code></td><td>Options read by MariaDB Server.</td></tr>
<tr><td><code>[mariadb-X.Y]</code></td><td>&nbsp;Options read by a specific version of MariaDB Server. For example, <code>[mariadb-10.4]</code>.</td></tr>
<tr><td><code>[mariadbd]</code></td><td>Options read by MariaDB Server. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[mariadbd-X.Y]</code></td><td>Options read by a specific version of MariaDB Server. For example, <code>[mariadbd-10.4]</code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[galera]</code></td><td>&nbsp;Options read by MariaDB Server, but only if it is compiled with <a href="/kb/en/galera/">Galera Cluster</a> support. In <a href="/kb/en/what-is-mariadb-101/">MariaDB 10.1</a> and later, all builds on Linux are compiled with <a href="/kb/en/galera/">Galera Cluster</a> support. When using one of these builds, options from this option group are read even if the <a href="/kb/en/galera/">Galera Cluster</a> functionality is not enabled.</td></tr>
</tbody></table>

<em>X.Y</em> in the examples above refer to the base (major.minor) version of the server. For example, [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/) would read from `[mariadb-10.3]`. By using the `mariadb-X.Y` syntax, one can create option files that have MariaDB-only options in the MariaDB-specific option groups. That would allow the option file to work for both MariaDB and MySQL.

### Client Option Groups

MariaDB programs reads client options from the following option groups:

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[client]</code></td><td>&nbsp;Options read by all MariaDB and MySQL <a href="/kb/en/clients-utilities/">client programs</a>, which includes both MariaDB and MySQL clients. For example, <code>mysqldump</code>.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[client-mariadb]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a>.</td></tr>
</tbody></table>

### Tool-Specific Option Groups

Many MariaDB tools reads options from their own option groups as well. Many of them are listed below:

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysqld_safe]</code></td><td>Options read by <code><a href="/kb/en/mysqld_safe/">mysqld_safe</a></code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[safe_mysqld]</code></td><td>Options read by <code><a href="/kb/en/mysqld_safe/">mysqld_safe</a></code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb_safe]</code></td><td>Options read by <code><a href="/kb/en/mysqld_safe/">mysqld_safe</a></code> from MariaDB Server.</td></tr>
<tr><td><code>[mariadb-safe]</code></td><td>Options read by <code><a href="/kb/en/mysqld_safe/">mysqld_safe</a></code> from MariaDB Server. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[mariabackup]</code></td><td>Options read by <a href="/kb/en/mariabackup/">Mariabackup</a>. Available starting with <a href="/kb/en/mariadb-10131-release-notes/">MariaDB 10.1.31</a> and <a href="/kb/en/mariadb-10213-release-notes/">MariaDB 10.2.13</a>.</td></tr>
<tr><td><code>[xtrabackup]</code></td><td>Options read by <a href="/kb/en/mariabackup/">Mariabackup</a> and <a href="/kb/en/percona-xtrabackup-overview/">Percona XtraBackup</a>.</td></tr>
<tr><td><code>[mysql_upgrade]</code></td><td>Options read by <code><a href="/kb/en/mysql_upgrade/">mysql_upgrade</a></code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-upgrade]</code></td><td>Options read by <code><a href="/kb/en/mysql_upgrade/">mysql_upgrade</a></code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[sst]</code></td><td>Specific options read by the <a href="/kb/en/mariabackup-sst-method/">mariabackup SST method</a> and the <a href="/kb/en/xtrabackup-v2-sst-method/">xtrabackup-v2 SST method</a>.</td></tr>
<tr><td><code>[mysql]</code></td><td>Options read by <code><a href="/kb/en/mysql-client/">mysql</a></code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-client]</code></td><td>Options read by <code><a href="/kb/en/mysql-client/">mysql</a></code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[mysqldump]</code></td><td>Options read by <code><a href="/kb/en/mysqldump/">mysqldump</a></code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-dump]</code></td><td>Options read by <code><a href="/kb/en/mysqldump/">mysqldump</a></code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[mysqlimport]</code></td><td>Options read by <code><a href="/kb/en/mysqlimport/">mysqlimport</a></code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-import]</code></td><td>Options read by <code><a href="/kb/en/mysqlimport/">mysqlimport</a></code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[mysqlbinlog]</code></td><td>Options read by <code><a href="/kb/en/mysqlbinlog/">mysqlbinlog</a></code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-binlog]</code></td><td>Options read by <code><a href="/kb/en/mysqlbinlog/">mysqlbinlog</a></code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[mysqladmin]</code></td><td>Options read by <code><a href="/kb/en/mysqladmin/">mysqladmin</a></code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-admin]</code></td><td>Options read by <code><a href="/kb/en/mysqladmin/">mysqladmin</a></code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[mysqlshow]</code></td><td>Options read by <code><a href="/kb/en/mysqlshow/">mysqlshow</a></code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-show]</code></td><td>Options read by <code><a href="/kb/en/mysqlshow/">mysqlshow</a></code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[mysqlcheck]</code></td><td>Options read by <code><a href="/kb/en/mysqlcheck/">mysqlcheck</a></code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-check]</code></td><td>Options read by <code><a href="/kb/en/mysqlcheck/">mysqlcheck</a></code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[mysqlslap]</code></td><td>Options read by <code><a href="/kb/en/mysqlslap/">mysqlslap</a></code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-slap]</code></td><td>Options read by <code><a href="/kb/en/mysqlslap/">mysqlslap</a></code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[odbc]</code></td><td>Options read by <a href="/kb/en/mariadb-connector-odbc/">MariaDB Connector/ODBC</a>, but only if the <code><a href="/kb/en/about-mariadb-connector-odbc/#general-connection-parameters">USE_MYCNF</a></code> parameter has been set.</td></tr>
</tbody></table>

### Custom Option Group Suffixes

MariaDB can be configured to read options from option groups with a custom suffix by providing the following command-line argument. This command-line argument can be used with most of MariaDB's command-line tools, not just [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/). It must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-defaults-group-suffix">--defaults-group-suffix</a></code> <code>=suffix</code></td><td>In addition to the default option groups, also read option groups with the given suffix.</td></tr>
</tbody></table>

The default group suffix can also be specified via the `MYSQL_GROUP_SUFFIX` [environment variable](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-environment-variables/).

## Including Option Files

It is possible to include additional option files from another option file. For example, to include `/etc/mysql/dbserver1.cnf`, an option file could contain:

```sql
[mariadb]
...
!include /etc/mysql/dbserver1.cnf
```

## Including Option File Directories

It is also possible to include all option files in a directory from another option file. For example, to include all option files in `/etc/my.cnf.d/`, an option file could contain:

```sql
[mariadb]
...
!includedir /etc/my.cnf.d/
```

The option files within the directory are read in alphabetical order.

All option file names must end in `.cnf` on Unix-like operating systems. On Windows, all option file names must end in `.cnf` or `.ini`.

## Checking Program Options

You can check which options a given program is going to use by using the <a undefined>--print-defaults</a> command-line argument:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-print-defaults">--print-defaults</a></code></td><td>Read options from option files, print all option values, and then exit the program.</td></tr>
</tbody></table>

This command-line argument can be used with most of MariaDB's command-line tools, not just [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/). It must be given as the first argument on the command-line. For example:

```sql
$ mysqldump --print-defaults
mysqldump would have been started with the following arguments:
--ssl_cert=/etc/my.cnf.d/certificates/client-cert.pem --ssl_key=/etc/my.cnf.d/certificates/client-key.pem --ssl_ca=/etc/my.cnf.d/certificates/ca.pem --ssl-verify-server-cert --max_allowed_packet=1GB
```

You can also check which options a given program is going to use by using the [my_print_defaults](/clients-utilities/my_print_defaults/) utility and providing the names of the option groups that the program reads. For example:

```sql
$ my_print_defaults mysqldump client client-server client-mariadb
--ssl_cert=/etc/my.cnf.d/certificates/client-cert.pem
--ssl_key=/etc/my.cnf.d/certificates/client-key.pem
--ssl_ca=/etc/my.cnf.d/certificates/ca.pem
--ssl-verify-server-cert
--max_allowed_packet=1GB
```

The [my_print_defaults](/clients-utilities/my_print_defaults/) utility's `--mysqld` command-line option provides a shortcut to refer to all of the [server option groups](#server-option-groups):

```sql
$ my_print_defaults --mysqld
--log_bin=mariadb-bin
--log_slave_updates=ON
--ssl_cert=/etc/my.cnf.d/certificates/server-cert.pem
--ssl_key=/etc/my.cnf.d/certificates/server-key.pem
--ssl_ca=/etc/my.cnf.d/certificates/ca.pem
```

## MySQL 5.6 Obfuscated Authentication Credential Option File

MySQL 5.6 and above support an obfuscated authentication credential option file called `.mylogin.cnf` that is created with <a undefined>mysql_config_editor</a>.

MariaDB does not support this. The passwords in MySQL's `.mylogin.cnf` are only obfuscated, rather than encrypted, so the feature does not really add much from a security perspective. It is more likely to give users a false sense of security, rather than to seriously protect them.

## Option Prefixes

MariaDB supports certain prefixes that can be used with options. The supported option prefixes are:

<table><tbody><tr><th>Option Prefix</th><th>Description</th></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-autoset-">autoset</a></code></td><td>Sets the option value automatically. Only supported for certain options. Available in <a href="/kb/en/mariadb-1017-release-notes/">MariaDB 10.1.7</a> and later.</td></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-disable-">disable</a></code></td><td>For all boolean options, disables the setting (equivalent to setting it to <code>0</code>). Same as <code>skip</code>.</td></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-enable-">enable</a></code></td><td>For all boolean options, enables the setting (equivalent to setting it to <code>1</code>).</td></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-loose-">loose</a></code></td><td>Don't produce an error if the option doesn't exist.</td></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-maximum-">maximum</a></code></td><td>Sets the maximum value for the option.</td></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-skip-">skip</a></code></td><td>For all boolean options, disables the setting (equivalent to setting it to <code>0</code>). Same as <code>disable</code>.</td></tr>
</tbody></table>

For example:

```sql
[mariadb]
...
# determine a good value for open_files_limit automatically
autoset_open_files_limit

# disable the unix socket plugin
disable_unix_socket

# enable the slow query log
enable_slow_query_log

# don't produce an error if these options don't exist
loose_file_key_management_filename = /etc/mysql/encryption/keyfile.enc
loose_file_key_management_filekey = FILE:/etc/mysql/encryption/keyfile.key
loose_file_key_management_encryption_algorithm = AES_CTR

# set max_allowed_packet to maximum value
maximum_max_allowed_packet

# disable external locking for MyISAM
skip_external_locking
```

## Options

Dashes (`-`) and underscores (`_`) in options are interchangeable.

If an option is not explicitly set, then the server or client will simply use the default value for that option.

### MariaDB Server Options

MariaDB Server options can be set in [server option groups](#server-option-groups).

For a list of options that can be set for MariaDB Server, see the list of options available for [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/).

Most of the [server system variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) can also be set in MariaDB's option file.

### MariaDB Client Options

MariaDB client options can be set in [client option groups](#client-option-groups).

See the specific page for each [client program](/clients-utilities/) to determine what options are available for that program.

## Example Option Files

Most MariaDB installations include a sample MariaDB option file called `my-default.cnf`. On older releases, you would have also found the following option files:

- `my-small.cnf`
- `my-medium.cnf`
- `my-large.cnf`
- `my-huge.cnf`

However, these option files are now very dated for modern servers, so they were removed in [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/).

In source distributions, the sample option files are usually found in the `support-files` directory, and in other distributions, the option files are usually found in the `share/mysql` directory that is relative to the MariaDB base installation directory.

You can copy one of these sample MariaDB option files and use it as the basis for building your server's primary MariaDB option file.

### Example Minimal Option File

The following is a minimal my.cnf file that you can use to test MariaDB.

```sql
[client-server]
# Uncomment these if you want to use a nonstandard connection to MariaDB
#socket=/tmp/mysql.sock
#port=3306

# This will be passed to all MariaDB clients
[client]
#password=my_password

# The MariaDB server
[mysqld]
# Directory where you want to put your data
data=/usr/local/mysql/var
# Directory for the errmsg.sys file in the language you want to use
language=/usr/local/share/mysql/english

# This is the prefix name to be used for all log, error and replication files
log-basename=mysqld

# Enable logging by default to help find problems
general-log
log-slow-queries
```

### Example Hybrid Option File

The following is an extract of an option file that one can use if one wants to work with both MySQL and MariaDB.

```sql
# Example mysql config file.

[client-server]
socket=/tmp/mysql-dbug.sock
port=3307

# This will be passed to all mysql clients
[client]
password=my_password

# Here are entries for some specific programs
# The following values assume you have at least 32M ram

# The MySQL server
[mysqld]
temp-pool
key_buffer_size=16M
datadir=/my/mysqldata
loose-innodb_file_per_table

[mariadb]
datadir=/my/data
default-storage-engine=aria
loose-mutex-deadlock-detector
max-connections=20

[mariadb-5.5]
language=/my/maria-5.5/sql/share/english/
socket=/tmp/mysql-dbug.sock
port=3307

[mariadb-10.1]
language=/my/maria-10.1/sql/share/english/
socket=/tmp/mysql2-dbug.sock

[mysqldump]
quick
max_allowed_packet=16M

[mysql]
no-auto-rehash
loose-abort-source-on-error
```

## See Also

- [Configuring MariaDB Connector/C with Option Files](/kb/en/configuring-mariadb-connectorc-with-option-files/)
- [Troubleshooting Connection Issues](/kb/en/troubleshooting-connection-issues/)
- [Configuring MariaDB for Remote Client Access](/kb/en/configuring-mariadb-for-remote-client-access/)
- [MySQL 5.6: Security through Complacency?](https://mariadb.com/resources/blog/mysql-5-6-security-through-complacency)