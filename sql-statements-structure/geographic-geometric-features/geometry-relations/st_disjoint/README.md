# ST_DISJOINT

## Syntax

```sql
ST_DISJOINT(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether geometry <em>`g1`</em> is spatially disjoint from
(does not intersect with) geometry <em>`g2`</em>.

ST_DISJOINT() uses object shapes, while [DISJOINT()](/sql-statements-structure/geographic-geometric-features/geometry-relations/disjoint/), based on the original MySQL implementation, uses object bounding rectangles.

ST_DISJOINT() tests the opposite relationship to [ST_INTERSECTS()](/kb/en/st_intersects/).

## Examples

```sql
SET @g1 = ST_GEOMFROMTEXT('POINT(0 0)');

SET @g2 = ST_GEOMFROMTEXT('LINESTRING(2 0, 0 2)');

SELECT ST_DISJOINT(@g1,@g2);
+----------------------+
| ST_DISJOINT(@g1,@g2) |
+----------------------+
|                    1 |
+----------------------+

SET @g2 = ST_GEOMFROMTEXT('LINESTRING(0 0, 0 2)');

SELECT ST_DISJOINT(@g1,@g2);
+----------------------+
| ST_DISJOINT(@g1,@g2) |
+----------------------+
|                    0 |
+----------------------+
```