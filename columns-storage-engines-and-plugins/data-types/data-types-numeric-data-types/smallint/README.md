# SMALLINT

## Syntax

```sql
SMALLINT[(M)] [SIGNED | UNSIGNED | ZEROFILL]
```

## Description

A small [integer](/kb/en/sql_language-data_types-int/). The signed range is -32768 to 32767. The unsigned range is 0 to 65535.

If a column has been set to ZEROFILL, all values will be prepended by zeros so that the SMALLINT value contains a number of M digits.

<strong>Note:</strong> If the ZEROFILL attribute has been specified, the column will automatically become UNSIGNED.

`INT2` is a synonym for `SMALLINT`.

For more details on the attributes, see [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview).

## Examples

```sql
CREATE TABLE smallints (a SMALLINT,b SMALLINT UNSIGNED,c SMALLINT ZEROFILL);
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) set, the default from [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/):

```sql
INSERT INTO smallints VALUES (-10,-10,-10);
ERROR 1264 (22003): Out of range value for column 'b' at row 1

INSERT INTO smallints VALUES (-10,10,-10);
ERROR 1264 (22003): Out of range value for column 'c' at row 1

INSERT INTO smallints VALUES (-10,10,10);

INSERT INTO smallints VALUES (32768,32768,32768);
ERROR 1264 (22003): Out of range value for column 'a' at row 1

INSERT INTO smallints VALUES (32767,32768,32768);

SELECT * FROM smallints;
+-------+-------+-------+
| a     | b     | c     |
+-------+-------+-------+
|   -10 |    10 | 00010 |
| 32767 | 32768 | 32768 |
+-------+-------+-------+
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) unset, the default until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/):

```sql
INSERT INTO smallints VALUES (-10,-10,-10);
Query OK, 1 row affected, 2 warnings (0.09 sec)
Warning (Code 1264): Out of range value for column 'b' at row 1
Warning (Code 1264): Out of range value for column 'c' at row 1

INSERT INTO smallints VALUES (-10,10,-10);
Query OK, 1 row affected, 1 warning (0.08 sec)
Warning (Code 1264): Out of range value for column 'c' at row 1

INSERT INTO smallints VALUES (-10,10,10);

INSERT INTO smallints VALUES (32768,32768,32768);
Query OK, 1 row affected, 1 warning (0.04 sec)
Warning (Code 1264): Out of range value for column 'a' at row 1

INSERT INTO smallints VALUES (32767,32768,32768);

SELECT * FROM smallints;
+-------+-------+-------+
| a     | b     | c     |
+-------+-------+-------+
|   -10 |     0 | 00000 |
|   -10 |    10 | 00000 |
|   -10 |    10 | 00010 |
| 32767 | 32768 | 32768 |
| 32767 | 32768 | 32768 |
+-------+-------+-------+
```

## See Also

- [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview)
- [TINYINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/tinyint)
- [MEDIUMINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/mediumint)
- [INTEGER](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int)
- [BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint)