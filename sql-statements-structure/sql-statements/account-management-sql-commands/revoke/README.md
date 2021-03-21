# REVOKE

## Privileges

### Syntax

```sql
REVOKE 
    priv_type [(column_list)]
      [, priv_type [(column_list)]] ...
    ON [object_type] priv_level
    FROM user [, user] ...

REVOKE ALL PRIVILEGES, GRANT OPTION
    FROM user [, user] ...
```

### Description

The `REVOKE` statement enables system administrators to revoke
privileges (or roles - see [section below](#roles)) from MariaDB accounts. Each account is named using the same format
as for the `GRANT` statement; for example,
'`jeffrey'@'localhost`'. If you specify only the user name part
of the account name, a host name part of '`<code>%`</code>' is used. For
details on the levels at which privileges exist, the allowable
`priv_type` and `priv_level` values, and the
syntax for specifying users and passwords, see [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/).

To use the first `REVOKE` syntax, you must have the
`GRANT OPTION` privilege, and you must have the privileges that
you are revoking.

To revoke all privileges, use the second syntax, which drops all
global, database, table, column, and routine privileges for the named
user or users:

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM user [, user] ...
```

To use this `REVOKE` syntax, you must have the global
[CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/) privilege or the
[UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) privilege for the mysql database. See
[GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/).

### Examples

```sql
REVOKE SUPER ON *.* FROM 'alexander'@'localhost';
```

## Roles

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

[Roles](/mariadb-administration/user-server-security/user-account-management/roles/) were introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/).

### Syntax

```sql
REVOKE role  [, role ...]
    FROM grantee [, grantee2 ... ]

REVOKE ADMIN OPTION FOR role FROM grantee [, grantee2]
```

### Description

`REVOKE` is also used to remove a [role](/mariadb-administration/user-server-security/user-account-management/roles/) from a user or another role that it's previously been assigned to. If a role has previously been set as a [default role](/sql-statements-structure/sql-statements/account-management-sql-commands/set-default-role/), `REVOKE` does not remove the record of the default role from the <a undefined>mysql.user</a> table. If the role is subsequently granted again, it will again be the user's default. Use [SET DEFAULT ROLE NONE](/sql-statements-structure/sql-statements/account-management-sql-commands/set-default-role/) to explicitly remove this.

Before [MariaDB 10.1.13](/kb/en/mariadb-10113-release-notes/), the `REVOKE role` statement was not permitted in [prepared statements](/sql-statements-structure/sql-statements/prepared-statements/).

### Example

```sql
REVOKE journalist FROM hulda
```