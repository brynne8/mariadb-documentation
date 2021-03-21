# FLOAT

## Syntax

```sql
FLOAT[(M,D)] [SIGNED | UNSIGNED | ZEROFILL]
```

## Description

A small (single-precision) floating-point number (see [DOUBLE](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double) for a regular-size floating point number). Allowable values are:

- -3.402823466E+38 to -1.175494351E-38
- 0
- 1.175494351E-38 to 3.402823466E+38.

These are the theoretical limits, based on the IEEE 
standard. The actual range might be slightly smaller depending on your
hardware or operating system.

M is the total number of digits and D is the number of digits
following the decimal point. If M and D are omitted, values are stored
to the limits allowed by the hardware. A single-precision
floating-point number is accurate to approximately 7 decimal places.

UNSIGNED, if specified, disallows negative values.

Using FLOAT might give you some unexpected problems because all
calculations in MariaDB are done with double precision. See [Floating Point Accuracy](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/floating-point-accuracy).

For more details on the attributes, see [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview).