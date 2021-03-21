# IS_IPV6

##### MariaDB starting with [10.0.12](/kb/en/mariadb-10012-release-notes/)

IS_IPV6() has been available since [MariaDB 10.0.12](/kb/en/mariadb-10012-release-notes/).

## Syntax

```sql
IS_IPV6(expr)
```

## Description

Returns 1 if the expression is a valid IPv6 address specified as a string, otherwise returns 0. Does not consider IPv4 addresses to be valid IPv6 addresses.

## Examples

```sql
 SELECT IS_IPV6('48f3::d432:1431:ba23:846f');
+--------------------------------------+
| IS_IPV6('48f3::d432:1431:ba23:846f') |
+--------------------------------------+
|                                    1 |
+--------------------------------------+
1 row in set (0.02 sec)

SELECT IS_IPV6('10.0.1.1');
+---------------------+
| IS_IPV6('10.0.1.1') |
+---------------------+
|                   0 |
+---------------------+
```

## See Also

- [INET6](/columns-storage-engines-and-plugins/data-types/string-data-types/inet6/) data type
- [INET6_ATON](/built-in-functions/secondary-functions/miscellaneous-functions/inet6_aton/)
- [INET6_NTOA](/built-in-functions/secondary-functions/miscellaneous-functions/inet6_ntoa/)