# MEDIUMINT

## Syntax

```sql
MEDIUMINT[(M)] [SIGNED | UNSIGNED | ZEROFILL]
```

## Description

A medium-sized integer. The signed range is -8388608 to 8388607. The
unsigned range is 0 to 16777215.

`ZEROFILL` pads the integer with zeroes and assumes `UNSIGNED` (even if `UNSIGNED` is not specified).

`INT3` is a synonym for `MEDIUMINT`.

For details on the attributes, see [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview).

## Examples

```sql
CREATE TABLE mediumints (a MEDIUMINT,b MEDIUMINT UNSIGNED,c MEDIUMINT ZEROFILL);

DESCRIBE mediumints;
+-------+--------------------------------+------+-----+---------+-------+
| Field | Type                           | Null | Key | Default | Extra |
+-------+--------------------------------+------+-----+---------+-------+
| a     | mediumint(9)                   | YES  |     | NULL    |       |
| b     | mediumint(8) unsigned          | YES  |     | NULL    |       |
| c     | mediumint(8) unsigned zerofill | YES  |     | NULL    |       |
+-------+--------------------------------+------+-----+---------+-------+
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) set, the default from [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/):

```sql
INSERT INTO mediumints VALUES (-10,-10,-10);
ERROR 1264 (22003): Out of range value for column 'b' at row 1

INSERT INTO mediumints VALUES (-10,10,-10);
ERROR 1264 (22003): Out of range value for column 'c' at row 1

INSERT INTO mediumints VALUES (-10,10,10);

INSERT INTO mediumints VALUES (8388608,8388608,8388608);
ERROR 1264 (22003): Out of range value for column 'a' at row 1

INSERT INTO mediumints VALUES (8388607,8388608,8388608);

SELECT * FROM mediumints;
+---------+---------+----------+
| a       | b       | c        |
+---------+---------+----------+
|     -10 |      10 | 00000010 |
| 8388607 | 8388608 | 08388608 |
+---------+---------+----------+
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) unset, the default until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/):

```sql
INSERT INTO mediumints VALUES (-10,-10,-10);
Query OK, 1 row affected, 2 warnings (0.05 sec)
Warning (Code 1264): Out of range value for column 'b' at row 1
Warning (Code 1264): Out of range value for column 'c' at row 1

INSERT INTO mediumints VALUES (-10,10,-10);
Query OK, 1 row affected, 1 warning (0.08 sec)
Warning (Code 1264): Out of range value for column 'c' at row 1

INSERT INTO mediumints VALUES (-10,10,10);

INSERT INTO mediumints VALUES (8388608,8388608,8388608);
Query OK, 1 row affected, 1 warning (0.05 sec)
Warning (Code 1264): Out of range value for column 'a' at row 1

INSERT INTO mediumints VALUES (8388607,8388608,8388608);

SELECT * FROM mediumints;
+---------+---------+----------+
| a       | b       | c        |
+---------+---------+----------+
|     -10 |       0 | 00000000 |
|     -10 |       0 | 00000000 |
|     -10 |      10 | 00000000 |
|     -10 |      10 | 00000010 |
| 8388607 | 8388608 | 08388608 |
| 8388607 | 8388608 | 08388608 |
+---------+---------+----------+
```

## See Also

- [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview)
- [TINYINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/tinyint)
- [SMALLINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/smallint)
- [INTEGER](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int)
- [BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint)