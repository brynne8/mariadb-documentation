# mysqld_multi

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadbd-multi` is a symlink to `mysqld_multi`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadbd-multi` is the name of the server, with `mysqld_multi` a symlink .

Before using mysqld_multi be sure that you understand the meanings of the options that are passed to the mysqld servers and why you would want to have separate mysqld processes. Beware of the dangers of using multiple mysqld servers with the same data directory. Use separate data directories, unless you know what you are doing. Starting multiple servers with the same data directory does not give you extra performance in a threaded system.

The `mysqld_multi` startup script is in MariaDB distributions on Linux and Unix. It is a wrapper that is designed to manage several `mysqld` processes running on the same host. In order for multiple `mysqld` processes to work on the same host, these processes must:

- Use different Unix socket files for local connections.
- Use different TCP/IP ports for network connections.
- Use different data directories.
- Use different process ID files (specified by the `--pid-file` option) if using [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe) to start `mysqld`.

`mysqld_multi` can start or stop servers, or report their current status.

## Using mysqld_multi

The command to use `mysqld_multi` and the general syntax is:

```sql
mysqld_multi [options] {start|stop|report} [GNR[,GNR] ...]
```

`start`, `stop`, and `report` indicate which operation to perform.

You can specify which servers to perform the operation on by providing one or more `GNR` values. `GNR` refers to an option group number, and it is explained more in the [option groups](#option-groups) section below. If there is no `GNR` list, then `mysqld_multi` performs the operation for all `GNR` values found in its option files.

Multiple `GNR` values can be specified as a comma-separated list. `GNR` values can also be specified as a range by separating the numbers by a dash. There must not be any whitespace characters in the `GNR` list.

For example:

This command starts a single server using option group `[mysqld17]`:

```sql
mysqld_multi start 17
```

This command stops several servers, using option groups `[mysqld8]` and `[mysqld10]` through `[mysqld13]`:

```sql
mysqld_multi stop 8,10-13
```

### Options

`mysqld_multi` supports the following options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--example</code></td><td>Give an example of a config file with extra information.</td></tr>
<tr><td><code>--help</code></td><td>Display help and exit.</td></tr>
<tr><td><code>--log=filename</code></td><td>Specify the path and name of the log file. If the file exists, log output is appended to it.</td></tr>
<tr><td><code>--mysqladmin=prog_name</code></td><td>The <a href="/kb/en/mysqladmin/">mysqladmin</a> binary to be used to stop servers. Can be given within groups <code>[mysqld#]</code>.</td></tr>
<tr><td><code>--mysqld=prog_name</code></td><td>The mysqld binary to be used. Note that you can also specify <a href="/kb/en/mysqld_safe/">mysqld_safe</a> as the value for this option. If you use mysqld_safe to start the server, you can include the <code>mysqld</code> or <code>ledir</code> options in the corresponding <code>[mysqldN]</code> option group. These options indicate the name of the server that mysqld_safe should start and the path name of the directory where the server is located. Example:<br><code>[mysqld38]</code><br><code>mysqld = mysqld-debug</code><br><code>ledir  = /opt/local/mysql/libexec</code>.</td></tr>
<tr><td><code>--no-log</code></td><td>Print to stdout instead of the log file. By default the log file is turned on.</td></tr>
<tr><td><code>--password=password</code></td><td>The password of the MariaDB account to use when invoking <a href="/kb/en/mysqladmin/">mysqladmin</a>. Note that the password value is not optional for this option, unlike for other MariaDB programs.</td></tr>
<tr><td><code>--silent</code></td><td>Silent mode; disable warnings.</td></tr>
<tr><td><code>--tcp-ip</code></td><td>Connect to the MariaDB server(s) via the TCP/IP port instead of the UNIX socket. This affects stopping and reporting. If a socket file is missing, the server may still be running, but can be accessed only via the TCP/IP port. By default connecting is done via the UNIX socket. This option affects stop and report operations.</td></tr>
<tr><td><code>--user=username</code></td><td>The user name of the MariaDB account to use when invoking <a href="/kb/en/mysqladmin/">mysqladmin</a>.</td></tr>
<tr><td><code>--verbose</code></td><td>Be more verbose.</td></tr>
<tr><td><code>--version</code></td><td>Display version information and exit.</td></tr>
<tr><td><code>--wsrep-new-cluster</code></td><td>Bootstrap a cluster. Added in <a href="/kb/en/mariadb-10115-release-notes/">MariaDB 10.1.15</a>.</td></tr>
</tbody></table>

### Option Files

In addition to reading options from the command-line, `mysqld_multi` can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). If an unknown option is provided to `mysqld_multi` in an option file, then it is ignored.

The following options relate to how MariaDB command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
</tbody></table>

#### Option Groups

`mysqld_safe` reads options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysqld_multi]</code></td><td>&nbsp;Options read by <code>mysqld_multi</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
</tbody></table>

`mysqld_multi` also searches [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) for [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) with names like `[mysqldN]`, where `N` can be any positive integer. This number is referred to in the following discussion as the option group number, or `GNR`:

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysqldN]</code></td><td>&nbsp;Options read by a <code>mysqld</code> instance managed by <code>mysqld_multi</code>, which includes both MariaDB Server and MySQL Server. The <code>N</code> refers to the instance's <code>GNR</code>.</td></tr>
</tbody></table>

`GNR` values distinguish option groups from one another and are used as arguments to `mysqld_multi` to specify which servers you want to start, stop, or obtain a status report for. The `GNR` value should be the number at the end of the option group name in the option file. For example, the `GNR` for an option group named `[mysqld17]` is `17`.

Options listed in these option groups are the same that you would use in the regular server option groups used for configuring `mysqld`. However, when using multiple servers, it is necessary that each one use its own value for options such as the Unix socket file and TCP/IP port number.

The `[mysqld_multi]` option group can be used for options that are needed for `mysqld_multi` itself. `[mysqldN]` option groups can be used for options passed to specific `mysqld` instances.

The regular server [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) can also be used for common options that are read by all instances:

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysqld]</code></td><td>&nbsp;Options read by <code>mysqld</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[server]</code></td><td>Options read by MariaDB Server.</td></tr>
<tr><td><code>[mysqld-X.Y]</code></td><td>&nbsp;Options read by a specific version of <code>mysqld</code>, which includes both MariaDB Server and MySQL Server. For example, <code>[mysqld-5.5]</code>.</td></tr>
<tr><td><code>[mariadb]</code></td><td>Options read by MariaDB Server.</td></tr>
<tr><td><code>[mariadb-X.Y]</code></td><td>&nbsp;Options read by a specific version of MariaDB Server.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[galera]</code></td><td>&nbsp;Options read by a galera-capable MariaDB Server. Available on systems compiled with Galera support.</td></tr>
</tbody></table>

For an example of how you might set up an option file, use this command:

```sql
mysqld_multi --example
```

### Authentication and Privileges

Make sure that the MariaDB account used for stopping the `mysqld` processes (with the [mysqladmin](/clients-utilities/mysqladmin) utility) has the same user name and password for each server. Also, make sure that the account has the `SHUTDOWN` privilege. If the servers that you want to manage have different user names or passwords for the administrative accounts, you might want to create an account on each server that has the same user name and password. For example, you might set up a common `multi_admin` account by executing the following commands for each server:

```sql
shell> mysql -u root -S /tmp/mysql.sock -p
Enter password:
mysql> GRANT SHUTDOWN ON *.*
 -> TO ´multi_admin´@´localhost´ IDENTIFIED BY ´multipass´;
```

Change the connection parameters appropriately when connecting to each one. Note that the host name part of the account name must allow you to connect as `multi_admin` from the host where you want to run `mysqld_multi`.

## User Account

Make sure that the data directory for each server is fully accessible to the Unix account that the specific `mysqld` process is started as. If you run the `mysqld_multi` script as the Unix `root` account, and if you want the `mysqld` process to be started with another Unix account, then you can use use the `--user` option with `mysqld`. If you specify the `--user` option in an option file, and if you did not run the `mysqld_multi` script as the Unix `root` account, then it will just log a warning and the `mysqld` processes are started under the original Unix account.

Do not run the `mysqld` process as the Unix `root` account, unless you know what you are doing.

## Example

The following example shows how you might set up an option file for use with mysqld_multi. The order in which the mysqld programs are started or stopped depends on the order in which they appear in the option file. Group numbers need not form an unbroken sequence. The first and fifth [mysqldN] groups were intentionally omitted from the example to illustrate that you can have “gaps” in the option file. This gives you more flexibility.

```sql
           # This file should probably be in your home dir (~/.my.cnf)
           # or /etc/my.cnf
           # Version 2.1 by Jani Tolonen
           [mysqld_multi]
           mysqld     = /usr/local/bin/mysqld_safe
           mysqladmin = /usr/local/bin/mysqladmin
           user       = multi_admin
           password   = multipass
           [mysqld2]
           socket     = /tmp/mysql.sock2
           port       = 3307
           pid-file   = /usr/local/mysql/var2/hostname.pid2
           datadir    = /usr/local/mysql/var2
           language   = /usr/local/share/mysql/english
           user       = john
           [mysqld3]
           socket     = /tmp/mysql.sock3
           port       = 3308
           pid-file   = /usr/local/mysql/var3/hostname.pid3
           datadir    = /usr/local/mysql/var3
           language   = /usr/local/share/mysql/swedish
           user       = monty
           [mysqld4]
           socket     = /tmp/mysql.sock4
           port       = 3309
           pid-file   = /usr/local/mysql/var4/hostname.pid4
           datadir    = /usr/local/mysql/var4
           language   = /usr/local/share/mysql/estonia
           user       = tonu
           [mysqld6]
           socket     = /tmp/mysql.sock6
           port       = 3311
           pid-file   = /usr/local/mysql/var6/hostname.pid6
           datadir    = /usr/local/mysql/var6
           language   = /usr/local/share/mysql/japanese
           user       = jani
```