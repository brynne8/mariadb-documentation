# INET6_NTOA

##### MariaDB starting with [10.0.12](/kb/en/mariadb-10012-release-notes/)

INET6_NTOA() has been available from [MariaDB 10.0.12](/kb/en/mariadb-10012-release-notes/).

## Syntax

```sql
INET6_NTOA(expr)
```

## Description

Given an IPv6 or IPv4 network address as a numeric binary string, returns the address as a nonbinary string in the connection character set.

The return string is lowercase, and is platform independent, since it does not use functions specific to the operating system. It has a maximum length of 39 characters.

Returns NULL if the argument is not understood.

## Examples

```sql
SELECT INET6_NTOA(UNHEX('0A000101'));
+-------------------------------+
| INET6_NTOA(UNHEX('0A000101')) |
+-------------------------------+
| 10.0.1.1                      |
+-------------------------------+

SELECT INET6_NTOA(UNHEX('48F3000000000000D4321431BA23846F'));
+-------------------------------------------------------+
| INET6_NTOA(UNHEX('48F3000000000000D4321431BA23846F')) |
+-------------------------------------------------------+
| 48f3::d432:1431:ba23:846f                             |
+-------------------------------------------------------+
```

## See Also

- [INET6_ATON()](/built-in-functions/secondary-functions/miscellaneous-functions/inet6_aton/)
- [INET_NTOA()](/built-in-functions/secondary-functions/miscellaneous-functions/inet_ntoa/)