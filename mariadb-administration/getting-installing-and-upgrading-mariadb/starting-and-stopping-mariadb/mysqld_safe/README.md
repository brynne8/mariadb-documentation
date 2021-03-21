# mysqld_safe

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadbd-safe` is a symlink to `mysqld_safe`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadbd-safe` is the name of the server, with `mysqld_safe` a symlink .

<br><br>
The `mysqld_safe` startup script is in MariaDB distributions on Linux and Unix. It is a wrapper that starts `mysqld` with some extra safety features. For example, if `mysqld_safe` notices that `mysqld` has crashed, then `mysqld_safe` will automatically restart `mysqld`.

`mysqld_safe` is the recommended way to start `mysqld` on Linux and Unix distributions that do not support [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd). Additionally, the [mysql.server](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqlserver) init script used by [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit) starts `mysqld` with `mysqld_safe` by default.
<br><br>

## Using mysqld_safe

The command to use `mysqld_safe` and the general syntax is:

```sql
mysqld_safe [ --defaults-file | --defaults-extra-file ] <options> <mysqld_options>
```

### Options

Many of the options supported by `mysqld_safe` are identical to
options supported by [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options). If an unknown option is provided to `mysqld_safe` on the command-line, then it is passed to `mysqld`.

`mysqld_safe` supports the following options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--help</code></td><td>Display a help message and exit.</td></tr>
<tr><td><code>--autoclose</code></td><td>(NetWare only) On NetWare, <code class="fixed" style="white-space:pre-wrap">mysqld_safe</code> provides a screen presence. When you unload (shut down) the <code>mysqld_safe</code> NLM, the screen does not by default go away. Instead, it prompts for user input: <code>NLM has terminated; Press any key to close the screen</code>. If you want NetWare to close the screen automatically instead, use the <code>--autoclose</code> option to mysqld_safe.</td></tr>
<tr><td><code>--basedir=path</code></td><td>The path to the MariaDB installation directory.</td></tr>
<tr><td><code>--core-file-size=size</code></td><td>The size of the core file that mysqld should be able to create. The option value is passed to ulimit -c.</td></tr>
<tr><td><code>--crash-script=file</code></td><td>Script to call in the event of mysqld crashing.</td></tr>
<tr><td><code>--datadir=path</code></td><td>The path to the data directory.</td></tr>
<tr><td><code>--defaults-extra-file=path</code></td><td>The name of an option file to be read in addition to the usual option files. This must be the first option on the command line if it is used. If the file does not exist or is otherwise inaccessible, the server will exit with an error.</td></tr>
<tr><td><code>--defaults-file=file_name</code></td><td>The name of an option file to be read instead of the usual option files. This must be the first option on the command line if it is used.</td></tr>
<tr><td><code>--flush-caches</code></td><td>Flush and purge buffers/caches before starting the server.</td></tr>
<tr><td><code>--ledir=path</code></td><td>If mysqld_safe cannot find the server, use this option to indicate the path name to the directory where the server is located.</td></tr>
<tr><td><code>--log-error=file_name</code></td><td>Write the error log to the given file.</td></tr>
<tr><td><code>--malloc-lib=lib</code></td><td>Preload shared library <em>lib</em> if available. See <a href="/kb/en/debugging-a-running-server-on-linux/">debugging MariaDB</a> for an example.</td></tr>
<tr><td><code>--mysqld=prog_nam</code></td><td>The name of the server program (in the ledir directory) that you want to start. This option is needed if you use the MariaDB binary distribution but have the data directory outside of the binary distribution. If mysqld_safe cannot find the server, use the <code>--ledir</code>  option to indicate the path name to the directory where the server is located.</td></tr>
<tr><td><code>--mysqld-version=suffix</code></td><td>This option is similar to the <code>--mysqld</code> option, but you specify only the suffix for the server program name. The basename is assumed to be mysqld. For example, if you use<code>--mysqld-version=debug</code>, mysqld_safe starts the mysqld-debug program in the ledir directory.  If the argument to <code>--mysqld-version</code> is empty, mysqld_safe uses mysqld in the ledir directory.</td></tr>
<tr><td><code>--nice=priority</code></td><td>Use the nice program to set the server´s scheduling priority to the given value.</td></tr>
<tr><td><code>--no-auto-restart</code></td><td>Exit after starting mysqld.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Do not read any option files. This must be the first option on the command line if it is used.</td></tr>
<tr><td><code>--no-watch</code></td><td>Exit after starting mysqld.</td></tr>
<tr><td><code>--numa-interleave</code></td><td>Run mysqld with its memory interleaved on all NUMA nodes.</td></tr>
<tr><td><code>--open-files-limit=count</code></td><td>The number of files that mysqld should be able to open. The option value is passed to ulimit -n. Note that you need to start mysqld_safe as root for this to work properly.</td></tr>
<tr><td><code>--pid-file=file_name</code></td><td>The path name of the process ID file.</td></tr>
<tr><td><code>--plugin-dir=dir_name</code>,</td><td>Directory for client-side plugins.</td></tr>
<tr><td><code>--port=port_num</code></td><td>The port number that the server should use when listening for TCP/IP connections. The port number must be 1024 or higher unless the server is started by the root system user.</td></tr>
<tr><td><code>--skip-kill-mysqld</code></td><td>Do not try to kill stray mysqld processes at startup. This option works only on Linux.</td></tr>
<tr><td><code>--socket=path</code></td><td>The Unix socket file that the server should use when listening for local connections.</td></tr>
<tr><td><code>--syslog</code>, <code>--skip-syslog</code></td><td><code>--syslog</code> causes error messages to be sent to syslog on systems that support the logger program.  <code>--skip-syslog</code> suppresses the use of syslog; messages are written to an error log file.</td></tr>
<tr><td><code>--syslog-tag=tag</code></td><td>For logging to syslog, messages from <code>mysqld_safe</code> and mysqld are written with a tag of mysqld_safe and mysqld, respectively. To specify a suffix for the tag, use <code>--syslog-tag=tag</code>, which modifies the tags to be <code>mysqld_safe-tag</code> and <code>mysqld-tag</code>.</td></tr>
<tr><td><code>--timezone=timezone</code></td><td>Set the TZ time zone <a href="/kb/en/mariadb-environment-variables/">environment variable</a> to the given option value. Consult your operating system documentation for legal time zone specification formats. Also see <a href="/kb/en/time-zones/">Time Zones</a>.</td></tr>
<tr><td><code>--user={user_name or user_id}</code></td><td>Run the mysqld server as the user having the name user_name or the numeric user ID user_id.  (“User” in this context refers to a system login account, not a MariaDB user listed in the grant tables.)</td></tr>
</tbody></table>

### Option Files

In addition to reading options from the command-line, `mysqld_safe` can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). If an unknown option is provided to `mysqld_safe` in an option file, then it is ignored.

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
<tr><td><code>[mysqld_safe]</code></td><td>&nbsp;Options read by <code>mysqld_safe</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[safe_mysqld]</code></td><td>&nbsp;Options read by <code>mysqld_safe</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb_safe]</code></td><td>Options read by <code>mysqld_safe</code> from MariaDB Server.</td></tr>
<tr><td><code>[mariadb-safe]</code></td><td>Options read by <code>mysqld_safe</code> from MariaDB Server. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
</tbody></table>

The `[safe_mysqld]` option group is primarily supported for backward compatibility. You should rename such option groups to `[mysqld_safe]` in MariaDB installations to prevent breakage in the future if this compatibility is removed.

`mysqld_safe` also reads options from the following server [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysqld]</code></td><td>&nbsp;Options read by <code>mysqld</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[server]</code></td><td>Options read by MariaDB Server.</td></tr>
<tr><td><code>[mysqld-X.Y]</code></td><td>&nbsp;Options read by a specific version of <code>mysqld</code>, which includes both MariaDB Server and MySQL Server. For example, <code>[mysqld-5.5]</code>.</td></tr>
<tr><td><code>[mariadb]</code></td><td>Options read by MariaDB Server.</td></tr>
<tr><td><code>[mariadb-X.Y]</code></td><td>&nbsp;Options read by a specific version of MariaDB Server.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[galera]</code></td><td>&nbsp;Options read by a galera-capable MariaDB Server. Available on systems compiled with Galera support.</td></tr>
</tbody></table>

For example, if you specify the <a undefined>log_error</a> option in a server option group in an option file, like this:

```sql
[mariadb]
log_error=error.log
```

Then `mysqld_safe` will also use this value for its own `--log-error` option:

### Configuring the Open Files Limit

When using `mysqld_safe`, the system's open files limit can be changed by providing the `--open-files-limit` option either on the command-line or in an option file. For example:

```sql
[mysqld_safe]
open_files_limit=4294967295
```

The option value is passed to `ulimit -n`. Note that you need to start `mysqld_safe` as root for this to work properly. However, you can't currently set this to `unlimited`. See [MDEV-18410](https://jira.mariadb.org/browse/MDEV-18410) about that.

When `mysqld_safe` starts `mysqld`, it also uses this option to set the value of the <a undefined>open_files_limit</a> system variable for `mysqld`.

### Configuring the Core File Size

When using `mysqld_safe`, if you would like to [enable core dumps](/kb/en/enabling-core-dumps/), the system's core file size limit can be changed by providing the `--core-file-size` option either on the command-line or in an option file. For example:

```sql
[mysqld_safe]
core_file_size=unlimited
```

The option value is passed to `ulimit -c`. Note that you need to start `mysqld_safe` as root for this to work properly.

### Configuring MariaDB to Write the Error Log to Syslog

When using `mysqld_safe`, if you would like to redirect the error log to the [syslog](https://linux.die.net/man/8/rsyslogd), then that can easily be done by using the `--syslog` option. `mysqld_safe` redirects two types of log messages to the syslog--its own log messages, and log messages for `mysqld`.

- `mysqld_safe` configures its own log messages to go to the `daemon` syslog facility. The log level for these messages is either `notice` or `error`, depending on the specific type of log message. The default tag is `mysqld_safe`.

- `mysqld_safe` also configures the log messages for `mysqld` to go to the `daemon` syslog facility. The log level for these messages is `error`. The default tag is `mysqld`.

Sometimes it can be helpful to add a suffix to the syslog tag, such as if you are running multiple instances of MariaDB on the same host. To add a suffix to each syslog tag, use the `--syslog-tag` option.

## Specifying mysqld

By default, `mysqld_safe` tries to start an executable named `mysqld`.

You can also specify another executable for `mysqld_safe` to start instead of `mysqld` by providing the `--mysqld` or `--mysqld-version` options either on the command-line or in an option file.

By default, it will look for `mysqld` in the following locations in the following order:

- `$BASEDIR/libexec/mysqld`
- `$BASEDIR/sbin/mysqld`
- `$BASEDIR/bin/mysqld`
- `$PWD/bin/mysqld`
- `$PWD/libexec/mysqld`
- `$PWD/sbin/mysqld`
- `@libexecdir@/mysql`

Where `$BASEDIR` is set by the `--basedir` option, `$PWD` is the current working directory where `mysqld_safe` was invoked, and `@libexecdir@` is set at compile-time by the `INSTALL_BINDIR` option for <a undefined>cmake</a>.

You can also specify where the executable is located by providing the `--ledir` option either on the command-line or in an option file.

## Specifying datadir

By default, `mysqld_safe` will look for the `datadir` in the following locations in the following order:

- `$BASEDIR/data/mysql`
- `$BASEDIR/data`
- `$BASEDIR/var/mysql`
- `$BASEDIR/var`
- `@localstatedir@`

Where `$BASEDIR` is set by the `--basedir` option, and `@localstatedir@` is set at compile-time by the `INSTALL_MYSQLDATADIR` option for <a undefined>cmake</a>.

You can also specify where the `datadir` is located by providing the `--datadir` option either on the command-line or in an option file.

## Logging

When you use `mysqld_safe` to start `mysqld`, `mysqld_safe` logs to the same destination as `mysqld`.

`mysqld_safe` has several log-related options:

- `--syslog`: Write error messages to syslog on systems that
  support the logger program.
- `--skip-syslog`: Do not write error messages to syslog.
  Messages are written to the default error log file (host_name.err in the data
  directory), or to a named file if the <code class="fixed" style="white-space:pre-wrap">--log-error</code> option
  is given.
- `--log-error=file_name`: Write error messages to the named
  error file.

If none of these options is provided, then the default is `--skip-syslog`.

If `--syslog` and `--log-error` are both provided, then a warning is issued and `--log-error` takes precedence.

`mysqld_safe` also writes notices to `stdout` and errors to `stderr`.

## Editing mysqld_safe

`mysqld_safe` is a `sh` script, so if you need to change its behavior, then it can easily be edited. However, you should not normally edit the script. A lot of behavior can be changed by providing options either on the command-line or in an option file.

If you do edit `mysqld_safe`, then you should be aware of the fact that a package upgrade can overwrite your changes. If you would like to preserve your changes, be sure to have a backup.

## NetWare

On NetWare, mysqld_safe is a NetWare Loadable Module (NLM)
that is ported from the original Unix shell script. It starts the server as
follows:

1 Runs a number of system and option checks.
2 Runs a check on MyISAM tables.
3 Provides a screen presence for the MariaDB server.
4 Starts mysqld, monitors it, and restarts it if it terminates in error.
5 Sends error messages from mysqld to the host_name.err file in the data directory.
6 Sends mysqld_safe screen output to the host_name.safe file
  in the data directory.

## See Also

- [How to increase max number of open files on Linux](http://www.cyberciti.biz/faq/linux-increase-the-maximum-number-of-open-files). This can be used to solve issues like this warning from mysqld:  <code class="fixed" style="white-space:pre-wrap">Changed limits: max_open_files: 1024 (requested 5000)"</code>
- [mysqld Options](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options)
- [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd)