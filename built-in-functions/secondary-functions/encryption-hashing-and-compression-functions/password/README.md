# PASSWORD

## Syntax

```sql
PASSWORD(str)
```

## Description

The PASSWORD() function is used for hashing passwords for use in authentication by the MariaDB server. It is not intended for use in other applications.

Calculates and returns a hashed password string from the plaintext password <em>str</em>. Returns an empty string (&gt;= [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)) if the argument was NULL.

The return value is a nonbinary string in the connection [character set and collation](/kb/en/data-types-character-sets-and-collations/), determined by the values of the [character_set_connection](/kb/en/server-system-variables/#character_set_connection) and [collation_connection](/kb/en/server-system-variables/#collation_connection) system variables.

This is the function that is used for hashing MariaDB passwords for storage in the Password column of the [user table](/kb/en/mysqluser-table/) (see [privileges](/sql-statements-structure/sql-statements/account-management-sql-commands/grant)), usually used with the [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password) statement. It is not intended for use in other applications.

Until [MariaDB 10.3](/kb/en/what-is-mariadb-103/), the return value is 41-bytes in length, and the first character is always '*'. From [MariaDB 10.4](/kb/en/what-is-mariadb-104/), the function takes into account the authentication plugin where applicable (A [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user) or [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password) statement). For example, when used in conjunction with a user authenticated by the [ed25519 plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-ed25519), the statement will create a longer hash:

```sql
CREATE USER edtest@localhost IDENTIFIED VIA ed25519 USING PASSWORD('secret');

CREATE USER edtest2@localhost IDENTIFIED BY 'secret';

SELECT CONCAT(user, '@', host, ' => ', JSON_DETAILED(priv)) FROM mysql.global_priv
  WHERE user LIKE 'edtest%'\G
*************************** 1. row ***************************
CONCAT(user, '@', host, ' => ', JSON_DETAILED(priv)): edtest@localhost => {
...
    "plugin": "ed25519",
    "authentication_string": "ZIgUREUg5PVgQ6LskhXmO+eZLS0nC8be6HPjYWR4YJY",
...
}
*************************** 2. row ***************************
CONCAT(user, '@', host, ' => ', JSON_DETAILED(priv)): edtest2@localhost => {
...
    "plugin": "mysql_native_password",
    "authentication_string": "*14E65567ABDB5135D0CFD9A70B3032C179A49EE7",
...
}

```

The behavior of this function is affected by the value of the [old_passwords](/kb/en/server-system-variables/#old_passwords) system variable. If this is set to `1` (`0` is default), MariaDB reverts to using the [mysql_old_password authentication plugin](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_old_password) by default for newly created users and passwords.

## Examples

```sql
SELECT PASSWORD('notagoodpwd');
+-------------------------------------------+
| PASSWORD('notagoodpwd')                   |
+-------------------------------------------+
| *3A70EE9FC6594F88CE9E959CD51C5A1C002DC937 |
+-------------------------------------------+
```

```sql
SET PASSWORD FOR 'bob'@'%.loc.gov' = PASSWORD('newpass');
```

## See Also

- [Password Validation Plugins](/columns-storage-engines-and-plugins/plugins/password-validation-plugins) - permits the setting of basic criteria for passwords
- [OLD_PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/old_password) - pre-MySQL 4.1 password function