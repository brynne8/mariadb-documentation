# CREATE USER

## Syntax

```sql
CREATE [OR REPLACE] USER [IF NOT EXISTS] 
 user_specification [,user_specification ...] 
  [REQUIRE {NONE | tls_option [[AND] tls_option ...] }]
  [WITH resource_option [resource_option ...] ]
  [lock_option] [password_option] 

user_specification:
  username [authentication_option]

authentication_option:
  IDENTIFIED BY 'password' 
  | IDENTIFIED BY PASSWORD 'password_hash'
  | IDENTIFIED {VIA|WITH} authentication_rule [OR authentication_rule  ...]

authentication_rule:
    authentication_plugin
  | authentication_plugin {USING|AS} 'authentication_string'
  | authentication_plugin {USING|AS} PASSWORD('password')

tls_option:
  SSL 
  | X509
  | CIPHER 'cipher'
  | ISSUER 'issuer'
  | SUBJECT 'subject'

resource_option:
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

The `CREATE USER` statement creates new MariaDB accounts. To use it, you must have the global [CREATE USER](/kb/en/grant/#create-user) privilege or the [INSERT](/kb/en/grant/#table-privileges) privilege for the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) database. For each account, `CREATE USER` creates a new row in
the [mysql.user](/kb/en/mysqluser-table/) table that has no privileges.

If any of the specified accounts, or any permissions for the specified accounts, already exist, then the server returns `ERROR 1396 (HY000)`. If an error occurs, `CREATE USER` will still create the accounts that do not result in an error. Only one error is produced for all users which have not been created:

```sql
ERROR 1396 (HY000): 
  Operation CREATE USER failed for 'u1'@'%','u2'@'%'
```

`CREATE USER`, [DROP USER](/sql-statements-structure/sql-statements/account-management-sql-commands/drop-user), [CREATE ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/create-role), and [DROP ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/drop-role)  all produce the same error code when they fail.

See [Account Names](#account-names) below for details on how account names are specified.

## OR REPLACE

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

If the optional `OR REPLACE` clause is used, it is basically a shortcut for:

```sql
DROP USER IF EXISTS name;
CREATE USER name ...;
```

For example:

```sql
CREATE USER foo2@test IDENTIFIED BY 'password';
ERROR 1396 (HY000): Operation CREATE USER failed for 'foo2'@'test'

CREATE OR REPLACE USER foo2@test IDENTIFIED BY 'password';
Query OK, 0 rows affected (0.00 sec)
```

## IF NOT EXISTS

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

When the `IF NOT EXISTS` clause is used, MariaDB will return a warning instead of an error if the specified user already exists.

For example:

```sql
CREATE USER foo2@test IDENTIFIED BY 'password';
ERROR 1396 (HY000): Operation CREATE USER failed for 'foo2'@'test'

CREATE USER IF NOT EXISTS foo2@test IDENTIFIED BY 'password';
Query OK, 0 rows affected, 1 warning (0.00 sec)

SHOW WARNINGS;
+-------+------+----------------------------------------------------+
| Level | Code | Message                                            |
+-------+------+----------------------------------------------------+
| Note  | 1973 | Can't create user 'foo2'@'test'; it already exists |
+-------+------+----------------------------------------------------+
1 row in set (0.00 sec
```

## Authentication Options

### IDENTIFIED BY 'password'

The optional `IDENTIFIED BY` clause can be used to provide an account with a password. The password should be specified in plain text. It will be hashed by the [PASSWORD](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password) function prior to being stored to the <a undefined>mysql.user</a> table.

For example, if our password is `mariadb`, then we can create the user with:

```sql
CREATE USER foo2@test IDENTIFIED BY 'mariadb';
```

If you do not specify a password with the `IDENTIFIED BY` clause, the user
will be able to connect without a password. A blank password is not a wildcard
to match any password. The user must connect without providing a password if no
password is set.

The only [authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins) that this clause supports are [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) and [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password).

### IDENTIFIED BY PASSWORD 'password_hash'

The optional `IDENTIFIED BY PASSWORD` clause can be used to provide an account with a password that has already been hashed. The password should be specified as a hash that was provided by the [PASSWORD](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password) function. It will be stored to the <a undefined>mysql.user</a> table as-is.

For example, if our password is `mariadb`, then we can find the hash with:

```sql
SELECT PASSWORD('mariadb');
+-------------------------------------------+
| PASSWORD('mariadb')                       |
+-------------------------------------------+
| *54958E764CE10E50764C2EECBB71D01F08549980 |
+-------------------------------------------+
1 row in set (0.00 sec)
```

And then we can create a user with the hash:

```sql
CREATE USER foo2@test IDENTIFIED BY PASSWORD '*54958E764CE10E50764C2EECBB71D01F08549980';
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
CREATE USER foo2@test IDENTIFIED VIA pam;
```

Some authentication plugins allow additional arguments to be specified after a `USING` or `AS` keyword. For example, the [PAM authentication plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam) accepts a [service name](/kb/en/authentication-plugin-pam/#configuring-the-pam-service):

```sql
CREATE USER foo2@test IDENTIFIED VIA pam USING 'mariadb';
```

The exact meaning of the additional argument would depend on the specific authentication plugin.

##### MariaDB starting with [10.4.0](/kb/en/mariadb-1040-release-notes/)

The `USING` or `AS` keyword can also be used to provide a plain-text password to a plugin if it's provided as an argument to the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password) function. This is only valid for [authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins) that have implemented a hook for the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password) function. For example, the [ed25519](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-ed25519) authentication plugin supports this:

```sql
CREATE USER safe@'%' IDENTIFIED VIA ed25519 USING PASSWORD('secret');
```

##### MariaDB starting with [10.4.3](/kb/en/mariadb-1043-release-notes/)

One can specify many authentication plugins, they all works as alternatives ways of authenticating a user:

```sql
CREATE USER safe@'%' IDENTIFIED VIA ed25519 USING PASSWORD('secret') OR unix_socket;
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

For example, you can create a user account that requires these TLS options with the following:

```sql
CREATE USER 'alice'@'%'
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

Here is an example showing how to create a user with resource limits:

```sql
CREATE USER 'someone'@'localhost' WITH
    MAX_USER_CONNECTIONS 10
    MAX_QUERIES_PER_HOUR 200;
```

The resources are tracked per account, which means `'user'@'server'`; not per user name or per connection.

The count can be reset for all users using [FLUSH USER_RESOURCES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush), [FLUSH PRIVILEGES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) or [mysqladmin reload](/clients-utilities/mysqladmin).

Per account resource limits are stored in the <a undefined>user</a> table, in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) database. Columns used for resources limits are named `max_questions`, `max_updates`, `max_connections` (for `MAX_CONNECTIONS_PER_HOUR`), and `max_user_connections` (for `MAX_USER_CONNECTIONS`).

## Account Names

Account names have both a user name component and a host name component, and are specified as `'user_name'@'host_name'`.

The user name and host name may be unquoted, quoted as strings using double quotes (`"`) or
single quotes (`'`), or quoted as identifiers using backticks (```). You must use quotes
when using special characters (such as a hyphen) or wildcard characters. If you quote, you 
must quote the user name and host name separately (for example `'user_name'@'host_name'`).

### Host Name Component

If the host name is not provided, it is assumed to be `'%'`.

Host names may contain the wildcard characters `%` and `_`. They are matched as if by
the [LIKE](/built-in-functions/string-functions/like) clause. If you need to use a wildcard character literally (for example, to
match a domain name with an underscore), prefix the character with a backslash. See `LIKE`
for more information on escaping wildcard characters.

Host name matches are case-insensitive. Host names can match either domain names or IP
addresses. Use `'localhost'` as the host name to allow only local client connections.

You can use a netmask to match a range of IP addresses using `'base_ip/netmask'` as the
host name. A user with an IP address <em>ip_addr</em> will be allowed to connect if the following
condition is true:

```sql
ip_addr & netmask = base_ip
```

You can only use netmasks that specify a multiple of 8 bits of the address to match. That is,
only the following netmasks are allowed:

```sql
255.0.0.0
255.255.0.0
255.255.255.0
255.255.255.255
```

Using `255.255.255.255` is equivalent to not using a netmask at all.

Note that the credentials added when creating a user with the `'%'` wildcard host will not grant access in all cases. For example, some systems come with an anonymous localhost user, and when connecting from localhost this will take precedence.

### User Name Component

User names must match exactly, including case. A user name that is empty is known as an anonymous account and is allowed to match a login attempt with any user name component. These are described more in the next section.

For valid identifiers to use as user names, see [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names).

It is possible for more than one account to match when a user connects. MariaDB selects
the first matching account after sorting according to the following criteria:

- Accounts with an exact host name are sorted before accounts using a wildcard in the
host name. Host names using a netmask are considered to be exact for sorting.
- Accounts with a wildcard in the host name are sorted according to the position of
the first wildcard character. Those with a wildcard character later in the host name
sort before those with a wildcard character earlier in the host name.
- Accounts with a non-empty user name sort before accounts with an empty user name.
- Accounts with an empty user name are sorted last. As mentioned previously, these are known as anonymous accounts. These are described more in the next section.

The following table shows a list of example account as sorted by these criteria:

```sql
+---------+-------------+
| User    | Host        |
+---------+-------------+
| joffrey | 192.168.0.3 |
|         | 192.168.0.% |
| joffrey | 192.168.%   |
|         | 192.168.%   |
+---------+-------------+
```

Once connected, you only have the privileges granted to the account that matched,
not all accounts that could have matched. For example, consider the following
commands:

```sql
CREATE USER 'joffrey'@'192.168.0.3';
CREATE USER 'joffrey'@'%';
GRANT SELECT ON test.t1 to 'joffrey'@'192.168.0.3';
GRANT SELECT ON test.t2 to 'joffrey'@'%';
```

If you connect as joffrey from <code class="fixed" style="white-space:pre-wrap">192.168.0.3</code>, you will have the `SELECT`
privilege on the table `test.t1`, but not on the table `test.t2`. If you connect as joffrey from any other IP address, you will have the `SELECT` privilege on the table `test.t2`, but not
on the table `test.t1`.

##### MariaDB starting with [5.5.31](/kb/en/mariadb-5531-release-notes/)

Beginning with [MariaDB 5.5.31](/kb/en/mariadb-5531-release-notes/), usernames can be up to 80 characters long. From [MariaDB 10.0](/kb/en/what-is-mariadb-100/) the system tables are all by default this length. However, in order to enable this feature in [MariaDB 5.5](/kb/en/what-is-mariadb-55/), the following schema changes must be made:

```sql
ALTER TABLE mysql.user         MODIFY User         CHAR(80)  BINARY NOT NULL DEFAULT '';
ALTER TABLE mysql.db           MODIFY User         CHAR(80)  BINARY NOT NULL DEFAULT '';
ALTER TABLE mysql.tables_priv  MODIFY User         CHAR(80)  BINARY NOT NULL DEFAULT '';
ALTER TABLE mysql.columns_priv MODIFY User         CHAR(80)  BINARY NOT NULL DEFAULT '';
ALTER TABLE mysql.procs_priv   MODIFY User         CHAR(80)  BINARY NOT NULL DEFAULT '';
ALTER TABLE mysql.proc         MODIFY definer      CHAR(141) COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE mysql.event        MODIFY definer      CHAR(141) COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE mysql.proxies_priv MODIFY User         CHAR(80)  COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE mysql.proxies_priv MODIFY Proxied_user CHAR(80)  COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE mysql.proxies_priv MODIFY Grantor      CHAR(141) COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE mysql.servers      MODIFY Username     CHAR(80)                   NOT NULL DEFAULT '';
ALTER TABLE mysql.procs_priv   MODIFY Grantor      CHAR(141) COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE mysql.tables_priv  MODIFY Grantor      CHAR(141) COLLATE utf8_bin NOT NULL DEFAULT '';

FLUSH PRIVILEGES;
```

### Anonymous Accounts

Anonymous accounts are accounts where the user name portion of the account name is empty. These accounts act as special catch-all accounts. If a user attempts to log into the system from a host, and an anonymous account exists with a host name portion that matches the user's host, then the user will log in as the anonymous account if there is no more specific account match for the user name that the user entered.

For example, here are some anonymous accounts:

```sql
CREATE USER ''@'localhost';
CREATE USER ''@'192.168.0.3';
```

#### Fixing a Legacy Default Anonymous Account

On some systems, the <a undefined>mysql.db</a> table has some entries for the `''@'%'` anonymous account by default. Unfortunately, there is no matching entry in the <a undefined>mysql.user</a> table, which means that this anonymous account doesn't exactly exist, but it does have privileges--usually on the default `test` database created by [mysql_install_db](/clients-utilities/mysql_install_db). These account-less privileges are a legacy that is leftover from a time when MySQL's privilege system was less advanced.

This situation means that you will run into errors if you try to create a `''@'%'` account. For example:

```sql
CREATE USER ''@'%';
ERROR 1396 (HY000): Operation CREATE USER failed for ''@'%'
```

The fix is to [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) the row in the <a undefined>mysql.db</a> table and then execute [FLUSH PRIVILEGES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush):

```sql
DELETE FROM mysql.db WHERE User='' AND Host='%';
FLUSH PRIVILEGES;
```

And then the account can be created:

```sql
CREATE USER ''@'%';
Query OK, 0 rows affected (0.01 sec)
```

See [MDEV-13486](https://jira.mariadb.org/browse/MDEV-13486) for more information.

## Password Expiry

##### MariaDB starting with [10.4.3](/kb/en/mariadb-1043-release-notes/)

Besides automatic password expiry, as determined by [default_password_lifetime](/kb/en/server-system-variables/#default_password_lifetime), password expiry times can be set on an individual user basis, overriding the global setting, for example:

```sql
CREATE USER 'monty'@'localhost' PASSWORD EXPIRE INTERVAL 120 DAY;
```

See [User Password Expiry](/mariadb-administration/user-server-security/user-account-management/user-password-expiry) for more details.

## Account Locking

##### MariaDB starting with [10.4.2](/kb/en/mariadb-1042-release-notes/)

Account locking permits privileged administrators to lock/unlock user accounts. No new client connections will be permitted if an account is locked (existing connections are not affected). For example:

```sql
CREATE USER 'marijn'@'localhost' ACCOUNT LOCK;
```

See [Account Locking](/mariadb-administration/user-server-security/user-account-management/account-locking) for more details.

From [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/) and [MariaDB 10.5.8](/kb/en/mariadb-1058-release-notes/), the <em>lock_option</em> and <em>password_option</em> clauses can occur in either order.

## See Also

- [Troubleshooting Connection Issues](/kb/en/troubleshooting-connection-issues/)
- [Authentication from MariaDB 10.4](/mariadb-administration/user-server-security/user-account-management/authentication-from-mariadb-104)
- [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names)
- [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant)
- [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user)
- [DROP USER](/sql-statements-structure/sql-statements/account-management-sql-commands/drop-user)
- [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password)
- [SHOW CREATE USER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-user)
- [mysql.user table](/kb/en/mysqluser-table/)
- [Password Validation Plugins](/columns-storage-engines-and-plugins/plugins/password-validation-plugins) - permits the setting of basic criteria for passwords
- [Authentication Plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins) - allow various authentication methods to be used, and new ones to be developed.