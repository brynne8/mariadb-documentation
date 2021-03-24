# Authentication Plugin - Unix Socket

##### MariaDB starting with [10.4.3](/kb/en/mariadb-1043-release-notes/)

In [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later, the `unix_socket` authentication plugin is installed by default, and it is used by the `'root'@'localhost'` user account by default. See [Authentication from MariaDB 10.4](/mariadb-administration/user-server-security/user-account-management/authentication-from-mariadb-104/) for more information.

The `unix_socket` authentication plugin allows the user to use operating system credentials when connecting to MariaDB via the local Unix socket file. This Unix socket file is defined by the [socket](/kb/en/server-system-variables/#socket) system variable.

The `unix_socket` authentication plugin works by calling the [getsockopt](http://man7.org/linux/man-pages/man7/socket.7.html) system call with the `SO_PEERCRED` socket option, which allows it to retrieve the `uid` of the process that is connected to the socket. It is then able to get the user name associated with that `uid`. Once it has the user name, it will authenticate the connecting user as the MariaDB account that has the same user name.

The `unix_socket` authentication plugin is not suited to multiple Unix users accessing a single MariaDB user account.

## Security

A `unix_socket` authentication plugin is a passwordless security mechanism. Its security is in the strength of the access to the Unix user rather than the complexity and the secrecy of the password. As the security is different from passwords, the strengths and weaknesses need to be considered, and these aren't the same in every installation.

### Strengths

- Access is limited to the Unix user so, for example, a `www-data` user cannot access `root` with the `unix_socket` authentication plugin.
- There is no password to brute force.
- There is no password that can be accidentally exposed by user accident, poor security on backups, or poor security on passwords in configuration files.
- Default Unix user security is usually strong on preventing remote access and password brute force attempts.

### Weaknesses

The strength of a `unix_socket` authentication plugin is effectively the strength of the security of the Unix users on the system. The Unix user default installation in most cases is sufficiently secure, however, business requirements or unskilled management may expose risks. The following is a non-exhaustive list of potential Unix user security issues that may arise.

- Common access areas without screen locks, where an unauthorized user accesses the logged in Unix user of an authorized user.
- Extensive sudo access grants that provide users with access to execute commands of a different Unix user.
- Scripts writable by Unix users other than the Unix user that are executed (cron or directly) by the unix user.
- Web pages that are susceptible to command injection, where the Unix user running the web page has elevated privileges in the database that weren't intended to be used.
- Poor Unix user password practices including weak user passwords, password exposure and password reuse accompanied by an access vulnerability/mechanism of an unauthorized user to exploit this weakness.
- Weak remote access mechanisms and network file system privileges.
- Poor user security behavior including running untrusted scripts and software.

In some of these scenarios a database password may prevent these security exploits, however it will remove all the strengths of the `unix_socket` authentication plugin previously mentioned.

## Disabling the Plugin

##### MariaDB starting with [10.4.3](/kb/en/mariadb-1043-release-notes/)

In [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later, the `unix_socket` authentication plugin is installed by default, so <strong>if you do not want it to be available by default on those versions, then you will need to disable it</strong>.

The `unix_socket` authentication plugin is also installed by default in <strong>new installations</strong> that use the [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files/) packages provided by Debian's default repositories in Debian 9 and later and Ubuntu's default repositories in Ubuntu 15.10 and later, so <strong>if you do not want it to be available by default on those systems when those packages are used, then you will need to disable it</strong>. See [Differences in MariaDB in Debian (and Ubuntu)](/mariadb-administration/getting-installing-and-upgrading-mariadb/troubleshooting-installation-issues/installation-issues-on-debian-and-ubuntu/differences-in-mariadb-in-debian-and-ubuntu/) for more information.

The `unix_socket` authentication plugin can be disabled by starting the server with the <a undefined>unix_socket</a> option set to `OFF`. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
unix_socket=OFF
```

As an alternative, the <a undefined>unix_socket</a> option can also be set to `OFF` by pairing the option with the `disable` [option prefix](/kb/en/configuring-mariadb-with-option-files/#option-prefixes). For example:

```sql
[mariadb]
...
disable_unix_socket
```

## Installing the Plugin

##### MariaDB starting with [10.4.3](/kb/en/mariadb-1043-release-notes/)

In [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later, the `unix_socket` authentication plugin is installed by default, so <strong>this step can be skipped on those versions</strong>.

The `unix_socket` authentication plugin is also installed by default in <strong>new installations</strong> that use the [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files/) packages provided by Debian's default repositories in Debian 9 and later and Ubuntu's default repositories in Ubuntu 15.10 and later, so <strong>this step can be skipped on those systems when those packages are used</strong>. See [Differences in MariaDB in Debian (and Ubuntu)](/mariadb-administration/getting-installing-and-upgrading-mariadb/troubleshooting-installation-issues/installation-issues-on-debian-and-ubuntu/differences-in-mariadb-in-debian-and-ubuntu/) for more information.

In other systems, although the plugin's shared library is distributed with MariaDB by default as `auth_socket.so`, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). For example:

```sql
INSTALL SONAME 'auth_socket';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = auth_socket
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'auth_socket';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Creating Users

To create a user account via [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/), specify the name of the plugin in the <a undefined>IDENTIFIED VIA</a> clause. For example:

```sql
CREATE USER username@hostname IDENTIFIED VIA unix_socket;
```

If [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) does not have `NO_AUTO_CREATE_USER` set, then you can also create the user account via [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/). For example:

```sql
GRANT SELECT ON db.* TO username@hostname IDENTIFIED VIA unix_socket;
```

## Switching to Password-based Authentication

Sometimes Unix socket authentication does not meet your needs, so it can be desirable to switch a user account back to password-based authentication. This can easily be done by telling MariaDB to use another [authentication plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins/) for the account by executing the [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user/) statement. The specific authentication plugin is specified with the <a undefined>IDENTIFIED VIA</a> clause. For example, if you wanted to switch to the [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password/) authentication plugin, then you could execute:

```sql
ALTER USER root@localhost IDENTIFIED VIA mysql_native_password;
SET PASSWORD = PASSWORD('foo');
```

Note that if your operating system has scripts that require password-less access to MariaDB, then this may break those scripts. You may be able to fix that by setting a password in the `[client]` [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in your /root/.my.cnf [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[client]
password=foo
```

## Client Authentication Plugins

The `unix_socket` authentication plugin does not require any specific client authentication plugins. It should work with all clients.

## Support in Client Libraries

The `unix_socket` authentication plugin does not require any special support in client libraries. It should work with all client libraries.

## Example

```sql
$ mysql -uroot
MariaDB []> CREATE USER serg IDENTIFIED VIA unix_socket;
MariaDB []> CREATE USER monty IDENTIFIED VIA unix_socket;
MariaDB []> quit
Bye
$ whoami
serg
$ mysql --user=serg
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 2
Server version: 5.2.0-MariaDB-alpha-debug Source distribution
MariaDB []> quit
Bye
$ mysql --user=monty
ERROR 1045 (28000): Access denied for user 'monty'@'localhost' (using password: NO)
```

In this example, a user `serg` is already logged into the operating system and has full shell access. He has already authenticated with the operating system and his MariaDB account is configured to use the `unix_socket` authentication plugin, so he does not need to authenticate again for the database. MariaDB accepts his operating system credentials and allows him to connect. However, any attempt to connect to the database as another operating system user will be denied.

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10011-release-notes/">MariaDB 10.0.11</a></td></tr>
<tr><td>1.0</td><td>Beta</td><td><a href="/kb/en/mariadb-520-release-notes/">MariaDB 5.2.0</a></td></tr>
</tbody></table>

## Options

### `unix_socket`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the [mysql.plugin](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlplugin-table/) table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--unix-socket=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---

## See Also

- [Differences in MariaDB in Debian (and Ubuntu)](/mariadb-administration/getting-installing-and-upgrading-mariadb/troubleshooting-installation-issues/installation-issues-on-debian-and-ubuntu/differences-in-mariadb-in-debian-and-ubuntu/)
- [Authentication from MariaDB 10.4](/mariadb-administration/user-server-security/user-account-management/authentication-from-mariadb-104/)
- [Authentication from MariaDB 10 4 video tutorial](https://www.youtube.com/watch?v=aWFG4uLbimM)