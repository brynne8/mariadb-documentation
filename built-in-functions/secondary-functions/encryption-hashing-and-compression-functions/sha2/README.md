# SHA2

##### MariaDB starting with [5.5](/kb/en/what-is-mariadb-55/)

SHA2() was introduced in [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

## Syntax

```sql
SHA2(str,hash_len)
```

## Description

Given a string <em>`str`</em>, calculates an SHA-2 checksum, which is considered more cryptographically secure than its [SHA-1](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/sha1/) equivalent. The SHA-2 family includes SHA-224, SHA-256, SHA-384, and SHA-512, and the <em>`hash_len`</em> must correspond to one of these, i.e. 224, 256, 384 or 512. 0 is equivalent to 256.

The return value is a nonbinary string in the connection [character set and collation](/kb/en/data-types-character-sets-and-collations/), determined by the values of the [character_set_connection](/kb/en/server-system-variables/#character_set_connection) and [collation_connection](/kb/en/server-system-variables/#collation_connection) system variables.

NULL is returned if the hash length is not valid, or the string `str` is NULL.

SHA2 will only work if MariaDB was has been configured with [TLS support](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview/).

## Examples

```sql
SELECT SHA2('Maria',224);
+----------------------------------------------------------+
| SHA2('Maria',224)                                        |
+----------------------------------------------------------+
| 6cc67add32286412efcab9d0e1675a43a5c2ef3cec8879f81516ff83 |
+----------------------------------------------------------+

SELECT SHA2('Maria',256);
+------------------------------------------------------------------+
| SHA2('Maria',256)                                                |
+------------------------------------------------------------------+
| 9ff18ebe7449349f358e3af0b57cf7a032c1c6b2272cb2656ff85eb112232f16 |
+------------------------------------------------------------------+

SELECT SHA2('Maria',0);
+------------------------------------------------------------------+
| SHA2('Maria',0)                                                  |
+------------------------------------------------------------------+
| 9ff18ebe7449349f358e3af0b57cf7a032c1c6b2272cb2656ff85eb112232f16 |
+------------------------------------------------------------------+
```