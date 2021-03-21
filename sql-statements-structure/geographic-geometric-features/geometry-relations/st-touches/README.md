# ST_TOUCHES

## Syntax

```sql
ST_TOUCHES(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether geometry <em>`g1`</em> spatially touches geometry <em>`g2`</em>. Two geometries spatially touch if the interiors of the geometries do not intersect,
but the boundary of one of the geometries intersects either the boundary or the
interior of the other.

ST_TOUCHES() uses object shapes, while [TOUCHES()](/sql-statements-structure/geographic-geometric-features/geometry-relations/touches/), based on the original MySQL implementation, uses object bounding rectangles.

## Examples

```sql
SET @g1 = ST_GEOMFROMTEXT('POINT(2 0)');

SET @g2 = ST_GEOMFROMTEXT('LINESTRING(2 0, 0 2)');

SELECT ST_TOUCHES(@g1,@g2);
+---------------------+
| ST_TOUCHES(@g1,@g2) |
+---------------------+
|                   1 |
+---------------------+

SET @g1 = ST_GEOMFROMTEXT('POINT(2 1)');

SELECT ST_TOUCHES(@g1,@g2);
+---------------------+
| ST_TOUCHES(@g1,@g2) |
+---------------------+
|                   0 |
+---------------------+
```