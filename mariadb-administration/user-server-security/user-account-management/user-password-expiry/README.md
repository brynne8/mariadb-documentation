# User Password Expiry

##### MariaDB starting with [10.4.3](/kb/en/mariadb-1043-release-notes/)

User password expiry was introduced in [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/).

Password expiry permits administrators to expire user passwords, either manually or automatically.

## System Variables

There are two system variables which affect password expiry: [default_password_lifetime](/kb/en/server-system-variables/#default_password_lifetime), which determines the amount of time between requiring the user to change their password. `0`, the default, means automatic password expiry is not active.

The second variable, [disconnect_on_expired_password](/kb/en/server-system-variables/#disconnect_on_expired_password) determines whether a client is permitted to connect if their password has expired, or whether they are permitted to connect in sandbox mode, able to perform a limited subset of queries related to resetting the password, in particular [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password/) and [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/).

## Setting a Password Expiry Limit for a User

Besides automatic password expiry, as determined by [default_password_lifetime](/kb/en/server-system-variables/#default_password_lifetime), password expiry times can be set on an individual user basis, overriding the global using the [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/) or [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user/) statements, for example:

```sql
CREATE USER 'monty'@'localhost' PASSWORD EXPIRE INTERVAL 120 DAY;
```

```sql
ALTER USER 'monty'@'localhost' PASSWORD EXPIRE INTERVAL 120 DAY;
```

Limits can be disabled by use of the `NEVER` keyword, for example:

```sql
CREATE USER 'monty'@'localhost' PASSWORD EXPIRE NEVER;
```

```sql
ALTER USER 'monty'@'localhost' PASSWORD EXPIRE NEVER;
```

A manually set limit can be restored the system default by use of `DEFAULT`, for example:

```sql
CREATE USER 'monty'@'localhost' PASSWORD EXPIRE DEFAULT;
```

```sql
ALTER USER 'monty'@'localhost' PASSWORD EXPIRE DEFAULT;
```

## SHOW CREATE USER

The [SHOW CREATE USER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-user/) statement will display information about the password expiry status of the user. Unlike MySQL, it will not display if the user is unlocked, or if the password expiry is set to default.

```sql
CREATE USER 'monty'@'localhost' PASSWORD EXPIRE INTERVAL 120 DAY;
CREATE USER 'konstantin'@'localhost' PASSWORD EXPIRE NEVER;
CREATE USER 'amse'@'localhost' PASSWORD EXPIRE DEFAULT;

SHOW CREATE USER 'monty'@'localhost';
+------------------------------------------------------------------+
| CREATE USER for monty@localhost                                  |
+------------------------------------------------------------------+
| CREATE USER 'monty'@'localhost' PASSWORD EXPIRE INTERVAL 120 DAY |
+------------------------------------------------------------------+

SHOW CREATE USER 'konstantin'@'localhost';
+------------------------------------------------------------+
| CREATE USER for konstantin@localhost                       |
+------------------------------------------------------------+
| CREATE USER 'konstantin'@'localhost' PASSWORD EXPIRE NEVER |
+------------------------------------------------------------+

SHOW CREATE USER 'amse'@'localhost';
+--------------------------------+
| CREATE USER for amse@localhost |
+--------------------------------+
| CREATE USER 'amse'@'localhost' |
+--------------------------------+
```

## Checking When Passwords Expire

The following query can be used to check when the current passwords expire for all users:

```sql
WITH password_expiration_info AS (
  SELECT User, Host,
  IF(
   IFNULL(JSON_EXTRACT(Priv, '$.password_lifetime'), -1) = -1,
   @@global.default_password_lifetime,
   JSON_EXTRACT(Priv, '$.password_lifetime')
  ) AS password_lifetime,
  JSON_EXTRACT(Priv, '$.password_last_changed') AS password_last_changed
  FROM mysql.global_priv
)
SELECT pei.User, pei.Host,
  pei.password_lifetime,
  FROM_UNIXTIME(pei.password_last_changed) AS password_last_changed_datetime,
  FROM_UNIXTIME(
   pei.password_last_changed +
   (pei.password_lifetime * 60 * 60 * 24)
  ) AS password_expiration_datetime
  FROM password_expiration_info pei
  WHERE pei.password_lifetime != 0
   AND pei.password_last_changed IS NOT NULL
UNION
SELECT pei.User, pei.Host,
  pei.password_lifetime,
  FROM_UNIXTIME(pei.password_last_changed) AS password_last_changed_datetime,
  0 AS password_expiration_datetime
  FROM password_expiration_info pei
  WHERE pei.password_lifetime = 0
   OR pei.password_last_changed IS NULL;
```

## --connect-expired-password Client Option

The [mysql client](/clients-utilities/mysql-client/mysql-command-line-client/) `--connect-expired-password` option notifies the server that the client is prepared to handle expired password sandbox mode (even if the `--batch` option was specified).

## See Also

- [Account Locking and Password Expiry](https://www.youtube.com/watch?v=AWM_fWZ3XIw) video tutorial