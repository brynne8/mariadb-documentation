# mysql.server

The [mysql.server](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqlserver) startup script is in MariaDB distributions on Linux and Unix. It is a wrapper that works as a standard [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit) script. However, it can be used independently of [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit) as a regular `sh` script. The script starts the [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) server process by first changing its current working directory to the MariaDB install directory and then starting [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe). The script requires the standard [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit) arguments, such as `start`, `stop`, `restart`, and `status`. For example:

```sql
mysql.server start
mysql.server restart
mysql.server stop
mysql.server status
```

It can be used on systems such as Linux, Solaris, and Mac OS X.

The `mysql.server` script starts [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) by first changing to the MariaDB install directory and then calling <a undefined>mysqld_safe</a>.

## Using mysql.server

The command to use `mysql.server` and the general syntax is:

```sql
mysql.server [ start | stop | restart | status ] <options> <mysqld_options>
```

### Options

If an unknown option is provided to `mysqld_safe` on the command-line, then it is passed to `mysqld_safe`.

`mysql.server` supports the following options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--basedir=path</code></td><td>The path to the MariaDB installation directory.</td></tr>
<tr><td><code>--datadir=path</code></td><td>The path to the MariaDB data directory.</td></tr>
<tr><td><code>--pid-file=file_name</code></td><td>The path name of the file in which the server should write its process ID. If not provided, the default, <code>host_name.pid</code> is used.</td></tr>
<tr><td><code>--service-startup-timeout=file_name</code></td><td>How long in seconds to wait for confirmation of server startup. If the server does not start within this time, <em>mysql.server</em> exits with an error. The default value is 900. A value of 0 means not to wait at all for startup.  Negative values mean to wait forever (no timeout).</td></tr>
<tr><td><code>--use-mysqld_safe</code></td><td>Use <a href="/kb/en/mysqld_safe/">mysqld_safe</a> to start the server. This is the default.</td></tr>
<tr><td><code>--use-manager</code></td><td>Use Instance Manager to start the server.</td></tr>
<tr><td><code>--user=user_name</code></td><td>The login user name to use for running <code>mysqld</code>.</td></tr>
</tbody></table>

### Option Files

In addition to reading options from the command-line, `mysql.server` can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

The following options relate to how MariaDB command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
</tbody></table>

#### Option Groups

`mysql.server` reads options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysql.server]</code></td><td>&nbsp;Options read by <code>mysql.server</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
</tbody></table>

`mysql.server` also reads options from the following server [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysqld]</code></td><td>&nbsp;Options read by <code>mysqld</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[server]</code></td><td>Options read by MariaDB Server.</td></tr>
<tr><td><code>[mysqld-X.Y]</code></td><td>&nbsp;Options read by a specific version of <code>mysqld</code>, which includes both MariaDB Server and MySQL Server. For example, <code>[mysqld-5.5]</code>.</td></tr>
<tr><td><code>[mariadb]</code></td><td>Options read by MariaDB Server.</td></tr>
<tr><td><code>[mariadb-X.Y]</code></td><td>&nbsp;Options read by a specific version of MariaDB Server.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[galera]</code></td><td>&nbsp;Options read by a galera-capable MariaDB Server. Available on systems compiled with Galera support.</td></tr>
</tbody></table>

### Customizing mysql.server

If you have installed MariaDB to a non-standard location, then you may need to edit the `mysql.server` script to get it to work right.

If you do not want to edit the `mysql.server` script itself, then `mysql.server` also sources a few other `sh` scripts. These files can be used to set any variables that might be needed to make the script work in your specific environment. The files are:

- /etc/default/mysql
- /etc/sysconfig/mysql
- /etc/conf.d/mysql

## Installed Locations

`mysql.server` can be found in the `support-files` directory under your MariaDB installation directory or in a MariaDB source distribution.

### Installed SysVinit Locations

On systems that use [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit), `mysql.server` may also be installed in other locations and with other names.

If you installed MariaDB on Linux using [RPMs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm), then the `mysql.server` script will be installed into the `/etc/init.d` directory with the name `mysql`. You need not install it manually.

#### Manually Installing with SysVinit

If you install MariaDB from [source](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source) or from a [binary tarball](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs) that does not install [mysql.server](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqlserver)
automatically, and if you are on a system that uses [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit), then you can manually install `mysql.server` with [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit). This is usually done by copying it to <code class="highlight fixed" style="white-space:pre-wrap">/etc/init.d/</code> and then creating specially named symlinks in the appropriate <code class="highlight fixed" style="white-space:pre-wrap">/etc/rcX.d/</code> directories (where 'X' is a number between 0 and 6).

In the examples below we will follow the historical convention of renaming the
 <code class="highlight fixed" style="white-space:pre-wrap">mysql.server</code> script to '<code class="highlight fixed" style="white-space:pre-wrap">mysql</code>' when we copy it to <code class="highlight fixed" style="white-space:pre-wrap">/etc/init.d/</code>.

The first step for most Linux distributions is to copy the `mysql.server` script to <code class="highlight fixed" style="white-space:pre-wrap">/etc/init.d/</code> and make it executable:

```sql
cd /path/to/your/mariadb-version/support-files/
cp mysql.server /etc/init.d/mysql
chmod +x /etc/init.d/mysql
```

Now all that is needed is to create the specially-named symlinks. On both RPM and Debian-based Linux distributions there are tools which do this for you. Consult your distribution's documentation if neither of these work for you and follow their instructions for generating the symlinks or creating them manually.

On RPM-based distributions (like Fedora and CentOS), you use <code class="highlight fixed" style="white-space:pre-wrap">chkconfig</code>:

```sql
chkconfig --add mysql
chkconfig --level 345 mysql on
```

On Debian-based distributions you use <code class="highlight fixed" style="white-space:pre-wrap">update-rc.d</code>:

```sql
update-rc.d mysql defaults
```

On FreeBSD, the location for startup scripts is
 <code class="highlight fixed" style="white-space:pre-wrap">/usr/local/etc/rc.d/</code> and when you copy the
 <code class="highlight fixed" style="white-space:pre-wrap">mysql.server</code> script there you should rename it so that it matches the <code class="highlight fixed" style="white-space:pre-wrap">*.sh</code> pattern, like so:

```sql
cd /path/to/your/mariadb/support-files/
cp mysql.server /usr/local/etc/rc.d/mysql.server.sh
```

As stated above, consult your distribution's documentation for more information on starting services like MariaDB at system startup.

See [mysqld startup options](/kb/en/mysqld-startup-options/) for information on configuration options for `mysqld`.