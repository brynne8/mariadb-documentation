# RENAME USER

## Syntax

```sql
RENAME USER old_user TO new_user
    [, old_user TO new_user] ...
```

## Description

The RENAME USER statement renames existing MariaDB accounts. To use it,
you must have the global <a undefined>CREATE USER</a> privilege
or the <a undefined>UPDATE</a> privilege for the `mysql` database.
Each account is named using the same format as for the [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/)
statement; for example, `'jeffrey'@'localhost'`.
If you specify only the user name part of the account name, a host
name part of `'%'` is used.

If any of the old user accounts do not exist or any of the new user accounts already
exist, `ERROR 1396 (HY000)` results. If an error occurs, `RENAME USER`
will still rename the accounts that do not result in an error.

## Examples

```sql
CREATE USER 'donald', 'mickey';
RENAME USER 'donald' TO 'duck'@'localhost', 'mickey' TO 'mouse'@'localhost';
```