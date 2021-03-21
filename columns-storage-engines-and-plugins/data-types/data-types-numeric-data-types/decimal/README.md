# DECIMAL

## Syntax

```sql
DECIMAL[(M[,D])] [SIGNED | UNSIGNED | ZEROFILL]
```

## Description

A packed "exact" fixed-point number. `M` is the total number of digits (the
precision) and `D` is the number of digits after the decimal point (the
scale).

- The decimal point and (for negative numbers) the "-" sign are not
counted in `M`.
- If `D` is `0`, values have no decimal point or fractional
part and on [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert) the value will be rounded to the nearest `DECIMAL`.
- The maximum number of digits (`M`) for `DECIMAL` is 65.
- The maximum number of supported decimals (`D`) is `30` before [MariadB 10.2.1](/kb/en/mariadb-1021-release-notes/) and `38` afterwards.
- If `D` is omitted, the default is `0`. If `M` is omitted, the default is `10`.

`UNSIGNED`, if specified, disallows negative values.

`ZEROFILL`, if specified, pads the number with zeros, up to the total number
of digits specified by `M`.

All basic calculations (+, -, *, /) with `DECIMAL` columns are done with
a precision of 65 digits.

For more details on the attributes, see [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview).

`DEC`, `NUMERIC` and `FIXED` are synonyms, as well as `NUMBER` in [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types).

## Examples

```sql
CREATE TABLE t1 (d DECIMAL UNSIGNED ZEROFILL);

INSERT INTO t1 VALUES (1),(2),(3),(4.0),(5.2),(5.7);
Query OK, 6 rows affected, 2 warnings (0.16 sec)
Records: 6  Duplicates: 0  Warnings: 2

Note (Code 1265): Data truncated for column 'd' at row 5
Note (Code 1265): Data truncated for column 'd' at row 6

SELECT * FROM t1;
+------------+
| d          |
+------------+
| 0000000001 |
| 0000000002 |
| 0000000003 |
| 0000000004 |
| 0000000005 |
| 0000000006 |
+------------+
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) set, the default from [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/):

```sql
INSERT INTO t1 VALUES (-7);
ERROR 1264 (22003): Out of range value for column 'd' at row 1
```

With [strict_mode](/kb/en/sql-mode/#strict-mode) unset, the default until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/):

```sql
INSERT INTO t1 VALUES (-7);
Query OK, 1 row affected, 1 warning (0.02 sec)
Warning (Code 1264): Out of range value for column 'd' at row 1

SELECT * FROM t1;
+------------+
| d          |
+------------+
| 0000000001 |
| 0000000002 |
| 0000000003 |
| 0000000004 |
| 0000000005 |
| 0000000006 |
| 0000000000 |
+------------+
```

## See Also

- [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types)