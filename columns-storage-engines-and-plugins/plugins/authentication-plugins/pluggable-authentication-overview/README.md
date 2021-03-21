# Pluggable Authentication Overview

When a user attempts to log in, the authentication plugin controls how MariaDB Server determines whether the connection is from a legitimate user.

When creating or altering a user account with the [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant), [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user) or [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user) statements, you can specify the authentication plugin you want the user account to use by providing the `IDENTIFIED VIA` clause. By default, when you issue one of these statements for a user account without specifying an authentication plugin, MariaDB uses the [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) plugin.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, there are some notable changes, such as:

- You can specify multiple authentication plugins for each user account.
- The `root@localhost` user created by [mysql_install_db](/clients-utilities/mysql_install_db) is created with the ability to use two authentication plugins. First, it is configured to try to use the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin. This allows the the `root@localhost` user to login without a password via the local Unix socket file defined by the <a undefined>socket</a> system variable, as long as the login is attempted from a process owned by the operating system `root` user account. Second, if authentication fails with the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin, then it is configured to try to use the [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) authentication plugin. However, an invalid password is initially set, so in order to authenticate this way, a password must be set with [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password).

## Supported Authentication Plugins

The authentication process is a conversation between the server and a client. MariaDB implements both server-side and client-side authentication plugins.

### Supported Server Authentication Plugins

MariaDB provides seven server-side authentication plugins:

- [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password)
- [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password)
- [ed25519](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-ed25519)
- [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi)
- [pam](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam) (Unix only)
- [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) (Unix only)
- [named_pipe](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-named-pipe) (Windows only)

### Supported Client Authentication Plugins

MariaDB provides eight client-side authentication plugins:

- <a undefined>mysql_native_password</a>
- <a undefined>mysql_old_password</a>
- <a undefined>client_ed25519</a>
- <a undefined>auth_gssapi_client</a>
- <a undefined>dialog</a>
- <a undefined>mysql_clear_password</a>
- <a undefined>sha256_password</a>
- <a undefined>caching_sha256_password</a>

## Options Related to Authentication Plugins

### Server Options Related to Authentication Plugins

MariaDB supports the following server options related to authentication plugins:

<table><tbody><tr><th>Server Option</th><th>Description</th></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#old_passwords">old_passwords={1 | 0}</a></code></td><td>If set to <code>1</code> (<code>0</code> is default), MariaDB reverts to using the <code><a href="/kb/en/authentication-plugin-mysql_old_password/">mysql_old_password</a></code> authentication plugin by default for newly created users and passwords, instead of the <code><a href="/kb/en/authentication-plugin-mysql_native_password/">mysql_native_password</a></code> authentication plugin.</td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#plugin_dir">plugin_dir=path</a></code></td><td>Path to the <a href="/kb/en/mariadb-plugins/">plugin</a> directory. For security reasons, either make sure this directory can only be read by the server, or set <a href="#secure_file_priv">secure_file_priv</a>.</td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#plugin_maturity">plugin_maturity=level</a></code></td><td>The lowest acceptable <a href="/kb/en/mariadb-plugins/">plugin</a> maturity. MariaDB will not load plugins less mature than the specified level.</td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#secure_auth">secure_auth</a></code></td><td>Connections will be blocked if they use the the <code><a href="/kb/en/authentication-plugin-mysql_old_password/">mysql_old_password</a></code> authentication plugin. The server will also fail to start if the privilege tables are in the old, pre-MySQL 4.1 format.</td></tr>
</tbody></table>

### Client Options Related to Authentication Plugins

Most [clients and utilities](/clients-utilities) support some command line arguments related to client authentication plugins:

<table><tbody><tr><th>Client Option</th><th>Description</th></tr>
<tr><td><code>--connect-expired-password</code></td><td>Notify the server that this client is prepared to handle <a href="/kb/en/user-password-expiry/">expired password sandbox mode</a> even if <code>--batch</code> was specified. From <a href="/kb/en/mariadb-1043-release-notes/">MariaDB 10.4.3</a>.</td></tr>
<tr><td><code>--default-auth=name</code></td><td>Default authentication client-side plugin to use.</td></tr>
<tr><td><code>--plugin-dir=path</code></td><td>Directory for client-side plugins.</td></tr>
<tr><td><code>--secure-auth</code></td><td>Refuse to connect to the server if the server uses the <code><a href="/kb/en/authentication-plugin-mysql_old_password/">mysql_old_password</a></code> authentication plugin. This mode is off by default, which is a difference in behavior compared to MySQL 5.6 and later, where it is on by default.</td></tr>
</tbody></table>

Developers who are using [MariaDB Connector/C](/kb/en/mariadb-connector-c/) can implement similar functionality in their application by setting the following options with the <a undefined>mysql_optionsv</a> function:

- `MYSQL_OPT_CAN_HANDLE_EXPIRED_PASSWORDS`
- `MYSQL_PLUGIN_DIR`
- `MYSQL_DEFAULT_AUTH`
- `MYSQL_SECURE_AUTH`

For example:

```sql
mysql_optionsv(mysql, MYSQL_OPT_CAN_HANDLE_EXPIRED_PASSWORDS, 1);
mysql_optionsv(mysql, MYSQL_DEFAULT_AUTH, "name");
mysql_optionsv(mysql, MYSQL_PLUGIN_DIR, "path");
mysql_optionsv(mysql, MYSQL_SECURE_AUTH, 1);
```

### Installation Options Related to Authentication Plugins

[mysql_install_db](/clients-utilities/mysql_install_db) supports the following installation options related to authentication plugins:

<table><tbody><tr><th>Installation Option</th><th>Description</th></tr>
<tr><td><code>--auth-root-authentication-method={normal <code>|</code> socket}</code></td><td>If set to <code>normal</code>, it creates a <code>root@localhost</code> account that authenticates with the <code><a href="/kb/en/authentication-plugin-mysql_native_password/">mysql_native_password</a></code> authentication plugin and that has no initial password set, which can be insecure. If set to <code>socket</code>, it creates a <code>root@localhost</code> account that authenticates with the <code><a href="/kb/en/authentication-plugin-unix-socket/">unix_socket</a></code> authentication plugin. Set to <code>normal</code> by default. Available since <a href="/kb/en/what-is-mariadb-101/">MariaDB 10.1</a>.</td></tr>
<tr><td><code>--auth-root-socket-user=USER</code></td><td>Used with <code>--auth-root-authentication-method=socket</code>. It specifies the name of the second account to create with <code><a href="/kb/en/grant/#global-privileges">SUPER</a></code> privileges in addition to <code>root</code>, as well as of the system account allowed to access it. Defaults to the value of <code>--user</code>.</td></tr>
</tbody></table>

## Extended SQL Syntax

MariaDB has extended the SQL standard [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant), [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user), and [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user) statements, so that they support specifying different authentication plugins for specific users. An authentication plugin can be specified with these statements by providing the `IDENTIFIED VIA` clause.

For example, the [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) syntax is:

```sql
GRANT <privileges> ON <level> TO <user> 
   IDENTIFIED VIA <plugin> [ USING <string> ]
```

And the [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user) syntax is:

```sql
CREATE USER <user> 
   IDENTIFIED VIA <plugin> [ USING <string> ]
```

And the [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user) syntax is:

```sql
ALTER USER <user> 
   IDENTIFIED VIA <plugin> [ USING <string> ]
```

The optional `USING` clause allows users to provide an authentication string to a plugin. The authentication string's format and meaning is completely defined by the plugin.

For example, for the [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) authentication plugin, the authentication string should be a password hash:

```sql
CREATE USER mysqltest_up1 
   IDENTIFIED VIA mysql_native_password USING '*E8D46CE25265E545D225A8A6F1BAF642FEBEE5CB';
```

Since [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) is the default authentication plugin, the above is just another way of saying the following:

```sql
CREATE USER mysqltest_up1 
   IDENTIFIED BY PASSWORD '*E8D46CE25265E545D225A8A6F1BAF642FEBEE5CB';
```

In contrast, for the [pam](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam) authentication plugin, the authentication string should refer to a [PAM service name](/kb/en/authentication-plugin-pam/#configuring-the-pam-service):

```sql
CREATE USER mysqltest_up1 
   IDENTIFIED VIA pam USING 'mariadb';
```

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, a user account can be associated with multiple authentication plugins.

For example, to configure the `root@localhost` user account to try the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin, followed by the [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) authentication plugin as a backup, you could execute the following:

```sql
CREATE USER root@localhost 
   IDENTIFIED VIA unix_socket 
   OR mysql_native_password USING PASSWORD("verysecret");
```

See [Authentication from MariaDB 10.4](/mariadb-administration/user-server-security/user-account-management/authentication-from-mariadb-104) for more information.

## Authentication Plugins Installed by Default

### Server Authentication Plugins Installed by Default

Not all server-side authentication plugins are installed by default. If a specific server-side authentication plugin is not installed by default, then you can find the installation procedure on the documentation page for the specific authentication plugin.

<br>

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, the following server-side authentication plugins are installed by default:

- The [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) and [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password) authentication plugins authentication plugins are installed by default in all builds.

- The [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin is installed by default in all builds on Unix and Linux.

- The [named_pipe](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-named-pipe) authentication plugin is installed by default in all builds on Windows.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and below, the following server-side authentication plugins are installed by default:

- The [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) and [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password) authentication plugins are installed by default in all builds.

- The [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin is installed by default in <strong>new installations</strong> that use the [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by Debian's default repositories in Debian 9 and later and Ubuntu's default repositories in Ubuntu 15.10 and later. See [Differences in MariaDB in Debian (and Ubuntu)](/mariadb-administration/getting-installing-and-upgrading-mariadb/troubleshooting-installation-issues/installation-issues-on-debian-and-ubuntu/differences-in-mariadb-in-debian-and-ubuntu) for more information.

- The [named_pipe](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-named-pipe) authentication plugin is installed by default in all builds on Windows.

### Client Authentication Plugins Installed by Default

Client-side authentication plugins do not need to be <em>installed</em> in the same way that server-side authentication plugins do. If the client uses either the `libmysqlclient` or [MariaDB Connector/C](/kb/en/mariadb-connector-c/) library, then the library automatically loads client-side authentication plugins from the library's plugin directory whenever they are needed.

Most [clients and utilities](/clients-utilities) support the `--plugin-dir` command line argument that can be used to set the path to the library's plugin directory:

<table><tbody><tr><th>Client Option</th><th>Description</th></tr>
<tr><td><code>--plugin-dir=path</code></td><td>Directory for client-side plugins.</td></tr>
</tbody></table>

Developers who are using [MariaDB Connector/C](/kb/en/mariadb-connector-c/) can implement similar functionality in their application by setting the `MYSQL_PLUGIN_DIR` option with the <a undefined>mysql_optionsv</a> function.

For example:

```sql
mysql_optionsv(mysql, MYSQL_PLUGIN_DIR, "path");
```

If your client encounters errors similar to the following, then you may need to set the path to the library's plugin directory:

```sql
ERROR 2059 (HY000): Authentication plugin 'dialog' cannot be loaded: /usr/lib/mysql/plugin/dialog.so: cannot open shared object file: No such file or directory
```

If the client does <strong>not</strong> use either the `libmysqlclient` or [MariaDB Connector/C](/kb/en/mariadb-connector-c/) library, then you will have to determine which authentication plugins are supported by the specific client library used by the client.

If the client uses either the `libmysqlclient` or [MariaDB Connector/C](/kb/en/mariadb-connector-c/) library, but the client is not bundled with either library's <em>optional</em> client authentication plugins, then you can only use the conventional authentication plugins (like [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) and [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password)) and the non-conventional authentication plugins that don't require special client-side authentication plugins (like [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) and [named_pipe](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-named-pipe)).

## Default Authentication Plugin

### Default Server Authentication Plugin

The [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) authentication plugin is currently the default authentication plugin in all versions of MariaDB if the <a undefined>old_passwords</a> system variable is set to `0`, which is the default.

On a system with the <a undefined>old_passwords</a> system variable set to `0`, this means that if you create a user account with either the [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) or [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user) statements, and if you do not specify an authentication plugin with the `IDENTIFIED VIA` clause, then MariaDB will use the [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) authentication plugin for the user account.

For example, this user account will use the [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) authentication plugin:

```sql
CREATE USER username@hostname;
```

And so will this user account:

```sql
CREATE USER username@hostname IDENTIFIED BY 'notagoodpassword';
```

The [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password) authentication plugin becomes the default authentication plugin in all versions of MariaDB if the <a undefined>old_passwords</a> system variable is explicitly set to `1`.

However, the [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password) authentication plugin is not considered secure, so it is recommended to avoid using this authentication plugin. To help prevent undesired use of the [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password) authentication plugin, the server supports the <a undefined>secure_auth</a> system variable that can be used to configured the server to refuse connections that try to use the [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password) authentication plugin:

<table><tbody><tr><th>Server Option</th><th>Description</th></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#old_passwords">old_passwords={1 | 0}</a></code></td><td>If set to <code>1</code> (<code>0</code> is default), MariaDB reverts to using the <code><a href="/kb/en/authentication-plugin-mysql_old_password/">mysql_old_password</a></code> authentication plugin by default for newly created users and passwords, instead of the <code><a href="/kb/en/authentication-plugin-mysql_native_password/">mysql_native_password</a></code> authentication plugin.</td></tr>
<tr><td><code><a href="/kb/en/server-system-variables/#secure_auth">secure_auth</a></code></td><td>Connections will be blocked if they use the the <code><a href="/kb/en/authentication-plugin-mysql_old_password/">mysql_old_password</a></code> authentication plugin. The server will also fail to start if the privilege tables are in the old, pre-MySQL 4.1 format.</td></tr>
</tbody></table>

Most [clients and utilities](/clients-utilities) also support the `--secure-auth` command line argument that can also be used to configure the client to refuse to connect to servers that use the [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password) authentication plugin:

<table><tbody><tr><th>Client Option</th><th>Description</th></tr>
<tr><td><code>--secure-auth</code></td><td>Refuse to connect to the server if the server uses the <code><a href="/kb/en/authentication-plugin-mysql_old_password/">mysql_old_password</a></code> authentication plugin. This mode is off by default, which is a difference in behavior compared to MySQL 5.6 and later, where it is on by default.</td></tr>
</tbody></table>

Developers who are using [MariaDB Connector/C](/kb/en/mariadb-connector-c/) can implement similar functionality in their application by setting the `MYSQL_SECURE_AUTH` option with the <a undefined>mysql_optionsv</a> function.

For example:

```sql
mysql_optionsv(mysql, MYSQL_SECURE_AUTH, 1);
```

### Default Client Authentication Plugin

The default client-side authentication plugin depends on a few factors.

If a client doesn't explicitly set the default client-side authentication plugin, then the client will determine which authentication plugin to use by checking the length of the scramble in the server's handshake packet.

If the server's handshake packet contains a 9-byte scramble, then the client will default to the [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password) authentication plugin.

If the server's handshake packet contains a 20-byte scramble, then the client will default to the [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) authentication plugin.

#### Setting the Default Client Authentication Plugin

Most [clients and utilities](/clients-utilities) support the `--default-auth` command line argument that can be used to set the default client-side authentication plugin:

<table><tbody><tr><th>Client Option</th><th>Description</th></tr>
<tr><td><code>--default-auth=name</code></td><td>Default authentication client-side plugin to use.</td></tr>
</tbody></table>

Developers who are using [MariaDB Connector/C](/kb/en/mariadb-connector-c/) can implement similar functionality in their application by setting the `MYSQL_DEFAULT_AUTH` option with the <a undefined>mysql_optionsv</a> function.

For example:

```sql
mysql_optionsv(mysql, MYSQL_DEFAULT_AUTH, "name");
```

If you know that your user account is configured to require a client-side authentication plugin that isn't [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password) or [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password), then it can help speed up your connection process to explicitly set the default client-side authentication plugin.

According to the [client-server protocol](/kb/en/clientserver-protocol/), the server first sends the handshake packet to the client, then the client replies with a packet containing the user name of the user account that is requesting access. The server handshake packet initially tells the client to use the default server authentication plugin, and the client reply initially tells the server that it will use the default client authentication plugin.

However, the server-side and client-side authentication plugins mentioned in these initial packets may not be the correct ones for this specific user account. The server only knows what authentication plugin to use for this specific user account after reading the user name from the client reply packet and finding the appropriate row for the user account in either the <a undefined>mysql.user</a> table or the <a undefined>mysql.global_priv</a> table, depending on the MariaDB version.

If the server finds that either the server-side or client-side default authentication plugin does not match the actual authentication plugin that should be used for the given user account, then the server restarts the authentication on either the server side or the client side.

This means that, if you know what client authentication plugin your user account requires, then you can avoid an unnecessary authentication restart and you can save two packets and two round-trips.between the client and server by configuring your client to use the correct authentication plugin by default.

## Authentication Plugins

### Server Authentication Plugins

#### `mysql_native_password`

The [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) authentication plugin uses the password hashing algorithm introduced in MySQL 4.1, which is also used by the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password) function when <a undefined>old_passwords=0</a> is set. This hashing algorithm is based on [SHA-1](https://en.wikipedia.org/wiki/SHA-1).

#### `mysql_old_password`

The [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password) authentication plugin uses the pre-MySQL 4.1 password hashing algorithm, which is also used by the [OLD_PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/old_password) function and by the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password) function when <a undefined>old_passwords=1</a> is set.

#### `ed25519`

The [ed25519](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-ed25519) authentication plugin uses [Elliptic Curve Digital Signature Algorithm](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) to securely store users' passwords and to authenticate users. The [ed25519](https://en.wikipedia.org/wiki/EdDSA#Ed25519) algorithm is the same one that is [used by OpenSSH](https://www.openssh.com/txt/release-6.5). It is based on the elliptic curve and code created by [Daniel J. Bernstein](https://en.wikipedia.org/wiki/Daniel_J._Bernstein).

From a user's perspective, the [ed25519](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-ed25519) authentication plugin still provides conventional password-based authentication.

#### `gssapi`

The [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi) authentication plugin allows the user to authenticate with services that use the [Generic Security Services Application Program Interface (GSSAPI)](https://en.wikipedia.org/wiki/Generic_Security_Services_Application_Program_Interface). Windows has a slightly different but very similar API called [Security Support Provider Interface (SSPI)](https://docs.microsoft.com/en-us/windows/desktop/secauthn/sspi).

On Windows, this authentication plugin supports [Kerberos](https://docs.microsoft.com/en-us/windows/desktop/secauthn/microsoft-kerberos) and [NTLM](https://docs.microsoft.com/en-us/windows/desktop/secauthn/microsoft-ntlm) authentication. Windows authentication is supported regardless of whether a [domain](https://en.wikipedia.org/wiki/Windows_domain) is used in the environment.

On Unix systems, the most dominant GSSAPI service is [Kerberos](https://en.wikipedia.org/wiki/Kerberos_(protocol)). However, it is less commonly used on Unix systems than it is on Windows. Regardless, this authentication plugin also supports Kerberos authentication on Unix.

The [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi) authentication plugin is most often used for authenticating with [Microsoft Active Directory](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).

#### `pam`

The [pam](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam) authentication plugin allows MariaDB to offload user authentication to the system's [Pluggable Authentication Module (PAM)](http://en.wikipedia.org/wiki/Pluggable_authentication_module) framework. PAM is an authentication framework used by Linux, FreeBSD, Solaris, and other Unix-like operating systems.

#### `unix_socket`

The [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin allows the user to use operating system credentials when connecting to MariaDB via the local Unix socket file. This Unix socket file is defined by the <a undefined>socket</a> system variable.

The [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin works by calling the <a undefined>getsockopt</a> system call with the `SO_PEERCRED` socket option, which allows it to retrieve the `uid` of the process that is connected to the socket. It is then able to get the user name associated with that `uid`. Once it has the user name, it will authenticate the connecting user as the MariaDB account that has the same user name.

For example:

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

In this example, a user `serg` is already logged into the operating system and has full shell access. He has already authenticated with the operating system and his MariaDB account is configured to use the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin, so he does not need to authenticate again for the database. MariaDB accepts his operating system credentials and allows him to connect. However, any attempt to connect to the database as another operating system user will be denied.

#### `named_pipe`

The [named_pipe](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-named-pipe) authentication plugin allows the user to use operating system credentials when connecting to MariaDB via named pipe on Windows. Named pipe connections are enabled by the <a undefined>named_pipe</a> system variable.

The [named_pipe](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-named-pipe) authentication plugin works by using [named pipe impersonation](https://msdn.microsoft.com/en-us/library/windows/desktop/aa378618%28v=vs.85%29.aspx) and calling `GetUserName()` to retrieve the user name of the process that is connected to the named pipe. Once it has the user name, it authenticates the connecting user as the MariaDB account that has the same user name.

For example:

```sql
CREATE USER wlad IDENTIFIED VIA named_pipe;
CREATE USER monty IDENTIFIED VIA named_pipe;
quit

C:\>echo %USERNAME%
wlad

C:\> mysql --user=wlad --protocol=PIPE
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 4
Server version: 10.1.12-MariaDB-debug Source distribution

Copyright (c) 2000, 2015, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> quit
Bye

C:\> mysql --user=monty  --protocol=PIPE
ERROR 1698 (28000): Access denied for user 'monty'@'localhost'
```

## Authentication Plugin API

The authentication plugin API is extensively documented in the [source code](/kb/en/getting-the-mariadb-source-code/) in the following files:

- `mysql/plugin_auth.h` (server part)
- `mysql/client_plugin.h` (client part)
- `mysql/plugin_auth_common.h` (common parts)

The MariaDB [source code](/kb/en/getting-the-mariadb-source-code/) also contains some authentication plugins that are intended explicitly to be examples for developers. They are located in `plugin/auth_examples`.

The definitions of two example authentication plugins called `two_questions` and `three_attempts` can be seen in `plugin/auth_examples/dialog_examples.c`. These authentication plugins demonstrate how to communicate with the user using the <a undefined>dialog</a> client authentication plugin.

The `two_questions` authentication plugin asks the user for a password and a confirmation ("Are you sure?").

The `three_attempts` authentication plugin gives the user three attempts to enter a correct password.

The password for both of these plugins should be specified in the plain text in the `USING` clause:

```sql
CREATE USER insecure IDENTIFIED VIA two_questions USING 'notverysecret';
```

### Dialog Client Authentication Plugin - Client Library Extension

The <a undefined>dialog</a> client authentication plugin, strictly speaking, is not part of the client-server or authentication plugin API. But it can be loaded into any client application that uses the `libmysqlclient` or [MariaDB Connector/C](/kb/en/mariadb-connector-c/) libraries. This authentication plugin provides a way for the application to customize the UI of the dialog function.

In order to use the <a undefined>dialog</a> client authentication plugin to communicate with the user in a customized way, the application will need to implement a function with the following signature:

```sql
extern "C" char *mysql_authentication_dialog_ask(
  MYSQL *mysql, int type, const char *prompt, char *buf, int buf_len)
```

The function takes the following arguments:

- The connection handle.
- A question "type", which has one of the following values:
<ul start="1"><li>`1` - Normal question
</li><li>`2` - Password (no echo)
</li></ul>
- A prompt.
- A buffer.
- The length of the buffer.

The function returns a pointer to a string of characters, as entered by the user. It may be stored in `buf` or allocated with `malloc()`.

Using this function a GUI application can pop up a dialog window, a network application can send the question over the network, as required. If no `mysql_authentication_dialog_ask` function is provided by the application, the <a undefined>dialog</a> client authentication plugin falls back to <a undefined>fputs()</a> and <a undefined>fgets()</a>.

Providing this callback is particularly important on Windows, because Windows GUI applications have no associated console and the default dialog function will not be able to reach the user. An example of Windows GUI client that does it correctly is [HeidiSQL](/clients-utilities/graphical-and-enhanced-clients/heidisql).

## See Also

- [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant)
- [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user)
- [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user)
- [Authentication from MariaDB 10.4](/mariadb-administration/user-server-security/user-account-management/authentication-from-mariadb-104)
- [Who are you? The history of MySQL and MariaDB authentication protocols from 1997 to 2017](https://mariadb.org/history-of-mysql-mariadb-authentication-protocols/)
- [MySQL 5.6 Reference Manual: Pluggable Authentication](https://dev.mysql.com/doc/refman/5.6/en/pluggable-authentication.html)
- [MySQL 5.6 Reference Manual: Writing Authentication Plugins](http://dev.mysql.com/doc/refman/5.6/en/writing-authentication-plugins.html)