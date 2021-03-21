# ATAN2

## Syntax

```sql
ATAN(Y,X), ATAN2(Y,X)
```

## Description

Returns the arc tangent of the two variables X and Y. It is similar to
calculating the arc tangent of Y / X, except that the signs of both
arguments are used to determine the quadrant of the result.

## Examples

```sql
SELECT ATAN(-2,2);
+---------------------+
| ATAN(-2,2)          |
+---------------------+
| -0.7853981633974483 |
+---------------------+

SELECT ATAN2(PI(),0);
+--------------------+
| ATAN2(PI(),0)      |
+--------------------+
| 1.5707963267948966 |
+--------------------+
```