# INET6_ATON

##### MariaDB starting with [10.0.12](/kb/en/mariadb-10012-release-notes/)

INET6_ATON() has been available since [MariaDB 10.0.12](/kb/en/mariadb-10012-release-notes/).

## Syntax

```sql
INET6_ATON(expr)
```

## Description

Given an IPv6 or IPv4 network address as a string, returns a binary string that represents the numeric value of the address.

No trailing zone ID's or traling network masks are permitted. For IPv4 addresses, or IPv6 addresses with IPv4 address parts, no classful addresses or trailing port numbers are permitted and octal numbers are not supported.

The returned binary string will be [VARBINARY(16)](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary) or [VARBINARY(4)](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary) for IPv6 and IPv4 addresses respectively.

Returns NULL if the argument is not understood.

##### MariaDB starting with [10.5.0](/kb/en/mariadb-1050-release-notes/)

From [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/), `INET6_ATON` can take [INET6](/columns-storage-engines-and-plugins/data-types/string-data-types/inet6) as an argument.

## Examples

```sql
SELECT HEX(INET6_ATON('10.0.1.1'));
+-----------------------------+
| HEX(INET6_ATON('10.0.1.1')) |
+-----------------------------+
| 0A000101                    |
+-----------------------------+

SELECT HEX(INET6_ATON('48f3::d432:1431:ba23:846f'));
+----------------------------------------------+
| HEX(INET6_ATON('48f3::d432:1431:ba23:846f')) |
+----------------------------------------------+
| 48F3000000000000D4321431BA23846F             |
+----------------------------------------------+
```

## See Also

- [INET6_NTOA()](/built-in-functions/secondary-functions/miscellaneous-functions/inet6_ntoa)
- [INET_ATON()](/built-in-functions/secondary-functions/miscellaneous-functions/inet_aton)
- [INET6](/columns-storage-engines-and-plugins/data-types/string-data-types/inet6) Data Type