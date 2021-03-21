# IS_IPV4

##### MariaDB starting with [10.0.12](/kb/en/mariadb-10012-release-notes/)

IS_IPV4() has been available since [MariaDB 10.0.12](/kb/en/mariadb-10012-release-notes/).

## Syntax

```sql
IS_IPV4(expr)
```

## Description

If the expression is a valid IPv4 address, returns 1, otherwise returns 0.

IS_IPV4() is stricter than [INET_ATON()](/built-in-functions/secondary-functions/miscellaneous-functions/inet_aton/), but as strict as [INET6_ATON()](/built-in-functions/secondary-functions/miscellaneous-functions/inet6_aton/), in determining the validity of an IPv4 address. This implies that if IS_IPV4 returns 1, the same expression will always return a non-NULL result when passed to INET_ATON(), but that the reverse may not apply.

## Examples

```sql
SELECT IS_IPV4('1110.0.1.1');
+-----------------------+
| IS_IPV4('1110.0.1.1') |
+-----------------------+
|                     0 |
+-----------------------+

SELECT IS_IPV4('48f3::d432:1431:ba23:846f');
+--------------------------------------+
| IS_IPV4('48f3::d432:1431:ba23:846f') |
+--------------------------------------+
|                                    0 |
+--------------------------------------+
```