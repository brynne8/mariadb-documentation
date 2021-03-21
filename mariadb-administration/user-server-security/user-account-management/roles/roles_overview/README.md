# Roles Overview

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

Roles were introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/).

## Description

A role bundles a number of privileges together. It assists larger organizations where, typically, a number of users would have the same privileges, and, previously, the only way to change the privileges for a group of users was by changing each user's privileges individually.

Alternatively, multiple external users could have been assigned the same user, and there would have been no way to see which actual user was responsible for which action.

With roles, managing this is easy. For example, there could be a number of users assigned to a journalist role, with identical privileges. Changing the privileges for all the journalists is a matter of simply changing the role's privileges, while the individual user is still linked with any changes that take place.

Roles are created with the [CREATE ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/create-role) statement, and dropped with the [DROP ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/drop-role) statement. Roles are then assigned to a user with an extension to the [GRANT](/kb/en/grant/#roles) statement, while privileges are assigned to a role in the regular way with [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant). Similarly, the [REVOKE](/sql-statements-structure/sql-statements/account-management-sql-commands/revoke) statement can be used to both revoke a role from a user, or revoke a privilege from a role.

Once a user has connected, he can obtain all  privileges associated with a role by <strong>setting</strong> a role with the [SET ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/set-role) statement. The [CURRENT_ROLE](/built-in-functions/secondary-functions/information-functions/current_role) function returns the currently set role for the session, if any.

Only roles granted directly to a user can be set, roles granted to other roles cannot. Instead the privileges granted to a role, which is, in turn, granted to another role (grantee), will be immediately available to any user who sets this second grantee role.

Roles were implemented as a GSoC 2013 project by Vicentiu Ciorbaru.

##### MariaDB starting with [10.1.1](/kb/en/mariadb-1011-release-notes/)

The [SET DEFAULT ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/set-default-role) statement allows one to set a default role for a user. A default role is automatically enabled when a user connects (an implicit SET ROLE statement is executed immediately after a connection is established).

## System Tables

Information about roles and who they've been granted to can be found in the [Information Schema APPLICABLE_ROLES table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-applicable_roles-table) as well as the [mysql.ROLES_MAPPING table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlroles_mapping-table).

The [Information Schema ENABLED_ROLES table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-enabled_roles-table) shows the enabled roles for the current session.

## Examples

Creating a role and granting a privilege:

```sql
CREATE ROLE journalist;

GRANT SHOW DATABASES ON *.* TO journalist;

GRANT journalist to hulda;
```

Note, that hulda has no `SHOW DATABASES` privilege, even though she was granted the journalist role. She needs to <strong>set</strong> the role first:

```sql
SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
+--------------------+

SELECT CURRENT_ROLE;
+--------------+
| CURRENT_ROLE |
+--------------+
| NULL         |
+--------------+

SET ROLE journalist;

SELECT CURRENT_ROLE;
+--------------+
| CURRENT_ROLE |
+--------------+
| journalist   |
+--------------+

SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| ...                |
| information_schema |
| mysql              |
| performance_schema |
| test               |
| ...                |
+--------------------+

SET ROLE NONE;
```

Roles can be granted to roles:

```sql
CREATE ROLE writer;

GRANT SELECT ON data.* TO writer;

GRANT writer TO journalist;
```

But one does not need to set a role granted to a role. For example, hulda will automatically get all writer privileges when she sets the journalist role:

```sql
SELECT CURRENT_ROLE;
+--------------+
| CURRENT_ROLE |
+--------------+
| NULL         |
+--------------+

SHOW TABLES FROM data;
Empty set (0.01 sec)

SET ROLE journalist;

SELECT CURRENT_ROLE;
+--------------+
| CURRENT_ROLE |
+--------------+
| journalist   |
+--------------+

SHOW TABLES FROM data;
+------------------------------+
| Tables_in_data               |
+------------------------------+
| set1                         |
| ...                          |
+------------------------------+
```

## Roles and Views (and Stored Routines)

When a user sets a role, he, in a sense, has two identities with two associated sets of privileges.
But a view (or a stored routine) can have only one definer. So, when a view (or a stored routine) is created with the `SQL SECURITY DEFINER`, one can specify whether the definer should be `CURRENT_USER` (and the view will have none of the privileges of the user's role) or `CURRENT_ROLE` (in this case, the view will use role's privileges, but none of the user's privileges). As a result, sometimes one can create a view that is impossible to use.

```sql
CREATE ROLE r1;

GRANT ALL ON db1.* TO r1;

GRANT r1 TO foo@localhost;

GRANT ALL ON db.* TO foo@localhost;

SELECT CURRENT_USER
+---------------+
| current_user  |
+---------------+
| foo@localhost |
+---------------+

SET ROLE r1;

CREATE TABLE db1.t1 (i int);

CREATE VIEW db.v1 AS SELECT * FROM db1.t1;

SHOW CREATE VIEW db.v1;
+------+------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
| View | Create View                                                                                                                              | character_set_client | collation_connection |
+------+------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
| v1   | CREATE ALGORITHM=UNDEFINED DEFINER=`foo`@`localhost` SQL SECURITY DEFINER VIEW `db`.`v1` AS SELECT `db1`.`t1`.`i` AS `i` from `db1`.`t1` | utf8                 | utf8_general_ci      |
+------+------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+

CREATE DEFINER=CURRENT_ROLE VIEW db.v2 AS SELECT * FROM db1.t1;

SHOW CREATE VIEW db.b2;
+------+-----------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
| View | Create View                                                                                                                 | character_set_client | collation_connection |
+------+-----------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
| v2   | CREATE ALGORITHM=UNDEFINED DEFINER=`r1` SQL SECURITY DEFINER VIEW `db`.`v2` AS select `db1`.`t1`.`a` AS `a` from `db1`.`t1` | utf8                 | utf8_general_ci      |
+------+-----------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
```

## Other Resources

- [Roles Review](http://ocelot.ca/blog/blog/2014/01/12/roles-review/) by Peter Gulutzan