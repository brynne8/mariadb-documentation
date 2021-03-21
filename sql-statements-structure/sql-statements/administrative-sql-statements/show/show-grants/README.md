# SHOW GRANTS

## Syntax

```sql
SHOW GRANTS [FOR user|role]
```

## Description

The `SHOW GRANTS` statement lists privileges granted to a particular user or role.

### Users

The statement lists the [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) statement or
statements that must be issued to duplicate the privileges that are granted to
a MariaDB user account. The account is named using the same format as for the
<code class="fixed" style="white-space:pre-wrap">GRANT</code> statement; for example,
'<code class="fixed" style="white-space:pre-wrap">jeffrey'@'localhost</code>'. If you specify only the user name part
of the account name, a host name part of '<code class="fixed" style="white-space:pre-wrap">%</code>' is used.  For
additional information about specifying account names, see
[GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant).

```sql
SHOW GRANTS FOR 'root'@'localhost';
+---------------------------------------------------------------------+
| Grants for root@localhost                                           |
+---------------------------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION |
+---------------------------------------------------------------------+
```

To list the privileges granted to the account that you are using to
connect to the server, you can use any of the following statements:

```sql
SHOW GRANTS;
SHOW GRANTS FOR CURRENT_USER;
SHOW GRANTS FOR CURRENT_USER();
```

If <code class="highlight fixed" style="white-space:pre-wrap">SHOW GRANTS FOR CURRENT_USER</code> (or any
of the equivalent syntaxes) is used in <code class="highlight fixed" style="white-space:pre-wrap">DEFINER</code> context (such
as within a stored procedure that is defined with 
 <code class="highlight fixed" style="white-space:pre-wrap">SQL SECURITY DEFINER</code>), the grants displayed are those of the
definer and not the invoker.

Note that the `DELETE HISTORY` privilege, introduced in [MariaDB 10.3.4](/kb/en/mariadb-1034-release-notes/), was displayed as `DELETE VERSIONING ROWS` when running `SHOW GRANTS` until [MariaDB 10.3.15](/kb/en/mariadb-10315-release-notes/) ([MDEV-17655](https://jira.mariadb.org/browse/MDEV-17655)).

### Roles

`SHOW GRANTS` can also be used to view the privileges granted to a [role](/mariadb-administration/user-server-security/user-account-management/roles).

#### Example

```sql
SHOW GRANTS FOR journalist;
+------------------------------------------+
| Grants for journalist                    |
+------------------------------------------+
| GRANT USAGE ON *.* TO 'journalist'       |
| GRANT DELETE ON `test`.* TO 'journalist' |
+------------------------------------------+
```

## See Also

- [Authentication from MariaDB 10.4](/mariadb-administration/user-server-security/user-account-management/authentication-from-mariadb-104)
- [SHOW CREATE USER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-user) shows how the user was created.
- [SHOW PRIVILEGES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-privileges) shows the privileges supported by MariaDB.
- [Roles](/mariadb-administration/user-server-security/user-account-management/roles)