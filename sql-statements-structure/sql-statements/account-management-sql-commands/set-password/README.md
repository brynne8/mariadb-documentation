# SET PASSWORD

## Syntax

```sql
SET PASSWORD [FOR user] =
    {
        PASSWORD('some password')
      | OLD_PASSWORD('some password')
      | 'encrypted password'
    }
```

## Description

The `SET PASSWORD` statement assigns a password to an existing MariaDB user
account.

If the password is specified using the [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/) or [OLD_PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/old_password/)
function, the literal text of the password should be given. If the
password is specified without using either function, the password
should be the already-encrypted password value as returned by
[PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/).

[OLD_PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/old_password/) should only be used if your MariaDB/MySQL clients are very old (&lt; 4.0.0).

With no `FOR` clause, this statement sets the password for the current
user. Any client that has connected to the server using a non-anonymous
account can change the password for that account.

With a `FOR` clause, this statement sets the password for a specific
account on the current server host. Only clients that have the `UPDATE`
privilege for the `mysql` database can do this. The user value should be
given in <code class="fixed" style="white-space:pre-wrap">user_name@host_name</code> format, where `user_name` and `host_name` are
exactly as they are listed in the User and Host columns of the
<a undefined>mysql.user</a> table entry.

The argument to [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/) and the password given to MariaDB clients can be of arbitrary length.

## Authentication Plugin Support

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, `SET PASSWORD` (with or without `PASSWORD()`) works for accounts authenticated via any [authentication plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins/) that supports passwords stored in the <a undefined>mysql.global_priv</a> table.

The [ed25519](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-ed25519/), [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password/), and [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password/) authentication plugins store passwords in the <a undefined>mysql.global_priv</a> table.

If you run `SET PASSWORD` on an account that authenticates with one of these authentication plugins that stores passwords in the <a undefined>mysql.global_priv</a> table, then the `PASSWORD()` function is evaluated by the specific authentication plugin used by the account. The authentication plugin hashes the password with a method that is compatible with that specific authentication plugin.

The [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/), [named_pipe](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-named-pipe/), [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi/), and [pam](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam/) authentication plugins do <strong>not</strong> store passwords in the <a undefined>mysql.global_priv</a> table. These authentication plugins rely on other methods to authenticate the user.

If you attempt to run `SET PASSWORD` on an account that authenticates with one of these authentication plugins that doesn't store a password in the <a undefined>mysql.global_priv</a> table, then MariaDB Server will raise a warning like the following:

```sql
SET PASSWORD is ignored for users authenticating via unix_socket plugin
```

See [Authentication from MariaDB 10.4](/mariadb-administration/user-server-security/user-account-management/authentication-from-mariadb-104/) for an overview of authentication changes in [MariaDB 10.4](/kb/en/what-is-mariadb-104/).

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, `SET PASSWORD` (with or without `PASSWORD()`) only works for accounts authenticated via [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password/) or [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password/) authentication plugins

## Passwordless User Accounts

User accounts do not always require passwords to login.

The [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) ,  [named_pipe](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-named-pipe/) and [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi/) authentication plugins do not require a password to authenticate the user.

The [pam](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam/) authentication plugin may or may not require a password to authenticate the user, depending on the specific configuration.

The [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password/) and [mysql_old_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password/) authentication plugins require passwords for authentication, but the password can be blank. In that case, no password is required.

If you provide a password while attempting to log into the server as an account that doesn't require a password, then MariaDB server will simply ignore the password.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, a user account can be defined to use multiple authentication plugins in a specific order of preference. This specific scenario may be more noticeable in these versions, since an account could be associated with some authentication plugins that require a password, and some that do not.

## Example

For example, if you had an entry with User and
Host column values of '<code class="fixed" style="white-space:pre-wrap">bob</code>' and 
'<code class="fixed" style="white-space:pre-wrap">%.loc.gov</code>', you would write the
statement like this:

```sql
SET PASSWORD FOR 'bob'@'%.loc.gov' = PASSWORD('newpass');
```

If you want to delete a password for a user, you would do:

```sql
SET PASSWORD FOR 'bob'@localhost = PASSWORD("");
```

## See Also

- [Password Validation Plugins](/columns-storage-engines-and-plugins/plugins/password-validation-plugins/) - permits the setting of basic criteria for passwords
- [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user/)