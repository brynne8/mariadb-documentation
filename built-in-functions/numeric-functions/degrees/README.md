# DEGREES

## Syntax

```sql
DEGREES(X)
```

## Description

Returns the argument <em>`X`</em>, converted from radians to degrees.

This is the converse of the [RADIANS()](/built-in-functions/numeric-functions/radians/) function.

## Examples

```sql
SELECT DEGREES(PI());
+---------------+
| DEGREES(PI()) |
+---------------+
|           180 |
+---------------+

SELECT DEGREES(PI() / 2);
+-------------------+
| DEGREES(PI() / 2) |
+-------------------+
|                90 |
+-------------------+

SELECT DEGREES(45);
+-----------------+
| DEGREES(45)     |
+-----------------+
| 2578.3100780887 |
+-----------------+
```