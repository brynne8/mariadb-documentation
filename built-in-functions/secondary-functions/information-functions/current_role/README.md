# CURRENT_ROLE

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

[Roles](/mariadb-administration/user-server-security/user-account-management/roles/) were introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/).

## Syntax

```sql
CURRENT_ROLE, CURRENT_ROLE()
```

## Description

Returns the current [role](/mariadb-administration/user-server-security/user-account-management/roles/) name. This determines your access privileges. The return value is a string in the
utf8 [character set](/kb/en/data-types-character-sets-and-collations/).

If there is no current role, NULL is returned.

The output of `SELECT CURRENT_ROLE` is equivalent to the contents of the [ENABLED_ROLES](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-enabled_roles-table/) Information Schema table.

[USER()](/built-in-functions/secondary-functions/information-functions/user/) returns the combination of user and host used to login. [CURRENT_USER()](/built-in-functions/secondary-functions/information-functions/current_user/) returns the account used to determine current connection's privileges.

## Examples

```sql
SELECT CURRENT_ROLE;
+--------------+
| CURRENT_ROLE |
+--------------+
| NULL         |
+--------------+

SET ROLE staff;

SELECT CURRENT_ROLE;
+--------------+
| CURRENT_ROLE |
+--------------+
| staff        |
+--------------+
```