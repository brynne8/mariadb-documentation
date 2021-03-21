# Authentication Plugin - PAM

The `pam` authentication plugin allows MariaDB to offload user authentication to the system's [Pluggable Authentication Module (PAM)](http://en.wikipedia.org/wiki/Pluggable_authentication_module) framework. PAM is an authentication framework used by Linux, FreeBSD, Solaris, and other Unix-like operating systems.

<strong>Note:</strong> Windows does not support PAM, so the `pam` authentication plugin does not support Windows. However, one can use a MariaDB client on Windows to connect to MariaDB server that is installed on a Unix-like operating system and that is configured to use the `pam` authentication plugin. For an example of how to do this, see the blog post: [MariaDB: Improve Security with Two-Step Verification](https://mariadb.org/improve-security-with-two-step-verification/).

## Use Cases

PAM makes it possible to implement various authentication scenarios of
different complexity. For example,

- Authentication using passwords from `/etc/shadow` (indeed, this is what a default PAM configuration usually does). See the <a undefined>pam_unix</a> PAM module.
- Authentication using LDAP. See the <a undefined>pam_ldap</a> PAM module.
- Authentication using Microsoft's Active Directory. See the <a undefined>pam_lsass</a>, <a undefined>pam_winbind</a>, and <a undefined>pam_centrifydc</a> PAM modules.
- Authentication using one-time passwords (even with SMS confirmation!). See the <a undefined>pam_google_authenticator</a> and <a undefined>pam_securid</a> PAM modules.
- Authentication using SSH keys. See the <a undefined>pam_ssh</a> PAM module.
- User and group mapping. See the <a undefined>pam_user_map</a> PAM module.
- Combining different authentication modules in interesting ways in a [PAM service](#configuring-the-pam-service).
- Password expiration.
- Limiting access by time, date, day of the week, etc. See the <a undefined>pam_time</a> PAM module.
- Logging every login attempt.
- and so on, the list is in no way exhaustive.

## Installing the Plugin

The `pam` authentication plugin's library is provided in [binary packages](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages) in all releases on Linux.

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin). For example:

```sql
INSTALL SONAME 'auth_pam';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
plugin_load_add = auth_pam
```

### Installing the v1 Plugin

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

Starting in [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/), the `auth_pam` shared library actually refers to version `2.0` of the `pam` authentication plugin. [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/) and later also provides version `1.0` of the plugin as the `auth_pam_v1` shared library.

In [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/) and later, if you need to install version `1.0` of the authentication plugin instead of version `2.0`, then you can do so. For example, with [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin):

```sql
INSTALL SONAME 'auth_pam_v1';
```

Or by specifying in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

```sql
[mariadb]
...
plugin_load_add = auth_pam_v1
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin). For example:

```sql
UNINSTALL SONAME 'auth_pam';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

### Uninstalling the v1 Plugin

If you installed version `1.0` of the authentication plugin, then you can uninstall that by executing a similar statement for `auth_pam_v1`. For example:

```sql
UNINSTALL SONAME 'auth_pam_v1';
```

## Configuring PAM

The `pam` authentication plugin tells MariaDB to delegate the authentication to the PAM authentication framework. How exactly that authentication is performed depends on how PAM was configured.

### Configuring the PAM Service

PAM is divided into `services`. PAM services are configured by [PAM configuration files](http://www.linux-pam.org/Linux-PAM-html/sag-configuration-file.html). Typically, the global PAM configuration file is located at `/etc/pam.conf` and [PAM directory-based configuration files](http://www.linux-pam.org/Linux-PAM-html/sag-configuration-directory.html) for individual services are located in `/etc/pam.d/`.

If you want to use a PAM service called `mariadb` for your MariaDB PAM authentication, then the PAM configuration file for that service would also be called `mariadb`, and it would typically be located at `/etc/pam.d/mariadb`.

For example, here is a minimal PAM service configuration file that performs simple password authentication with UNIX passwords:

```sql
auth required pam_unix.so audit
account required pam_unix.so audit
```

Let's breakdown this relatively simple PAM service configuration file.

Each line of a PAM service configuration file has the following general format:

<em>type control module-path module-arguments</em>

The above PAM service configuration file instructs the PAM authentication framework that for successful <strong>authentication</strong> (i.e. type=<a undefined>auth</a>), it is <strong>required</strong> that the `pam_unix.so` PAM module returns a success.

The above PAM service configuration file also instructs the PAM authentication framework that for an <strong>account</strong> (i.e. type=<a undefined>account</a>) to be valid, it is <strong>required</strong> that the `pam_unix.so` PAM module returns a success.

PAM also supports <a undefined>session</a> and <a undefined>password</a> types, but MariaDB's `pam` authentication plugin does not support those.

The above PAM service configuration file also provides the `audit` module argument to the `pam_unix` PAM module. The [pam_unix manual](https://linux.die.net/man/8/pam_unix) says that this module argument enables extreme debug logging to the syslog.

On most systems, you can find many other examples of PAM service configuration files in your `/etc/pam.d/` directory.

#### Configuring the pam_unix PAM Module

If you configure PAM to use the <a undefined>pam_unix</a> PAM module (as in the above example), then you might notice on some systems that this will fail by default with errors like the following:

```sql
Apr 14 12:56:23 localhost unix_chkpwd[3332]: check pass; user unknown
Apr 14 12:56:23 localhost unix_chkpwd[3332]: password check failed for user (alice)
Apr 14 12:56:23 localhost mysqld: pam_unix(mysql:auth): authentication failure; logname= uid=991 euid=991 tty= ruser= rhost=  user=alice
```

The problem is that on some systems, the `pam_unix` PAM module needs access to `/etc/shadow` in order to function, and most systems only allow `root` to access that file by default.

Newer versions of PAM do not have this limitation, so you may want to try upgrading your version of PAM to see if that fixes the issue.

If that does not work, then you can work around this problem by giving the user that runs [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) access to `/etc/shadow`. For example, if the `mysql` user runs [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options), then you could do the following:

```sql
sudo groupadd shadow
sudo usermod -a -G shadow mysql
sudo chown root:shadow /etc/shadow
sudo chmod g+r /etc/shadow
```

And then you would have to [restart the server](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

At that point, the server should be able to read `/etc/shadow`.

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

Starting in [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/), the `pam` authentication plugin uses a <a undefined>setuid</a> wrapper to perform its PAM checks, so it should not need any special workarounds to perform privileged operations, such as reading `/etc/shadow` when using the `pam_unix` PAM module. See [MDEV-7032](https://jira.mariadb.org/browse/MDEV-7032) for more information.

## Creating Users

Similar to all other [authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins), to create a user in MariaDB which uses the `pam` authentication plugin, you would execute [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user) while specifying the name of the plugin in the <a undefined>IDENTIFIED VIA</a> clause. For example:

```sql
CREATE USER username@hostname IDENTIFIED VIA pam;
```

If [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) does not have `NO_AUTO_CREATE_USER` set, then you can also create the user this way with [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant). For example:

```sql
GRANT SELECT ON db.* TO username@hostname IDENTIFIED VIA pam;
```

You can also specify a [PAM service name](#configuring-the-pam-service) for MariaDB to use by providing it with the `USING` clause. For example:

```sql
CREATE USER username@hostname IDENTIFIED VIA pam USING 'mariadb';
```

This line creates a user that needs to be authenticated via the `pam` authentication plugin using the [PAM service name](#configuring-the-pam-service) `mariadb`. As mentioned in a previous section, this service's configuration file will typically be present in `/etc/pam.d/mariadb`.

If no service name is specified, then the plugin will use `mysql` as the default [PAM service name](#configuring-the-pam-service).

## Client Authentication Plugins

For clients that use the `libmysqlclient` or [MariaDB Connector/C](/kb/en/mariadb-connector-c/) libraries, MariaDB provides two client authentication plugins that are compatible with the `pam` authentication plugin:

- `dialog`
- `mysql_clear_password`

When connecting with a [client or utility](/clients-utilities) to a server as a user account that authenticates with the `pam` authentication plugin, you may need to tell the client where to find the relevant client authentication plugin by specifying the `--plugin-dir` option. For example:

```sql
mysql --plugin-dir=/usr/local/mysql/lib64/mysql/plugin --user=alice
```

Both the `dialog` and the `mysql_clear_password` client authentication plugins transmit the password to the server in clear text. Therefore, when you use the `pam` authentication plugin, it is incredibly important to [encrypt client connections  using TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption) to prevent the clear-text passwords from being seen by unauthorized users.

### `dialog`

Usually the `pam` authentication plugin uses the `dialog` client authentication plugin to communicate with the user. This client authentication plugin allows MariaDB to support arbitrarily complex PAM configurations with regular or one-time passwords, challenge-response, multiple questions, or just about anything else. When using a MariaDB client library, there is no need to install or enable anything — the `dialog` client authentication plugin is loaded by the client library completely automatically and transparently for the application.

The `dialog` client authentication plugin was developed by MariaDB, so MySQL's clients and client libraries as well as third party applications that bundle MySQL's client libraries do not support the `dialog` client authentication plugin out of the box. If the server tells an unsupported client to use the `dialog` client authentication plugin, then the client is likely to throw an error like the following:

```sql
ERROR 2059 (HY000): Authentication plugin 'dialog' cannot be loaded: /usr/lib/mysql/plugin/dialog.so: cannot open shared object file: No such file or directory
```

For some libraries or applications, this problem can be fixed by copying `dialog.so` or `dialog.dll` from a MariaDB client installation that is compatible with the system into the system's MySQL client authentication plugin directory. However, not all clients are compatible with the `dialog` client authentication plugin, so this may not work for every client.

If your client does not support the `dialog` client authentication plugin, then you may need to use the <a undefined>mysql_clear_password</a> client authentication plugin instead.

The `dialog` client authentication plugin transmits the password to the server in clear text. Therefore, when you use the `pam` authentication plugin, it is incredibly important to [encrypt client connections using TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption) to prevent the clear-text passwords from being seen by unauthorized users.

### `mysql_clear_password`

##### MariaDB starting with [5.5.32](/kb/en/mariadb-5532-release-notes/)

The `mysql_clear_password` client authentication plugin and the <a undefined>pam_use_cleartext_plugin</a> system variable were first added in [MariaDB 5.5.32](/kb/en/mariadb-5532-release-notes/) and [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/).

Users can instruct the `pam` authentication plugin to use the `mysql_clear_password` client authentication plugin instead of the <a undefined>dialog</a> client authentication plugin by configuring the <a undefined>pam_use_cleartext_plugin</a> system variable on the server. It can be set in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
pam_use_cleartext_plugin
```

It is important to note that the `mysql_clear_password` plugin has very limited functionality.

- The `mysql_clear_password` client authentication plugin only supports PAM services that require password-based authentication.

- The `mysql_clear_password` client authentication plugin also only supports PAM services that ask the user a single question.

- If the PAM service requires challenge-responses, multiple questions, or other similar complicated authentication schemes, then the PAM service is not compatible with `mysql_clear_password` client authentication plugin. In that case, the <a undefined>dialog</a> client authentication plugin will have to be used instead.

The `mysql_clear_password` client authentication plugin transmits the password to the server in clear text. Therefore, when you use the `pam` authentication plugin, it is incredibly important to [encrypt client connections using TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption) to prevent the clear-text passwords from being seen by unauthorized users.

#### Compatiblity with MySQL Clients and Client Libraries

The `mysql_clear_password` client authentication plugin is similar to MySQL's <a undefined>mysql_clear_password</a> client authentication plugin.

The `mysql_clear_password` client authentication plugin is compatible with MySQL clients and most MySQL client libraries, while the <a undefined>dialog</a> client authentication plugin is not always compatible with them. Therefore, the `mysql_clear_password` client authentication plugin is most useful if you need some kind of MySQL compatibility in your environment, but you still want to use the `pam` authentication plugin.

Even though the `mysql_clear_password` client authentication plugin is compatible with MySQL clients and most MySQL client libraries, the `mysql_clear_password` client authentication plugin may be disabled by default by these clients and client libraries. For example, MySQL's version of the <a undefined>mysql</a> command-line client has the <a undefined>--enable-cleartext-plugin</a> option that must be set in order to use the `mysql_clear_password` client authentication plugin. For example:

```sql
mysql --enable-cleartext-plugin --user=alice -p 
```

Other clients may require other methods to enable the authentication plugin. For example, [MySQL Workbench](https://www.mysql.com/products/workbench/) has a checkbox titled <strong>Enable Cleartext Authentication Plugin</strong> under the <strong>Advanced</strong> tab on the connection configuration screen.

For applications that use MySQL's `libmysqlclient`, the authentication plugin can be enabled by setting the `MYSQL_ENABLE_CLEARTEXT_PLUGIN` option with the `mysql_options()` function. For example:

```sql
mysql_options(mysql, MYSQL_ENABLE_CLEARTEXT_PLUGIN, 1);
```

For MySQL compatibility, [MariaDB Connector/C](/kb/en/mariadb-connector-c/) also allows applications to set the `MYSQL_ENABLE_CLEARTEXT_PLUGIN` option with the <a undefined>mysql_optionsv</a> function. However, this option does not actually do anything in [MariaDB Connector/C](/kb/en/mariadb-connector-c/), because the `mysql_clear_password` client authentication plugin is always enabled for MariaDB clients and client libraries.

## Support in Client Libraries

### Using the Plugin with MariaDB Connector/C

[MariaDB Connector/C](/kb/en/mariadb-connector-c/) supports `pam` authentication using the [client authentication plugins](client-authentication-plugins) mentioned in the previous section since MariaDB Connector/C 2.1.0, regardless of the value of the <a undefined>pam_use_cleartext_plugin</a> system variable.

### Using the Plugin with MariaDB Connector/ODBC

[MariaDB Connector/ODBC](/kb/en/mariadb-connector-odbc/) supports `pam` authentication using the [client authentication plugins](client-authentication-plugins) mentioned in the previous section since MariaDB Connector/ODBC 1.0.0, regardless of the value of the <a undefined>pam_use_cleartext_plugin</a> system variable.

### Using the Plugin with MariaDB Connector/J

[MariaDB Connector/J](/kb/en/mariadb-connector-j/) supports `pam v1` authentication since MariaDB Connector/J 1.4.0, regardless of the value of the <a undefined>pam_use_cleartext_plugin</a> system variable.

[MariaDB Connector/J](/kb/en/mariadb-connector-j/) supports `pam v2` authentication since MariaDB Connector/J 2.4.4, regardless of the value of the <a undefined>pam_use_cleartext_plugin</a> system variable.

### Using the Plugin with MariaDB Connector/Node.js

[MariaDB Connector/Node.js](/kb/en/nodejs-connector/) supports `pam` authentication since MariaDB Connector/Node.js 0.7.0, regardless of the value of the <a undefined>pam_use_cleartext_plugin</a> system variable.

### Using the Plugin with MySqlConnector for .NET

[MySqlConnector for ADO.NET](/kb/en/mysqlconnector-for-adonet/) supports `pam` authentication since MySqlConnector 0.20.0, but only if the <a undefined>pam_use_cleartext_plugin</a> system variable is enabled on the server.

## Logging

### PAM Module Logging

Errors and messages from PAM modules are usually logged using the [syslog](https://linux.die.net/man/8/rsyslogd) daemon with the `authpriv` facility. To determine the specific log file where the `authpriv` facility is logged, you can check <a undefined>rsyslog.conf</a>.

On RHEL, CentOS, Fedora, and other similar Linux distributions, the default location for these messages is usually `/var/log/secure`.

On Debian, Ubuntu, and other similar Linux distributions, the default location for these messages is usually `/var/log/auth.log`.

For example, the syslog can contain messages like the following when MariaDB's `pam` authentication plugin is configured to use the <a undefined>pam_unix</a> PAM module, and the user enters an incorrect password:

```sql
Jan  9 05:35:41 ip-172-30-0-198 unix_chkpwd[1205]: password check failed for user (foo)
Jan  9 05:35:41 ip-172-30-0-198 mysqld: pam_unix(mariadb:auth): authentication failure; logname= uid=997 euid=997 tty= ruser= rhost=  user=foo
```

### PAM Authentication Plugin's Debug Logging

MariaDB's `pam` authentication plugin can also log additional verbose debug logging to the [error log](/mariadb-administration/server-monitoring-logs/error-log). This is only done if the plugin is a [debug build](/kb/en/compiling-mariadb-for-debugging/) and if <a undefined>pam_debug</a> is set.

The output looks like this:

```sql
PAM: pam_start(mariadb, alice)
PAM: pam_authenticate(0)
PAM: conv: send(Enter PASSCODE:)
PAM: conv: recv(123456789)
PAM: pam_acct_mgmt(0)
PAM: pam_get_item(PAM_USER)
PAM: status = 0 user = ��\>
```

### Custom Logging with pam_exec

The <a undefined>pam_exec</a> PAM module can be used to implement some custom logging. This can be very useful when debugging certain kinds of issues.

For example, first, create a script that writes the log output:

```sql
tee /tmp/pam_log_script.sh <<EOF
#!/bin/bash
echo "\${PAM_SERVICE}:\${PAM_TYPE} - \${PAM_RUSER}@\${PAM_RHOST} is authenticating as \${PAM_USER}" 
EOF
chmod 0775 /tmp/pam_log_script.sh
```

And then change the [PAM service configuration](#configuring-the-pam-service) to execute the script using the <a undefined>pam_exec</a> PAM module. For example:

```sql
auth optional pam_exec.so log=/tmp/pam_output.txt /tmp/pam_log_script.sh
auth required pam_unix.so audit
account optional pam_exec.so log=/tmp/pam_output.txt /tmp/pam_log_script.sh
account required pam_unix.so audit
```

Whenever the above PAM service is used, the output of the script will be written to `/tmp/pam_output.txt`. It may look similar to this:

```sql
*** Tue May 14 14:53:23 2019
mariadb:auth - @ is authenticating as alice
*** Tue May 14 14:53:25 2019
mariadb:account - @ is authenticating as alice
*** Tue May 14 14:53:28 2019
mariadb:auth - @ is authenticating as alice
*** Tue May 14 14:53:31 2019
mariadb:account - @ is authenticating as alice
```

## User and Group Mapping

Even when using the `pam` authentication plugin, the authenticating PAM user account still needs to exist in MariaDB, and the account needs to have privileges in the database. Creating these MariaDB accounts and making sure the privileges are correct can be a lot of work. To decrease the amount of work involved, some users would like to be able to map a PAM user to a different MariaDB user. For example, let’s say that `alice` and `bob` are both DBAs. It would be nice if each of them could log into MariaDB with their own PAM username and password, while MariaDB sees both of them as the same `dba` user. That way, there is only one MariaDB account to keep track of. See [User and Group Mapping with PAM](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/user-and-group-mapping-with-pam) for more information on how to do this.

## PAM Modules

There are many PAM modules. The ones described below are the ones that have been seen most often by MariaDB.

### pam_unix

The <a undefined>pam_unix</a> PAM module provides support for Unix password authentication. It is the default PAM module on most systems.

For a tutorial on setting up PAM authentication and user or group mapping with Unix authentication, see [Configuring PAM Authentication and User Mapping with Unix Authentication](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/configuring-pam-authentication-and-user-mapping-with-unix-authentication).

### pam_user_map

The [pam_user_map](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/user-and-group-mapping-with-pam) PAM module was developed by MariaDB to support user and group mapping.

### pam_ldap

The <a undefined>pam_ldap</a> PAM module provides support for LDAP authentication.

For a tutorial on setting up PAM authentication and user or group mapping with LDAP authentication, see [Configuring PAM Authentication and User Mapping with LDAP Authentication](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/configuring-pam-authentication-and-user-mapping-with-ldap-authentication).

This can also be configured for [Active Directory](https://en.wikipedia.org/wiki/Active_Directory) authentication.

### pam_sss

The <a undefined>pam_sss</a> PAM module provides support for authentication with [System Security Services Daemon (SSSD)](https://en.wikipedia.org/wiki/System_Security_Services_Daemon).

This can be configured for [Active Directory](https://en.wikipedia.org/wiki/Active_Directory) authentication.

### pam_lsass

The `pam_lsass` PAM module provides support for [Active Directory](https://en.wikipedia.org/wiki/Active_Directory) authentication. It is provided by [PowerBroker Identity Services – Open Edition](https://github.com/BeyondTrust/pbis-open/wiki).

### pam_winbind

The <a undefined>pam_winbind</a> PAM module provides support for [Active Directory](https://en.wikipedia.org/wiki/Active_Directory) authentication. It is provided by [winbindd](https://www.samba.org/samba/docs/current/man-html/winbindd.8.html) from the [samba](https://www.samba.org/samba/docs/current/man-html/samba.7.html) suite.

This PAM module converts all provided user names to lowercase. There is no way to disable this functionality. If you do not want to be forced to use all lowercase user names, then you may need to configure the <a undefined>pam_winbind_workaround</a> system variable. See [MDEV-18686](https://jira.mariadb.org/browse/MDEV-18686) for more information.

### pam_centrifydc

The <a undefined>pam_centrifydc</a> PAM module provides support for [Active Directory](https://en.wikipedia.org/wiki/Active_Directory) authentication. It integrates with the commercial [Active Directory Bridge from Centrify](https://www.centrify.com/products/infrastructure-services/authentication/active-directory-bridge/integration/).

### pam_krb5

The <a undefined>pam_krb5</a> PAM module provides support for [Kerberos](https://en.wikipedia.org/wiki/Kerberos_(protocol)) authentication.

This can be configured for [Active Directory](https://en.wikipedia.org/wiki/Active_Directory) authentication.

### pam_google_authenticator

The `pam_google_authenticator` PAM module provides two-factor identification with Google Authenticator. It is from Google's <a undefined>google-authenticator-libpam</a> open-source project. The PAM module should work with the open-source mobile apps built by Google's <a undefined>google-authenticator</a> and <a undefined>google-authenticator-android</a> projects as well as the the closed source Google Authenticator mobile apps that are present in each mobile app store.

For an example of how to use this PAM module, see the blog post: [MariaDB: Improve Security with Two-Step Verification](https://mariadb.org/improve-security-with-two-step-verification/).

### pam_securid

The `pam_securid` PAM module provides support for multi-factor authentication. It is part of the commercial [RSA SecurID Suite](https://www.rsa.com/en-us/products/rsa-securid-suite).

Note that current versions of this module are not [safe for multi-threaded environments](#multi-threaded-issues), and the vendor does not officially support the product on MariaDB. See [MDEV-10361](https://jira.mariadb.org/browse/MDEV-10361) about that. However, the module may work with [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/) and above.

### pam_ssh

The <a undefined>pam_ssh</a> PAM module provides authentication using SSH keys.

### pam_time

The <a undefined>pam_time</a> PAM module provides time-controlled access.

## Known Issues

### Multi-Threaded Issues

MariaDB is a multi-threaded program, which means that different connections concurrently run in different threads. Current versions of MariaDB's `pam` authentication plugin execute PAM module code in the server address space. This means that any PAM modules used with MariaDB must be safe for multi-threaded environments. Otherwise, if multiple clients try to authenticate with the same PAM module in parallel, undefined behavior can occur. For example, the `pam_fprintd` PAM module is not safe for multi-threaded environments, and if you use it with MariaDB, you may experience server crashes.

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

Starting in [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/), the `pam` authentication plugin isolates PAM module code from the server address space, so even PAM modules that are known to be unsafe for multi-threaded environments should not cause issues with MariaDB. See [MDEV-15473](https://jira.mariadb.org/browse/MDEV-15473) for more information.

### Conflicts with Password Validation

When a [password validation plugin](/columns-storage-engines-and-plugins/plugins/password-validation-plugins) is enabled, MariaDB won't allow an account to be created if the password validation plugin says that the account's password is too weak. This creates a problem for accounts that authenticate with the `pam` authentication plugin, since MariaDB has no knowledge of the user's password. When a user tries to create an account that authenticates with the `pam` authentication plugin, the password validation plugin would throw an error, even with <a undefined>strict_password_validation=OFF</a> set.

The workaround is to uninstall the [password validation plugin](/columns-storage-engines-and-plugins/plugins/password-validation-plugins) with [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin), and then create the account, and then reinstall the [password validation plugin](/kb/en/password-validation/) with [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin).

For example:

```sql
INSTALL PLUGIN simple_password_check SONAME 'simple_password_check';
Query OK, 0 rows affected (0.002 sec)

SET GLOBAL strict_password_validation=OFF;
Query OK, 0 rows affected (0.000 sec)

CREATE USER ''@'%' IDENTIFIED VIA pam USING 'mariadb';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
UNINSTALL PLUGIN simple_password_check;
Query OK, 0 rows affected (0.000 sec)

CREATE USER ''@'%' IDENTIFIED VIA pam USING 'mariadb';
Query OK, 0 rows affected (0.000 sec)

INSTALL PLUGIN simple_password_check SONAME 'simple_password_check';
Query OK, 0 rows affected (0.001 sec)
```

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

Starting in [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/), accounts that authenticate with the `pam` authentication plugin should be exempt from password validation checks. See [MDEV-12321](https://jira.mariadb.org/browse/MDEV-12321) and [MDEV-10457](https://jira.mariadb.org/browse/MDEV-10457) for more information.

### SELinux

[SELinux](/mariadb-administration/user-server-security/securing-mariadb/selinux) may cause issues when using the `pam` authentication plugin. For example, using <a undefined>pam_unix</a> with the `pam` authentication plugin while SELinux is enabled can sometimes lead to SELinux errors involving <a undefined>unix_chkpwd</a>, such as the following::

```sql
Apr 14 12:37:59 localhost setroubleshoot: Plugin Exception restorecon_source
Apr 14 12:37:59 localhost setroubleshoot: SELinux is preventing /usr/sbin/unix_chkpwd from execute access on the file . For complete SELinux messages. run sealert -l c56fe6e0-c78c-4bdb-a80f-27ef86a1ea85
Apr 14 12:37:59 localhost python: SELinux is preventing /usr/sbin/unix_chkpwd from execute access on the file .

*****  Plugin catchall (100. confidence) suggests   **************************

If you believe that unix_chkpwd should be allowed execute access on the  file by default.
Then you should report this as a bug.
You can generate a local policy module to allow this access.
Do
allow this access for now by executing:
# grep unix_chkpwd /var/log/audit/audit.log | audit2allow -M mypol
# semodule -i mypol.pp
```

Sometimes issues like this can be fixed by updating the system's SELinux policies. You may be able to update the policies using <a undefined>audit2allow</a>. See [SELinux: Generating SELinux Policies with audit2allow](/kb/en/selinux/#generating-selinux-policies-with-audit2allow) for more information.

If you can't get the `pam` authentication plugin to work with SELinux at all, then it can help to disable SELinux entirely. See [SELinux: Changing SELinux's Mode](/kb/en/selinux/#changing-selinuxs-mode) for information on how to do this.

## Tutorials

You may find the following PAM-related tutorials helpful:

- [Configuring PAM Authentication and User Mapping with Unix Authentication](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/configuring-pam-authentication-and-user-mapping-with-unix-authentication)
- [Configuring PAM Authentication and User Mapping with LDAP Authentication](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/configuring-pam-authentication-and-user-mapping-with-ldap-authentication)

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>2.0</td><td>Beta</td><td><a href="/kb/en/mariadb-1040-release-notes/">MariaDB 10.4.0</a></td></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10010-release-notes/">MariaDB 10.0.10</a></td></tr>
<tr><td>1.0</td><td>Beta</td><td><a href="/kb/en/mariadb-5210-release-notes/">MariaDB 5.2.10</a></td></tr>
</tbody></table>

## System Variables

### `pam_debug`

- <strong>Description:</strong> Enables verbose debug logging to the [error log](/mariadb-administration/server-monitoring-logs/error-log) for all authentication handled by the plugin.
<ul start="1"><li>This system variable is only available when the plugin is a [debug build](/kb/en/compiling-mariadb-for-debugging/).
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pam-debug</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), [MariaDB 10.1.17](/kb/en/mariadb-10117-release-notes/)

---

### `pam_use_cleartext_plugin`

- <strong>Description:</strong> Use the <a undefined>mysql_clear_password</a> client authentication plugin instead of the <a undefined>dialog</a> client authentication plugin. This may be needed for compatibility reasons, but it only supports simple PAM configurations that don't require any input besides a password.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pam-use-cleartext-plugin</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), [MariaDB 5.5.32](/kb/en/mariadb-5532-release-notes/)

---

### `pam_winbind_workaround`

- <strong>Description:</strong> Configures the authentication plugin to compare the user name provided by the client with the user name returned by the PAM module in a case insensitive manner. This may be needed if you use the <a undefined>pam_winbind</a> PAM module, which is known to convert all user names to lowercase, and which does not allow this behavior to be disabled.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pam-winbind-workaround</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`
- <strong>Introduced:</strong> [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/), [MariaDB 10.3.15](/kb/en/mariadb-10315-release-notes/), [MariaDB 10.2.24](/kb/en/mariadb-10224-release-notes/), [MariaDB 10.1.39](/kb/en/mariadb-10139-release-notes/)

---

## Options

### `pam`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the <a undefined>mysql.plugins</a> table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--pam=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---

## See Also

- [Writing a MariaDB PAM Authentication Plugin](https://mariadb.org/writing-a-mariadb-pam-authentication-plugin/)
- [MariaDB: Improve Security with Two-Step Verification](https://mariadb.org/improve-security-with-two-step-verification/)