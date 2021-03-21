# UUID

## Syntax

```sql
UUID()
```

## Description

Returns a Universal Unique Identifier (UUID) generated according to "DCE 1.1:
Remote Procedure Call" (Appendix A) CAE (Common Applications Environment)
Specifications published by The Open Group in October
1997 
([Document Number C706](http://www.opengroup.org/public/pubs/catalog/c706.htm)).

A UUID is designed as a number that is globally unique in space and time. Two
calls to <code class="highlight fixed" style="white-space:pre-wrap">UUID()</code> are expected to generate two different
values, even if these calls are performed on two separate computers that are
not connected to each other.

A UUID is a 128-bit number represented by a utf8 string of five
hexadecimal numbers in <code class="highlight fixed" style="white-space:pre-wrap">aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee</code>
format:

- The first three numbers are generated from a timestamp.
- The fourth number preserves temporal uniqueness in case the timestamp value
  loses monotonicity (for example, due to daylight saving time).
- The fifth number is an IEEE 802 node number that provides spatial uniqueness.
  A random number is substituted if the latter is not available (for example,
  because the host computer has no Ethernet card, or we do not know how to find
  the hardware address of an interface on your operating system). In this case,
  spatial uniqueness cannot be guaranteed. Nevertheless, a collision should
  have very low probability.

Currently, the MAC address of an interface is taken into account only on FreeBSD and Linux. On other operating systems, MariaDB uses a randomly generated 48-bit number.

Statements using the UUID() function are not [safe for replication](/kb/en/unsafe-statements-for-replication/).

UUID() results are intended to be unique, but cannot always be relied upon to unpredictable and unguessable, so should not be relied upon for these purposes.

## Examples

```sql
SELECT UUID();
+--------------------------------------+
| UUID()                               |
+--------------------------------------+
| cd41294a-afb0-11df-bc9b-00241dd75637 |
+--------------------------------------+
```

## See Also

- [UUID_SHORT()](/built-in-functions/secondary-functions/miscellaneous-functions/uuid_short/) ; Return short (64 bit) Universal Unique Identifier