# OLD_PASSWORD

## Syntax

```sql
OLD_PASSWORD(str)
```

## Description

`OLD_PASSWORD()` was added to MySQL when the implementation of 
[PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/) was changed to improve security. `OLD_PASSWORD()` returns the
value of the old (pre-MySQL 4.1) implementation of PASSWORD() as a
string, and is intended to permit you to reset passwords for any
pre-4.1 clients that need to connect to a more recent MySQL server version, or any version of MariaDB,
without locking them out.

As of [MariaDB 5.5](/kb/en/what-is-mariadb-55/), the return value is a nonbinary string in the connection [character set and collation](/kb/en/data-types-character-sets-and-collations/), determined by the values of the [character_set_connection](/kb/en/server-system-variables/#character_set_connection) and [collation_connection](/kb/en/server-system-variables/#collation_connection) system variables. Before 5.5, the return value was a binary string.

The return value is 16 bytes in length, or NULL if the argument was NULL.

## See Also

- [PASSWORD()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/password/)
- [MySQL manual on password hashing](http://dev.mysql.com/doc/refman/5.1/en/password-hashing.html)