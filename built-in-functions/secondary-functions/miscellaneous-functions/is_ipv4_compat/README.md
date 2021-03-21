# IS_IPV4_COMPAT

##### MariaDB starting with [10.0.12](/kb/en/mariadb-10012-release-notes/)

IS_IPV4_COMPAT() has been available since [MariaDB 10.0.12](/kb/en/mariadb-10012-release-notes/).

## Syntax

```sql
IS_IPV4_COMPAT(expr)
```

## Description

Returns 1 if a given numeric binary string IPv6 address, such as returned by [INET6_ATON()](/built-in-functions/secondary-functions/miscellaneous-functions/inet6_aton), is IPv4-compatible, otherwise returns 0.

##### MariaDB starting with [10.5.0](/kb/en/mariadb-1050-release-notes/)

From [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/), when the argument is not [INET6](/columns-storage-engines-and-plugins/data-types/string-data-types/inet6), automatic implicit [CAST](/built-in-functions/string-functions/cast) to INET6 is applied. As a consequence, `IS_IPV4_COMPAT` now understands arguments in both text representation and binary(16) representation. Before [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/), the function understood only binary(16) representation.

## Examples

```sql
SELECT IS_IPV4_COMPAT(INET6_ATON('::10.0.1.1'));
+------------------------------------------+
| IS_IPV4_COMPAT(INET6_ATON('::10.0.1.1')) |
+------------------------------------------+
|                                        1 |
+------------------------------------------+

SELECT IS_IPV4_COMPAT(INET6_ATON('::48f3::d432:1431:ba23:846f'));
+-----------------------------------------------------------+
| IS_IPV4_COMPAT(INET6_ATON('::48f3::d432:1431:ba23:846f')) |
+-----------------------------------------------------------+
|                                                         0 |
+-----------------------------------------------------------+
```