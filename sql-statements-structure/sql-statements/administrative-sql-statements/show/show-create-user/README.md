# SHOW CREATE USER

##### MariaDB starting with [10.2.0](/kb/en/mariadb-1020-release-notes/)

`SHOW CREATE USER` was introduced in [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/)

## Syntax

```sql
SHOW CREATE USER user_name
```

## Description

Shows the [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user) statement that created the given
user. The statement requires the [SELECT](/kb/en/grant/#table-privileges) privilege for the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) database, except for the current user.

## Examples

```sql
CREATE USER foo4@test require cipher 'text' 
  issuer 'foo_issuer' subject 'foo_subject';

SHOW CREATE USER foo4@test\G
*************************** 1. row ***************************
CREATE USER 'foo4'@'test' 
  REQUIRE ISSUER 'foo_issuer' 
  SUBJECT 'foo_subject' 
  CIPHER 'text'
```

[User Password Expiry](/mariadb-administration/user-server-security/user-account-management/user-password-expiry):

```sql
CREATE USER 'monty'@'localhost' PASSWORD EXPIRE INTERVAL 120 DAY;

SHOW CREATE USER 'monty'@'localhost';
+------------------------------------------------------------------+
| CREATE USER for monty@localhost                                  |
+------------------------------------------------------------------+
| CREATE USER 'monty'@'localhost' PASSWORD EXPIRE INTERVAL 120 DAY |
+------------------------------------------------------------------+
```

## See Also

- [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user)
- [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user)
- [SHOW GRANTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-grants) shows the `GRANTS/PRIVILEGES` for a user.
- [SHOW PRIVILEGES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-privileges) shows the privileges supported by MariaDB.