# INET_ATON

## Syntax

```sql
INET_ATON(expr)
```

## Description

Given the dotted-quad representation of an IPv4 network address as a string,
returns an integer that represents the numeric value of the address.
Addresses may be 4- or 8-byte addresses.

Returns NULL if the argument is not understood.

## Examples

```sql
SELECT INET_ATON('192.168.1.1');
+--------------------------+
| INET_ATON('192.168.1.1') |
+--------------------------+
|               3232235777 |
+--------------------------+
```

This is calculated as follows: 192 x 256<sup>3</sup> + 168 x 256 <sup>2</sup> + 1 x 256 + 1

## See Also

- [INET6_ATON()](/built-in-functions/secondary-functions/miscellaneous-functions/inet6_aton/)
- [INET_NTOA()](/built-in-functions/secondary-functions/miscellaneous-functions/inet_ntoa/)