# USER

## Syntax

```sql
USER()
```

## Description

Returns the current MariaDB user name and host name, given when authenticating to MariaDB,  as a string in the utf8 [character set](/kb/en/data-types-character-sets-and-collations/).

Note that the value of USER() may differ from the value of [CURRENT_USER()](/built-in-functions/secondary-functions/information-functions/current_user/), which is the user used to authenticate the current client. 
[CURRENT_ROLE()](/built-in-functions/secondary-functions/information-functions/current_role/) returns the current active role.

`SYSTEM_USER()` and `SESSION_USER` are synonyms for `USER()`.

Statements using the `USER()` function or one of its synonyms are not [safe for statement level replication](/kb/en/unsafe-statements-for-replication/).

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

## See Also

- [CURRENT_USER()](/built-in-functions/secondary-functions/information-functions/current_user/)