# WITHIN

## Syntax

```sql
Within(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether `g1` is spatially within `g2`.
This tests the opposite relationship as [Contains()](/sql-statements-structure/geographic-geometric-features/geometry-relations/contains).

WITHIN() is based on the original MySQL implementation, and uses object bounding rectangles, while [ST_WITHIN()](/kb/en/st_within/) uses object shapes.

## Examples

```sql
SET @g1 = GEOMFROMTEXT('POINT(174 149)');
SET @g2 = GEOMFROMTEXT('POINT(176 151)');
SET @g3 = GEOMFROMTEXT('POLYGON((175 150, 20 40, 50 60, 125 100, 175 150))');

SELECT within(@g1,@g3);
+-----------------+
| within(@g1,@g3) |
+-----------------+
|               1 |
+-----------------+

SELECT within(@g2,@g3);
+-----------------+
| within(@g2,@g3) |
+-----------------+
|               0 |
+-----------------+
```