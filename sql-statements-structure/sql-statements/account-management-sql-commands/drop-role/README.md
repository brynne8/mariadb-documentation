# DROP ROLE

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

[Roles](/mariadb-administration/user-server-security/user-account-management/roles) were introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/).

## Syntax

```sql
DROP ROLE [IF EXISTS] role_name [,role_name ...]
```

## Description

The `DROP ROLE` statement removes one or more MariaDB [roles](/mariadb-administration/user-server-security/user-account-management/roles). To use this
statement, you must have the global [CREATE USER](/kb/en/grant/#create-user) privilege or
the [DELETE](/kb/en/grant/#table-privileges) privilege for the mysql database.

`DROP ROLE` does not disable roles for connections which selected them with [SET ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/set-role). If a role has previously been set as a [default role](/sql-statements-structure/sql-statements/account-management-sql-commands/set-default-role), `DROP ROLE` does not remove the record of the default role from the [mysql.user](/kb/en/mysqluser-table/) table. If the role is subsequently recreated and granted, it will again be the user's default. Use [SET DEFAULT ROLE NONE](/sql-statements-structure/sql-statements/account-management-sql-commands/set-default-role) to explicitly remove this.

If any of the specified user accounts do not exist, `ERROR 1396 (HY000)`
results. If an error occurs, `DROP ROLE` will still drop the roles that
do not result in an error. Only one error is produced for all roles which have not been dropped:

```sql
ERROR 1396 (HY000): Operation DROP ROLE failed for 'a','b','c'
```

Failed `CREATE` or `DROP` operations, for both users and roles, produce the same error code.

Before [MariaDB 10.1.13](/kb/en/mariadb-10113-release-notes/), the `DROP ROLE` statement was not permitted in [prepared statements](/sql-statements-structure/sql-statements/prepared-statements).

#### IF EXISTS

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

The `IF EXISTS` clause was added in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

If the `IF EXISTS` clause is used, MariaDB will return a warning instead of an error if the role does not exist.

## Examples

```sql
DROP ROLE journalist;
```

The same thing using the optional `IF EXISTS` clause:

```sql
DROP ROLE journalist;
ERROR 1396 (HY000): Operation DROP ROLE failed for 'journalist'

DROP ROLE IF EXISTS journalist;
Query OK, 0 rows affected, 1 warning (0.00 sec)

Note (Code 1975): Can't drop role 'journalist'; it doesn't exist
```

## See Also

- [Roles Overview](/kb/en/roles-overview/)
- [CREATE ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/create-role)