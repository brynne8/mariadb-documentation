# ST_CROSSES

## Syntax

```sql
ST_CROSSES(g1,g2)
```

## Description

Returns `1` if geometry <em>`g1`</em> spatially crosses geometry <em>`g2`</em>. Returns `NULL` if `g1` is a [Polygon](/sql-statements-structure/geographic-geometric-features/geometry-constructors/polygon) or a [MultiPolygon](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multipolygon), or if `g2` is a
[Point](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point) or a [MultiPoint](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multipoint). Otherwise, returns `0`.

The term spatially crosses denotes a spatial relation between two
given geometries that has the following properties:

- The two geometries intersect
- Their intersection results in a geometry that has a dimension that is one
  less than the maximum dimension of the two given geometries
- Their intersection is not equal to either of the two given geometries

ST_CROSSES() uses object shapes, while [CROSSES()](/sql-statements-structure/geographic-geometric-features/geometry-relations/crosses), based on the original MySQL implementation, uses object bounding rectangles.

## Examples

```sql
SET @g1 = ST_GEOMFROMTEXT('LINESTRING(174 149, 176 151)');

SET @g2 = ST_GEOMFROMTEXT('POLYGON((175 150, 20 40, 50 60, 125 100, 175 150))');

SELECT ST_CROSSES(@g1,@g2);
+---------------------+
| ST_CROSSES(@g1,@g2) |
+---------------------+
|                   1 |
+---------------------+

SET @g1 = ST_GEOMFROMTEXT('LINESTRING(176 149, 176 151)');

SELECT ST_CROSSES(@g1,@g2);
+---------------------+
| ST_CROSSES(@g1,@g2) |
+---------------------+
|                   0 |
+---------------------+
```