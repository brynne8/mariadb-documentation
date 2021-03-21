# BIGINT

## Syntax

```sql
BIGINT[(M)] [SIGNED | UNSIGNED | ZEROFILL]
```

## Description

A large integer. The signed range is `-9223372036854775808` to
`9223372036854775807`. The unsigned range is `0` to
`18446744073709551615`.

If a column has been set to ZEROFILL, all values will be prepended by zeros so that the BIGINT value contains a number of M digits.

<strong>Note:</strong> If the ZEROFILL attribute has been specified, the column will automatically become UNSIGNED.

For more details on the attributes, see [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview).

`SERIAL` is an alias for:

```sql
BIGINT UNSIGNED NOT NULL AUTO_INCREMENT UNIQUE
```

`INT8` is a synonym for `BIGINT`.

## Examples

```sql
CREATE TABLE bigints (a BIGINT,b BIGINT UNSIGNED,c BIGINT ZEROFILL);
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) set, the default from [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/):

```sql
INSERT INTO bigints VALUES (-10,-10,-10);
ERROR 1264 (22003): Out of range value for column 'b' at row 1

INSERT INTO bigints VALUES (-10,10,-10);
ERROR 1264 (22003): Out of range value for column 'c' at row 1

INSERT INTO bigints VALUES (-10,10,10);

INSERT INTO bigints VALUES (9223372036854775808,9223372036854775808,9223372036854775808);
ERROR 1264 (22003): Out of range value for column 'a' at row 1

INSERT INTO bigints VALUES (9223372036854775807,9223372036854775808,9223372036854775808);

SELECT * FROM bigints;
+---------------------+---------------------+----------------------+
| a                   | b                   | c                    |
+---------------------+---------------------+----------------------+
|                 -10 |                  10 | 00000000000000000010 |
| 9223372036854775807 | 9223372036854775808 | 09223372036854775808 |
+---------------------+---------------------+----------------------+
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) unset, the default until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/):

```sql
INSERT INTO bigints VALUES (-10,-10,-10);
Query OK, 1 row affected, 2 warnings (0.08 sec)
Warning (Code 1264): Out of range value for column 'b' at row 1
Warning (Code 1264): Out of range value for column 'c' at row 1

INSERT INTO bigints VALUES (-10,10,-10);
Query OK, 1 row affected, 1 warning (0.08 sec)
Warning (Code 1264): Out of range value for column 'c' at row 1

INSERT INTO bigints VALUES (-10,10,10);

INSERT INTO bigints VALUES (9223372036854775808,9223372036854775808,9223372036854775808);
Query OK, 1 row affected, 1 warning (0.07 sec)
Warning (Code 1264): Out of range value for column 'a' at row 1

INSERT INTO bigints VALUES (9223372036854775807,9223372036854775808,9223372036854775808);

SELECT * FROM bigints;
+---------------------+---------------------+----------------------+
| a                   | b                   | c                    |
+---------------------+---------------------+----------------------+
|                 -10 |                   0 | 00000000000000000000 |
|                 -10 |                  10 | 00000000000000000000 |
|                 -10 |                  10 | 00000000000000000010 |
| 9223372036854775807 | 9223372036854775808 | 09223372036854775808 |
| 9223372036854775807 | 9223372036854775808 | 09223372036854775808 |
+---------------------+---------------------+----------------------+
```

## See Also

- [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview)
- [TINYINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/tinyint)
- [SMALLINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/smallint)
- [MEDIUMINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/mediumint)
- [INTEGER](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int)