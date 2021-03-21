# CURRENT_USER

## Syntax

```sql
CURRENT_USER, CURRENT_USER()
```

## Description

Returns the user name and host name combination for the MariaDB account
that the server used to authenticate the current client. This account
determines your access privileges. The return value is a string in the
utf8 [character set](/kb/en/data-types-character-sets-and-collations/).

The value of CURRENT_USER() can differ from the value of [USER()](/built-in-functions/secondary-functions/information-functions/user/). [CURRENT_ROLE()](/built-in-functions/secondary-functions/information-functions/current_role/) returns the current active role.

## Examples

```sql
shell> mysql --user="anonymous"

MariaDB [(none)]> select user(),current_user();
+---------------------+----------------+
| user()              | current_user() |
+---------------------+----------------+
| anonymous@localhost | @localhost     |
+---------------------+----------------+
```

When calling `CURRENT_USER()` in a stored procedure, it returns the owner of the stored procedure, as defined with `DEFINER`.

## See Also

- [USER()](/built-in-functions/secondary-functions/information-functions/user/)
- [CREATE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure/)