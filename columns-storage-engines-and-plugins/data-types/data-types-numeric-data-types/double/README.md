# DOUBLE

## Syntax

```sql
DOUBLE[(M,D)] [SIGNED | UNSIGNED | ZEROFILL]
DOUBLE PRECISION[(M,D)] [SIGNED | UNSIGNED | ZEROFILL]
REAL[(M,D)] [SIGNED | UNSIGNED | ZEROFILL]
```

## Description

A normal-size (double-precision) floating-point number (see [FLOAT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/float/) for a single-precision floating-point number).

Allowable values are:

- `-1.7976931348623157E+308` <em>to</em> `-2.2250738585072014E-308`
- `0`
- `2.2250738585072014E-308` <em>to</em> `1.7976931348623157E+308`

These are the theoretical limits, based on the IEEE standard. The actual range
might be slightly smaller depending on your hardware or operating system.

`M` is the total number of digits and `D` is the number of digits
following the decimal point. If `M` and `D` are omitted, values are stored
to the limits allowed by the hardware. A double-precision
floating-point number is accurate to approximately 15 decimal places.

`UNSIGNED`, if specified, disallows negative values.

`ZEROFILL`, if specified, pads the number with zeros, up to the total number
of digits specified by `M`.

REAL and DOUBLE PRECISION are synonyms, unless the REAL_AS_FLOAT [SQL mode](/kb/en/sql_mode/) is enabled, in which case REAL is a synonym for [FLOAT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/float/) rather than DOUBLE.

See [Floating Point Accuracy](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/floating-point-accuracy/) for issues when using floating-point numbers.

For more details on the attributes, see [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview/).

## Examples

```sql
CREATE TABLE t1 (d DOUBLE(5,0) zerofill);

INSERT INTO t1 VALUES (1),(2),(3),(4);

SELECT * FROM t1;
+-------+
| d     |
+-------+
| 00001 |
| 00002 |
| 00003 |
| 00004 |
+-------+
```