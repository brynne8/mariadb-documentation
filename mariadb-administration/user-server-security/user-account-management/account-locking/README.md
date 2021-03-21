# Account Locking

##### MariaDB starting with [10.4.2](/kb/en/mariadb-1042-release-notes/)

Account locking was introduced in [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/).

### Description

Account locking permits privileged administrators to lock/unlock user accounts. No new client connections will be permitted if an account is locked (existing connections are not affected).

User accounts can be locked at creation, with the [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/) statement, or modified after creation with the [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user/) statement. For example:

```sql
CREATE USER 'lorin'@'localhost' ACCOUNT LOCK;
```

or

```sql
ALTER USER 'marijn'@'localhost' ACCOUNT LOCK;
```

The server will return an `ER_ACCOUNT_HAS_BEEN_LOCKED` error when locked users attempt to connect:

```sql
mysql -ulorin
  ERROR 4151 (HY000): Access denied, this account is locked
```

The [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user/) statement is also used to unlock a user:

```sql
ALTER USER 'lorin'@'localhost' ACCOUNT UNLOCK;
```

The [SHOW CREATE USER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-user/) statement will show whether the account is locked:

```sql
SHOW CREATE USER 'marijn'@'localhost';
+-----------------------------------------------+
| CREATE USER for marijn@localhost              |
+-----------------------------------------------+
| CREATE USER 'marijn'@'localhost' ACCOUNT LOCK |
+-----------------------------------------------+
```

as well as querying the [mysql.global_priv table](/kb/en/mysqlglobal_priv-table/):

```sql
SELECT CONCAT(user, '@', host, ' => ', JSON_DETAILED(priv)) FROM mysql.global_priv 
  WHERE user='marijn';
+--------------------------------------------------------------------------------------+
| CONCAT(user, '@', host, ' => ', JSON_DETAILED(priv))                                 |
+--------------------------------------------------------------------------------------+
| marijn@localhost => {
    "access": 0,
    "plugin": "mysql_native_password",
    "authentication_string": "",
    "account_locked": true,
    "password_last_changed": 1558017158
} |
+--------------------------------------------------------------------------------------+
```

## See Also

- [Account Locking and Password Expiry](https://www.youtube.com/watch?v=AWM_fWZ3XIw) video tutorial