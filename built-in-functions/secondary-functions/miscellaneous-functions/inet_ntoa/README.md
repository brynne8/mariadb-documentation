# INET_NTOA

## Syntax

```sql
INET_NTOA(expr)
```

## Description

Given a numeric IPv4 network address in network byte order (4 or 8 byte),
returns the dotted-quad representation of the address as a string.

## Examples

```sql
SELECT INET_NTOA(3232235777);
+-----------------------+
| INET_NTOA(3232235777) |
+-----------------------+
| 192.168.1.1           |
+-----------------------+
```

192.168.1.1 corresponds to 3232235777 since 192 x 256<sup>3</sup> + 168 x 256 <sup>2</sup> + 1 x 256 + 1 = 3232235777

## See Also

- [INET6_NTOA()](/built-in-functions/secondary-functions/miscellaneous-functions/inet6_ntoa/)
- [INET_ATON()](/built-in-functions/secondary-functions/miscellaneous-functions/inet_aton/)