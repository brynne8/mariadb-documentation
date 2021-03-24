# Authentication Plugin - ed25519

##### MariaDB starting with [10.1.22](/kb/en/mariadb-10122-release-notes/)

The `ed25519` authentication plugin was first released in [MariaDB 10.1.22](/kb/en/mariadb-10122-release-notes/) and [MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/).

MySQL has used SHA-1 based authentication since version 4.1. Since [MariaDB 5.2](/kb/en/what-is-mariadb-52/) this authentication plugin has been called [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password/). Over the years as computers became faster, new attacks on SHA-1 were being developed. Nowadays SHA-1 is no longer considered as secure as it was in 2001. That's why the `ed25519` authentication plugin was created.

The `ed25519` authentication plugin uses [Elliptic Curve Digital Signature Algorithm (ECDSA)](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) to securely store users' passwords and to authenticate users. The [ed25519](https://en.wikipedia.org/wiki/EdDSA#Ed25519) algorithm is the same one that is [used by OpenSSH](https://www.openssh.com/txt/release-6.5). It is based on the elliptic curve and code created by [Daniel J. Bernstein](https://en.wikipedia.org/wiki/Daniel_J._Bernstein).

From a user's perspective, the `ed25519` authentication plugin still provides conventional password-based authentication.

## Installing the Plugin

Although the plugin's shared library is distributed with MariaDB by default as `auth_ed25519.so` or `auth_ed25519.dll` depending on the operating system, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). For example:

```sql
INSTALL SONAME 'auth_ed25519';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = auth_ed25519
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'auth_ed25519';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Creating Users

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, you can create a user account by executing the [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/) statement and providing the <a undefined>IDENTIFIED VIA</a> clause followied by the the name of the plugin, which is `ed25519`, and providing the the `USING` clause followed by the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/) function with the plain-text password as an argument. For example:

```sql
CREATE USER username@hostname IDENTIFIED VIA ed25519 USING PASSWORD('secret');
```

If [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) does not have `NO_AUTO_CREATE_USER` set, then you can also create the user account via [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/). For example:

```sql
GRANT SELECT ON db.* TO username@hostname IDENTIFIED VIA ed25519 USING PASSWORD('secret');
```

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/) function and [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password/) statement did not work with the `ed25519` authentication plugin. Instead, you would have to use the [UDF](/programming-customizing-mariadb/user-defined-functions/) that comes with the authentication plugin to calculate the password hash. For example:

```sql
CREATE FUNCTION ed25519_password RETURNS STRING SONAME "auth_ed25519.so";
```

Now you can calculate a password hash by executing:

```sql
SELECT ed25519_password("secret");
+---------------------------------------------+
| SELECT ed25519_password("secret");          |
+---------------------------------------------+
| ZIgUREUg5PVgQ6LskhXmO+eZLS0nC8be6HPjYWR4YJY |
+---------------------------------------------+
```

Now you can use it to create the user account using the new password hash.

To create a user account via [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/), specify the name of the plugin in the <a undefined>IDENTIFIED VIA</a> clause while providing the password hash as the `USING` clause. For example:

```sql
CREATE USER username@hostname IDENTIFIED VIA ed25519 USING 'ZIgUREUg5PVgQ6LskhXmO+eZLS0nC8be6HPjYWR4YJY';
```

If [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) does not have `NO_AUTO_CREATE_USER` set, then you can also create the user account via [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/). For example:

```sql
GRANT SELECT ON db.* TO username@hostname IDENTIFIED VIA ed25519 USING 'ZIgUREUg5PVgQ6LskhXmO+eZLS0nC8be6HPjYWR4YJY';
```

Note that users require a password in order to be able to connect. It is possible to create a user without specifying a password, but they will be unable to connect.

## Changing User Passwords

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, you can change a user account's password by executing the [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password/) statement followed by the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/) function and providing the plain-text password as an argument. For example:

```sql
SET PASSWORD =  PASSWORD('new_secret')
```

You can also change the user account's password with the [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user/) statement. You would have to specify the name of the plugin in the <a undefined>IDENTIFIED VIA</a> clause while providing the plain-text password as an argument to the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/) function in the `USING` clause. For example:

```sql
ALTER USER username@hostname IDENTIFIED VIA ed25519 USING PASSWORD('new_secret');
```

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/) function and [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password/) statement did not work with the `ed25519` authentication plugin. Instead, you would have to use the [UDF](/programming-customizing-mariadb/user-defined-functions/) that comes with the authentication plugin to calculate the password hash. For example:

```sql
CREATE FUNCTION ed25519_password RETURNS STRING SONAME "auth_ed25519.so";
```

Now you can calculate a password hash by executing:

```sql
SELECT ed25519_password("secret");
+---------------------------------------------+
| SELECT ed25519_password("secret");          |
+---------------------------------------------+
| ZIgUREUg5PVgQ6LskhXmO+eZLS0nC8be6HPjYWR4YJY |
+---------------------------------------------+
```

Now you can change the user account's password using the new password hash.

You can change the user account's password with the [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user/) statement. You would have to specify the name of the plugin in the <a undefined>IDENTIFIED VIA</a> clause while providing the password hash as the `USING` clause. For example:

```sql
ALTER USER username@hostname IDENTIFIED VIA ed25519 USING 'ZIgUREUg5PVgQ6LskhXmO+eZLS0nC8be6HPjYWR4YJY';
```

## Client Authentication Plugins

For clients that use the `libmysqlclient` or [MariaDB Connector/C](/kb/en/mariadb-connector-c/) libraries, MariaDB provides one client authentication plugin that is compatible with the `ed25519` authentication plugin:

- `client_ed25519`

When connecting with a [client or utility](/clients-utilities/) to a server as a user account that authenticates with the `ed25519` authentication plugin, you may need to tell the client where to find the relevant client authentication plugin by specifying the `--plugin-dir` option. For example:

```sql
mysql --plugin-dir=/usr/local/mysql/lib64/mysql/plugin --user=alice
```

### `client_ed25519`

The `client_ed25519` client authentication plugin hashes and signs the password using the [Elliptic Curve Digital Signature Algorithm (ECDSA)](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) before sending it to the server.

## Support in Client Libraries

### Using the Plugin with MariaDB Connector/C

[MariaDB Connector/C](/kb/en/mariadb-connector-c/) supports `ed25519` authentication using the [client authentication plugins](client-authentication-plugins) mentioned in the previous section since MariaDB Connector/C 3.1.0.

### Using the Plugin with MariaDB Connector/ODBC

[MariaDB Connector/ODBC](/kb/en/mariadb-connector-odbc/) supports `ed25519` authentication using the [client authentication plugins](client-authentication-plugins) mentioned in the previous section since MariaDB Connector/ODBC 3.1.2.

### Using the Plugin with MariaDB Connector/J

[MariaDB Connector/J](/kb/en/mariadb-connector-j/) supports `ed25519` authentication since MariaDB Connector/J 2.2.1.

### Using the Plugin with MariaDB Connector/Node.js

[MariaDB Connector/Node.js](/kb/en/nodejs-connector/) supports `ed25519` authentication since MariaDB Connector/Node.js 2.1.0.

### Using the Plugin with MySqlConnector for .NET

[MySqlConnector for ADO.NET](/kb/en/mysqlconnector-for-adonet/) supports `ed25519` authentication since MySqlConnector 0.56.0.

The connector implemented support for this authentication plugin in a separate [NuGet](https://docs.microsoft.com/en-us/nuget/what-is-nuget) package called <a undefined>MySqlConnector.Authentication.Ed25519</a>. After the package is installed, your application must call `Ed25519AuthenticationPlugin.Install` to enable it.

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.1</td><td>Stable</td><td><a href="/kb/en/mariadb-1040-release-notes/">MariaDB 10.4.0</a></td></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-1038-release-notes/">MariaDB 10.3.8</a>, <a href="/kb/en/mariadb-10217-release-notes/">MariaDB 10.2.17</a>, <a href="/kb/en/mariadb-10135-release-notes/">MariaDB 10.1.35</a></td></tr>
<tr><td>1.0</td><td>Beta</td><td><a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a>, <a href="/kb/en/mariadb-10122-release-notes/">MariaDB 10.1.22</a></td></tr>
</tbody></table>

## Options

### `ed25519`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the [mysql.plugins](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlplugin-table/) table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--ed25519=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---