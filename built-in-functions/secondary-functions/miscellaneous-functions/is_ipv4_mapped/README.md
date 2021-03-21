# IS_IPV4_MAPPED

##### MariaDB starting with [10.0.12](/kb/en/mariadb-10012-release-notes/)

IS_IPV4_MAPPED() has been available since [MariaDB 10.0.12](/kb/en/mariadb-10012-release-notes/).

## Syntax

```sql
IS_IPV4_MAPPED(expr)
```

## Description

Returns 1 if a given a numeric binary string IPv6 address, such as returned by [INET6_ATON()](/built-in-functions/secondary-functions/miscellaneous-functions/inet6_aton), is a valid IPv4-mapped address, otherwise returns 0.

##### MariaDB starting with [10.5.0](/kb/en/mariadb-1050-release-notes/)

From [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/), when the argument is not [INET6](/columns-storage-engines-and-plugins/data-types/string-data-types/inet6), automatic implicit [CAST](/built-in-functions/string-functions/cast) to INET6 is applied. As a consequence, `IS_IPV4_MAPPED` now understands arguments in both text representation and binary(16) representation. Before [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/), the function understood only binary(16) representation.

## Examples

```sql
SELECT IS_IPV4_MAPPED(INET6_ATON('::10.0.1.1'));
+------------------------------------------+
| IS_IPV4_MAPPED(INET6_ATON('::10.0.1.1')) |
+------------------------------------------+
|                                        0 |
+------------------------------------------+

SELECT IS_IPV4_MAPPED(INET6_ATON('::ffff:10.0.1.1'));
+-----------------------------------------------+
| IS_IPV4_MAPPED(INET6_ATON('::ffff:10.0.1.1')) |
+-----------------------------------------------+
|                                             1 |
+-----------------------------------------------+
```