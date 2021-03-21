# RADIANS

## Syntax

```sql
RADIANS(X)
```

## Description

Returns the argument <em>`X`</em>, converted from degrees to radians. Note that
π radians equals 180 degrees.

This is the converse of the [DEGREES()](/built-in-functions/numeric-functions/degrees) function.

## Examples

```sql
SELECT RADIANS(45);
+-------------------+
| RADIANS(45)       |
+-------------------+
| 0.785398163397448 |
+-------------------+

SELECT RADIANS(90);
+-----------------+
| RADIANS(90)     |
+-----------------+
| 1.5707963267949 |
+-----------------+

SELECT RADIANS(PI());
+--------------------+
| RADIANS(PI())      |
+--------------------+
| 0.0548311355616075 |
+--------------------+

SELECT RADIANS(180);
+------------------+
| RADIANS(180)     |
+------------------+
| 3.14159265358979 |
+------------------+
```