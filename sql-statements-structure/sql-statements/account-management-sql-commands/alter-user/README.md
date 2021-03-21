# ALTER USER

##### MariaDB starting with [10.2.0](/kb/en/mariadb-1020-release-notes/)

The ALTER USER statement was introduced in [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/).

## Syntax

```sql
ALTER USER [IF EXISTS] 
 user_specification [,user_specification] ...
  [REQUIRE {NONE | tls_option [[AND] tls_option] ...}]
  [WITH resource_option [resource_option] ...]
  [lock_option] [password_option] 

user_specification:
  username [authentication_option] [authentication_option] ... 

authentication_option:
  IDENTIFIED BY 'password' 
  | IDENTIFIED BY PASSWORD 'password_hash'
  | IDENTIFIED {VIA|WITH} authentication_plugin
  | IDENTIFIED {VIA|WITH} authentication_plugin {USING|AS} 'authentication_string'
  | IDENTIFIED {VIA|WITH} authentication_plugin {USING|AS} PASSWORD('password')

tls_option
  SSL 
  | X509
  | CIPHER 'cipher'
  | ISSUER 'issuer'
  | SUBJECT 'subject'

resource_option
  MAX_QUERIES_PER_HOUR count
  | MAX_UPDATES_PER_HOUR count
  | MAX_CONNECTIONS_PER_HOUR count
  | MAX_USER_CONNECTIONS count
  | MAX_STATEMENT_TIME time

password_option:
  PASSWORD EXPIRE
  | PASSWORD EXPIRE DEFAULT
  | PASSWORD EXPIRE NEVER
  | PASSWORD EXPIRE INTERVAL N DAY

lock_option:
    ACCOUNT LOCK
  | ACCOUNT UNLOCK
}
```

## Description

The `ALTER USER` statement modifies existing MariaDB accounts. To use it, you must have the global [CREATE USER](/kb/en/grant/#global-privileges) privilege or the [UPDATE](/kb/en/grant/#table-privileges) privilege for the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) database. The global [SUPER](/kb/en/grant/#global-privileges) privilege is also required if the [read_only](/kb/en/server-system-variables/#read_only) system variable is enabled.

If any of the specified user accounts do not yet exist, an error results. If an error occurs, `ALTER USER` will still modify the accounts that do not result in an error. Only one error is produced for all users which have not been modified.

## IF EXISTS

When the `IF EXISTS` clause is used, MariaDB will return a warning instead of an error for each specified user that does not exist.

## Account Names

For `ALTER USER` statements, account names are specified as the `username` argument in the same way as they are for [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user) statements. See [account names](/kb/en/create-user/#account-names) from the `CREATE USER` page for details on how account names are specified.

[CURRENT_USER](/built-in-functions/secondary-functions/information-functions/current_user) or `CURRENT_USER()` can also be used to alter the account logged into the current session. For example, to change the current user's password to `mariadb`:

```sql
ALTER USER CURRENT_USER() IDENTIFIED BY 'mariadb';
```

## Authentication Options

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

From [MariaDB 10.4](/kb/en/what-is-mariadb-104/), it is possible to use more than one authentication plugin for each user account. For example, this can be useful to slowly migrate users to the more secure ed25519 authentication plugin over time, while allowing the old mysql_native_password authentication plugin as an alternative for the transitional period. See [Authentication from MariaDB 10.4](/mariadb-administration/user-server-security/user-account-management/authentication-from-mariadb-104) for more.

When running `ALTER USER`, not specifying an authentication option will remove that authentication method. (However this was not the case before [MariaDB 10.4.13](/kb/en/mariadb-10413-release-notes/), see [MDEV-21928](https://jira.mariadb.org/browse/MDEV-21928))

For example, a user is created with the ability to authenticate via both a password and unix_socket:

```sql
CREATE USER 'bob'@'localhost' 
  IDENTIFIED VIA mysql_native_password USING PASSWORD('pwd') 
  OR unix_socket;

SHOW CREATE USER 'bob'@'localhost'\G
*************************** 1. row ***************************
CREATE USER for bob@localhost: CREATE USER `bob`@`localhost` 
  IDENTIFIED VIA mysql_native_password USING '*975B2CD4FF9AE554FE8AD33168FBFC326D2021DD' 
  OR unix_socket
```

If the user's password is updated, but unix_socket authentication is not specified, unix_socket authentication will no longer be permitted.

```sql
ALTER USER 'bob'@'localhost' IDENTIFIED VIA mysql_native_password USING PASSWORD('pwd2');

SHOW CREATE USER 'bob'@'localhost'\G
*************************** 1. row ***************************
CREATE USER for bob@localhost: CREATE USER `bob`@`localhost` 
  IDENTIFIED BY PASSWORD '*38366FDA01695B6A5A9DD4E428D9FB8F7EB75512'
```

### IDENTIFIED BY 'password'

The optional `IDENTIFIED BY` clause can be used to provide an account with a password. The password should be specified in plain text. It will be hashed by the [PASSWORD](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password) function prior to being stored to the [mysql.user](/kb/en/mysqluser-table/) table.

For example, if our password is `mariadb`, then we can set the account's password with:

```sql
ALTER USER foo2@test IDENTIFIED BY 'mariadb';
```

If you do not specify a password with the `IDENTIFIED BY` clause, the user
will be able to connect without a password. A blank password is not a wildcard
to match any password. The user must connect without providing a password if no
password is set.

The only [authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins) that this clause supports are [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) and [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password).

### IDENTIFIED BY PASSWORD 'password_hash'

The optional `IDENTIFIED BY PASSWORD` clause can be used to provide an account with a password that has already been hashed. The password should be specified as a hash that was provided by the [PASSWORD](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password)#function. It will be stored to the [mysql.user](/kb/en/mysqluser-table/) table as-is.

For example, if our password is `mariadb`, then we can find the hash with:

```sql
SELECT PASSWORD('mariadb');
+-------------------------------------------+
| PASSWORD('mariadb')                       |
+-------------------------------------------+
| *54958E764CE10E50764C2EECBB71D01F08549980 |
+-------------------------------------------+
```

And then we can set an account's password with the hash:

```sql
ALTER USER foo2@test IDENTIFIED BY PASSWORD '*54958E764CE10E50764C2EECBB71D01F08549980';
```

If you do not specify a password with the `IDENTIFIED BY` clause, the user
will be able to connect without a password. A blank password is not a wildcard
to match any password. The user must connect without providing a password if no
password is set.

The only [authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins) that this clause supports are [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) and [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password).

### IDENTIFIED {VIA|WITH} authentication_plugin

The optional `IDENTIFIED VIA authentication_plugin` allows you to specify that the account should be authenticated by a specific [authentication plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins). The plugin name must be an active authentication plugin as per [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins). If it doesn't show up in that output, then you will need to install it with [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin) or [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname).

For example, this could be used with the [PAM authentication plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam):

```sql
ALTER USER foo2@test IDENTIFIED VIA pam;
```

Some authentication plugins allow additional arguments to be specified after a `USING` or `AS` keyword. For example, the [PAM authentication plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam) accepts a [service name](/kb/en/authentication-plugin-pam/#configuring-the-pam-service):

```sql
ALTER USER foo2@test IDENTIFIED VIA pam USING 'mariadb';
```

The exact meaning of the additional argument would depend on the specific authentication plugin.

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, the `USING` or `AS` keyword can also be used to provide a plain-text password to a plugin if it's provided as an argument to the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password) function. This is only valid for [authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins) that have implemented a hook for the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password) function. For example, the [ed25519](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-ed25519) authentication plugin supports this:

```sql
ALTER USER safe@'%' IDENTIFIED VIA ed25519 USING PASSWORD('secret');
```

## TLS Options

By default, MariaDB transmits data between the server and clients without encrypting it. This is generally acceptable when the server and client run on the same host or in networks where security is guaranteed through other means. However, in cases where the server and client exist on separate networks or they are in a high-risk network, the lack of encryption does introduce security concerns as a malicious actor could potentially eavesdrop on the traffic as it is sent over the network between them.

To mitigate this concern, MariaDB allows you to encrypt data in transit between the server and clients using the Transport Layer Security (TLS) protocol. TLS was formerly known as Secure Socket Layer (SSL), but strictly speaking the SSL protocol is a predecessor to TLS and, that version of the protocol is now considered insecure. The documentation still uses the term SSL often and for compatibility reasons TLS-related server system and status variables still use the prefix ssl_, but internally, MariaDB only supports its secure successors.

See [Secure Connections Overview](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview) for more information about how to determine whether your MariaDB server has TLS support.

You can set certain TLS-related restrictions for specific user accounts. For instance, you might use this with user accounts that require access to sensitive data while sending it across networks that you do not control. These restrictions can be enabled for a user account with the [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user), [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user), or [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) statements. The following options are available:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>REQUIRE NONE</code></td><td>TLS is not required for this account, but can still be used.</td></tr>
<tr><td><code>REQUIRE SSL</code></td><td>The account must use TLS, but no valid X509 certificate is required. This option cannot be combined with other TLS options.</td></tr>
<tr><td><code>REQUIRE X509</code></td><td>The account must use TLS and must have a valid X509 certificate. This option implies <code>REQUIRE SSL</code>. This option cannot be combined with other TLS options.</td></tr>
<tr><td><code>REQUIRE ISSUER 'issuer'</code></td><td>The account must use TLS and must have a valid X509 certificate. Also, the Certificate Authority must be the one specified via the string <code>issuer</code>. This option implies <code>REQUIRE X509</code>. This option can be combined with the <code>SUBJECT</code>, and <code>CIPHER</code> options in any order.</td></tr>
<tr><td><code>REQUIRE SUBJECT 'subject'</code></td><td>The account must use TLS and must have a valid X509 certificate. Also, the certificate's Subject must be the one specified via the string <code>subject</code>. This option implies <code>REQUIRE X509</code>. This option can be combined with the <code>ISSUER</code>, and <code>CIPHER</code> options in any order.</td></tr>
<tr><td><code>REQUIRE CIPHER 'cipher'</code></td><td>The account must use TLS, but no valid X509 certificate is required. Also, the encryption used for the connection must use one of the methods specified in the string <code>cipher</code>. This option implies <code>REQUIRE SSL</code>. This option can be combined with the <code>ISSUER</code>, and <code>SUBJECT</code> options in any order.</td></tr>
</tbody></table>

The `REQUIRE` keyword must be used only once for all specified options, and the `AND` keyword can be used to separate individual options, but it is not required.

For example, you can alter a user account to require these TLS options with the following:

```sql
ALTER USER 'alice'@'%'
    REQUIRE SUBJECT '/CN=alice/O=My Dom, Inc./C=US/ST=Oregon/L=Portland'
    AND ISSUER '/C=FI/ST=Somewhere/L=City/ O=Some Company/CN=Peter Parker/emailAddress=p.parker@marvel.com'
    AND CIPHER 'TLSv1.2';
```

If any of these options are set for a specific user account, then any client who tries to connect with that user account will have to be configured to connect with TLS.

See [Securing Connections for Client and Server](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/securing-connections-for-client-and-server) for information on how to enable TLS on the client and server.

## Resource Limit Options

##### MariaDB starting with [10.2.0](/kb/en/mariadb-1020-release-notes/)

[MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/) introduced a number of resource limit options.

It is possible to set per-account limits for certain server resources. The following table shows the values that can be set per account:

<table><tbody><tr><th>Limit Type</th><th>Decription</th></tr>
<tr><td><code>MAX_QUERIES_PER_HOUR</code></td><td>Number of statements that the account can issue per hour (including updates)</td></tr>
<tr><td><code>MAX_UPDATES_PER_HOUR</code></td><td>Number of updates (not queries) that the account can issue per hour</td></tr>
<tr><td><code>MAX_CONNECTIONS_PER_HOUR</code></td><td>Number of connections that the account can start per hour</td></tr>
<tr><td><code>MAX_USER_CONNECTIONS</code></td><td>Number of simultaneous connections that can be accepted from the same account; if it is 0, <code>max_connections</code> will be used instead; if <code>max_connections</code> is 0, there is no limit for this account's simultaneous connections.</td></tr>
<tr><td><code>MAX_STATEMENT_TIME</code></td><td>Timeout, in seconds, for statements executed by the user. See also <a href="/kb/en/aborting-statements/">Aborting Statements that Exceed a Certain Time to Execute</a>.</td></tr>
</tbody></table>

If any of these limits are set to `0`, then there is no limit for that resource for that user.

Here is an example showing how to set an account's resource limits:

```sql
ALTER USER 'someone'@'localhost' WITH
    MAX_USER_CONNECTIONS 10
    MAX_QUERIES_PER_HOUR 200;
```

The resources are tracked per account, which means `'user'@'server'`; not per user name or per connection.

The count can be reset for all users using [FLUSH USER_RESOURCES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush), [FLUSH PRIVILEGES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) or [mysqladmin reload](/clients-utilities/mysqladmin).

Per account resource limits are stored in the [user](/kb/en/mysqluser-table/) table, in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) database. Columns used for resources limits are named `max_questions`, `max_updates`, `max_connections` (for `MAX_CONNECTIONS_PER_HOUR`), and `max_user_connections` (for `MAX_USER_CONNECTIONS`).

## Password Expiry

##### MariaDB starting with [10.4.3](/kb/en/mariadb-1043-release-notes/)

Besides automatic password expiry, as determined by [default_password_lifetime](/kb/en/server-system-variables/#default_password_lifetime), password expiry times can be set on an individual user basis, overriding the global setting, for example:

```sql
ALTER USER 'monty'@'localhost' PASSWORD EXPIRE INTERVAL 120 DAY;
ALTER USER 'monty'@'localhost' PASSWORD EXPIRE NEVER;
ALTER USER 'monty'@'localhost' PASSWORD EXPIRE DEFAULT;
```

See [User Password Expiry](/mariadb-administration/user-server-security/user-account-management/user-password-expiry) for more details.

## Account Locking

##### MariaDB starting with [10.4.2](/kb/en/mariadb-1042-release-notes/)

Account locking permits privileged administrators to lock/unlock user accounts. No new client connections will be permitted if an account is locked (existing connections are not affected). For example:

```sql
ALTER USER 'marijn'@'localhost' ACCOUNT LOCK;
```

See [Account Locking](/mariadb-administration/user-server-security/user-account-management/account-locking) for more details.

From [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/) and [MariaDB 10.5.8](/kb/en/mariadb-1058-release-notes/), the <em>lock_option</em> and <em>password_option</em> clauses can occur in either order.

## See Also

- [Authentication from MariaDB 10.4](/mariadb-administration/user-server-security/user-account-management/authentication-from-mariadb-104)
- [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant)
- [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user)
- [DROP USER](/sql-statements-structure/sql-statements/account-management-sql-commands/drop-user)
- [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password)
- [SHOW CREATE USER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-user)
- [mysql.user table](/kb/en/mysqluser-table/)
- [Password Validation Plugins](/columns-storage-engines-and-plugins/plugins/password-validation-plugins) - permits the setting of basic criteria for passwords
- [Authentication Plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins) - allow various authentication methods to be used, and new ones to be developed.