# BIT

## Syntax

```sql
BIT[(M)]
```

## Description

A bit-field type. `M` indicates the number of bits per value, from `1` to
`64`. The default is `1` if `M` is omitted.

Bit values can be inserted with `b'value'` notation, where `value` is the bit value in 0's and 1's.

Bit fields are automatically zero-padded from the left to the full length of the bit, so for example in a BIT(4) field, '10' is equivalent to '0010'.

Bits are returned as binary, so to display them, either add 0, or use a function such as [HEX](/built-in-functions/string-functions/hex), [OCT](/built-in-functions/numeric-functions/oct) or [BIN](/built-in-functions/string-functions/bin) to convert them.

## Examples

```sql
CREATE TABLE b ( b1 BIT(8) );
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) set, the default from [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/):

```sql
INSERT INTO b VALUES (b'11111111');

INSERT INTO b VALUES (b'01010101');

INSERT INTO b VALUES (b'1111111111111');
ERROR 1406 (22001): Data too long for column 'b1' at row 1

SELECT b1+0, HEX(b1), OCT(b1), BIN(b1) FROM b;
+------+---------+---------+----------+
| b1+0 | HEX(b1) | OCT(b1) | BIN(b1)  |
+------+---------+---------+----------+
|  255 | FF      | 377     | 11111111 |
|   85 | 55      | 125     | 1010101  |
+------+---------+---------+----------+
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) unset, the default until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/):

```sql
INSERT INTO b VALUES (b'11111111'),(b'01010101'),(b'1111111111111');
Query OK, 3 rows affected, 1 warning (0.10 sec)
Records: 3  Duplicates: 0  Warnings: 1

SHOW WARNINGS;
+---------+------+---------------------------------------------+
| Level   | Code | Message                                     |
+---------+------+---------------------------------------------+
| Warning | 1264 | Out of range value for column 'b1' at row 3 |
+---------+------+---------------------------------------------+

SELECT b1+0, HEX(b1), OCT(b1), BIN(b1) FROM b;
+------+---------+---------+----------+
| b1+0 | HEX(b1) | OCT(b1) | BIN(b1)  |
+------+---------+---------+----------+
|  255 | FF      | 377     | 11111111 |
|   85 | 55      | 125     | 1010101  |
|  255 | FF      | 377     | 11111111 |
+------+---------+---------+----------+
```