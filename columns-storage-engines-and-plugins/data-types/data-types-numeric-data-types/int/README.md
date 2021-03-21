# INT

## Syntax

```sql
INT[(M)] [SIGNED | UNSIGNED | ZEROFILL]
INTEGER[(M)] [SIGNED | UNSIGNED | ZEROFILL]
```

## Description

A normal-size integer. When marked UNSIGNED, it ranges from 0 to 4294967295, otherwise its range is -2147483648 to 2147483647 (SIGNED is the default). If a column has been set to ZEROFILL, all values will be prepended by zeros so that the INT value contains a number of M digits. INTEGER is a synonym for INT.

<strong>Note:</strong> If the ZEROFILL attribute has been specified, the column will automatically become UNSIGNED.

`INT4` is a synonym for `INT`.

For details on the attributes, see [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview/).

## Examples

```sql
CREATE TABLE ints (a INT,b INT UNSIGNED,c INT ZEROFILL);
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) set, the default from [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/):

```sql
INSERT INTO ints VALUES (-10,-10,-10);
ERROR 1264 (22003): Out of range value for column 'b' at row 1

INSERT INTO ints VALUES (-10,10,-10);
ERROR 1264 (22003): Out of range value for column 'c' at row 1

INSERT INTO ints VALUES (-10,10,10);

INSERT INTO ints VALUES (2147483648,2147483648,2147483648);
ERROR 1264 (22003): Out of range value for column 'a' at row 1

INSERT INTO ints VALUES (2147483647,2147483648,2147483648);

SELECT * FROM ints;
+------------+------------+------------+
| a          | b          | c          |
+------------+------------+------------+
|        -10 |         10 | 0000000010 |
| 2147483647 | 2147483648 | 2147483648 |
+------------+------------+------------+
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) unset, the default until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/):

```sql
INSERT INTO ints VALUES (-10,-10,-10);
Query OK, 1 row affected, 2 warnings (0.10 sec)
Warning (Code 1264): Out of range value for column 'b' at row 1
Warning (Code 1264): Out of range value for column 'c' at row 1

INSERT INTO ints VALUES (-10,10,-10);
Query OK, 1 row affected, 1 warning (0.08 sec)
Warning (Code 1264): Out of range value for column 'c' at row 1

INSERT INTO ints VALUES (-10,10,10);

INSERT INTO ints VALUES (2147483648,2147483648,2147483648);
Query OK, 1 row affected, 1 warning (0.07 sec)
Warning (Code 1264): Out of range value for column 'a' at row 1

INSERT INTO ints VALUES (2147483647,2147483648,2147483648);

SELECT * FROM ints;
+------------+------------+------------+
| a          | b          | c          |
+------------+------------+------------+
|        -10 |          0 | 0000000000 |
|        -10 |         10 | 0000000000 |
|        -10 |         10 | 0000000010 |
| 2147483647 | 2147483648 | 2147483648 |
| 2147483647 | 2147483648 | 2147483648 |
+------------+------------+------------+
```

## See Also

- [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview/)
- [TINYINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/tinyint/)
- [SMALLINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/smallint/)
- [MEDIUMINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/mediumint/)
- [BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint/)