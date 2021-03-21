# TINYINT

## Syntax

```sql
TINYINT[(M)] [SIGNED | UNSIGNED | ZEROFILL]
```

## Description

A very small [integer](/kb/en/sql_language-data_types-int/). The signed range is -128 to 127. The unsigned range is 0 to 255. For details on the attributes, see [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview/).

[INT1](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int1/) is a synonym for `TINYINT`. [BOOL and BOOLEAN](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/boolean/) are synonyms for `TINYINT(1)`.

## Examples

```sql
CREATE TABLE tinyints (a TINYINT,b TINYINT UNSIGNED,c TINYINT ZEROFILL);
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) set, the default from [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/):

```sql
INSERT INTO tinyints VALUES (-10,-10,-10);
ERROR 1264 (22003): Out of range value for column 'b' at row 1

INSERT INTO tinyints VALUES (-10,10,-10);
ERROR 1264 (22003): Out of range value for column 'c' at row 1

INSERT INTO tinyints VALUES (-10,10,10);

SELECT * FROM tinyints;
+------+------+------+
| a    | b    | c    |
+------+------+------+
|  -10 |   10 |  010 |
+------+------+------+

INSERT INTO tinyints VALUES (128,128,128);
ERROR 1264 (22003): Out of range value for column 'a' at row 1

INSERT INTO tinyints VALUES (127,128,128);

SELECT * FROM tinyints;
+------+------+------+
| a    | b    | c    |
+------+------+------+
|  -10 |   10 |  010 |
|  127 |  128 |  128 |
+------+------+------+
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) unset, the default until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/):

```sql
INSERT INTO tinyints VALUES (-10,-10,-10);
Query OK, 1 row affected, 2 warnings (0.08 sec)
Warning (Code 1264): Out of range value for column 'b' at row 1
Warning (Code 1264): Out of range value for column 'c' at row 1

INSERT INTO tinyints VALUES (-10,10,-10);
Query OK, 1 row affected, 1 warning (0.11 sec)
Warning (Code 1264): Out of range value for column 'c' at row 1

INSERT INTO tinyints VALUES (-10,10,10);

SELECT * FROM tinyints;
+------+------+------+
| a    | b    | c    |
+------+------+------+
|  -10 |    0 |  000 |
|  -10 |   10 |  000 |
|  -10 |   10 |  010 |
+------+------+------+

INSERT INTO tinyints VALUES (128,128,128);
Query OK, 1 row affected, 1 warning (0.19 sec)
Warning (Code 1264): Out of range value for column 'a' at row 1

INSERT INTO tinyints VALUES (127,128,128);

SELECT * FROM tinyints;
+------+------+------+
| a    | b    | c    |
+------+------+------+
|  -10 |    0 |  000 |
|  -10 |   10 |  000 |
|  -10 |   10 |  010 |
|  127 |  128 |  128 |
|  127 |  128 |  128 |
+------+------+------+
```

## See Also

- [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview/)
- [SMALLINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/smallint/)
- [MEDIUMINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/mediumint/)
- [INTEGER](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int/)
- [BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint/)
- [BOOLEAN](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/boolean/)