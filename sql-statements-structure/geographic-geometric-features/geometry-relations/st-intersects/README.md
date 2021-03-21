# ST_INTERSECTS

## Syntax

```sql
ST_INTERSECTS(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether geometry <em>`g1`</em> spatially intersects geometry <em>`g2`</em>.

ST_INTERSECTS() uses object shapes, while [INTERSECTS()](/sql-statements-structure/geographic-geometric-features/geometry-relations/intersects), based on the original MySQL implementation, uses object bounding rectangles.

ST_INTERSECTS() tests the opposite relationship to [ST_DISJOINT()](/sql-statements-structure/geographic-geometric-features/geometry-relations/st_disjoint).

## Examples

```sql
SET @g1 = ST_GEOMFROMTEXT('POINT(0 0)');

SET @g2 = ST_GEOMFROMTEXT('LINESTRING(0 0, 0 2)');

SELECT ST_INTERSECTS(@g1,@g2);
+------------------------+
| ST_INTERSECTS(@g1,@g2) |
+------------------------+
|                      1 |
+------------------------+
```

```sql
SET @g2 = ST_GEOMFROMTEXT('LINESTRING(2 0, 0 2)');

SELECT ST_INTERSECTS(@g1,@g2);
+------------------------+
| ST_INTERSECTS(@g1,@g2) |
+------------------------+
|                      0 |
+------------------------+
```