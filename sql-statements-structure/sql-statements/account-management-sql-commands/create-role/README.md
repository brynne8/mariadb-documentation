# CREATE ROLE

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

[Roles](/mariadb-administration/user-server-security/user-account-management/roles/) were introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/).

## Syntax

```sql
CREATE [OR REPLACE] ROLE [IF NOT EXISTS] role 
  [WITH ADMIN 
    {CURRENT_USER | CURRENT_ROLE | user | role}]
```

## Description

The `CREATE ROLE` statement creates one or more MariaDB [roles](/mariadb-administration/user-server-security/user-account-management/roles/). To
use it, you must have the global [CREATE USER](/kb/en/grant/#create-user)
privilege or the [INSERT](/kb/en/grant/#table-privileges) privilege for the mysql
database. For each account, `CREATE ROLE` creates a new row in the
[mysql.user](/kb/en/mysqluser-table/) table that has no privileges, and with the
corresponding `is_role` field set to `Y`. It also creates a record in the
[mysql.roles_mapping](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlroles_mapping-table/) table.

If any of the specified roles already exist, `ERROR 1396 (HY000)` results. If
an error occurs, `CREATE ROLE` will still create the roles that do not result
in an error. The maximum length for a role is 128 characters. Role names can be
quoted, as explained in the [Identifier names](/sql-statements-structure/sql-language-structure/identifier-names/) page. Only
one error is produced for all roles which have not been created:

```sql
ERROR 1396 (HY000): Operation CREATE ROLE failed for 'a','b','c'
```

Failed `CREATE` or `DROP` operations, for both users and roles, produce the
same error code.

`PUBLIC` and `NONE` are reserved, and cannot be used as role names. `NONE` is used to [unset a role](/sql-statements-structure/sql-statements/account-management-sql-commands/set-role/) and `PUBLIC` has a special use in other systems, such as Oracle, so is reserved for compatibility purposes.

Before [MariaDB 10.1.13](/kb/en/mariadb-10113-release-notes/), the `CREATE ROLE` statement was not permitted in [prepared statements](/sql-statements-structure/sql-statements/prepared-statements/).

For valid identifiers to use as role names, see [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names/).

#### WITH ADMIN

The optional `WITH ADMIN` clause determines whether the current user, the
current role or another user or role has use of the newly created role. If the
clause is omitted, `WITH ADMIN CURRENT_USER` is treated as the default, which
means that the current user will be able to [GRANT](/kb/en/grant/#roles) this role to
users.

#### OR REPLACE

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

The `OR REPLACE` clause was added in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

If the optional `OR REPLACE` clause is used, it acts as a shortcut for:

```sql
DROP ROLE IF EXISTS name;
CREATE ROLE name ...;
```

#### IF NOT EXISTS

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

The `IF NOT EXISTS` clause was added in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

When the `IF NOT EXISTS` clause is used, MariaDB will return a warning instead of an error if the specified role already exists. Cannot be used together with the `OR REPLACE` clause.

## Examples

```sql
CREATE ROLE journalist;

CREATE ROLE developer WITH ADMIN lorinda@localhost;
```

Granting the role to another user. Only user `lorinda@localhost` has permission to grant the `developer` role:

```sql
 SELECT USER();
+-------------------+
| USER()            |
+-------------------+
| henning@localhost |
+-------------------+
...
GRANT developer TO ian@localhost;
Access denied for user 'henning'@'localhost'

 SELECT USER();
+-------------------+
| USER()            |
+-------------------+
| lorinda@localhost |
+-------------------+

GRANT m_role TO ian@localhost;
```

The `OR REPLACE` and `IF NOT EXISTS` clauses. The `journalist` role already exists:

```sql
CREATE ROLE journalist;
ERROR 1396 (HY000): Operation CREATE ROLE failed for 'journalist'

CREATE OR REPLACE ROLE journalist;
Query OK, 0 rows affected (0.00 sec)

CREATE ROLE IF NOT EXISTS journalist;
Query OK, 0 rows affected, 1 warning (0.00 sec)
```

```sql
SHOW WARNINGS;
+-------+------+---------------------------------------------------+
| Level | Code | Message                                           |
+-------+------+---------------------------------------------------+
| Note  | 1975 | Can't create role 'journalist'; it already exists |
+-------+------+---------------------------------------------------+

```

## See Also

- [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names/)
- [Roles Overview](/kb/en/roles-overview/)
- [DROP ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/drop-role/)