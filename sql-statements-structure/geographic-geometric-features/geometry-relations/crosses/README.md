# CROSSES

## Syntax

```sql
Crosses(g1,g2)
```

## Description

Returns `1` if `g1` spatially crosses `g2`. Returns `NULL` if `g1` is
a [Polygon](/sql-statements-structure/geographic-geometric-features/geometry-constructors/polygon/) or a [MultiPolygon](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multipolygon/), or if `g2` is a
[Point](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point/) or a [MultiPoint](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multipoint/). Otherwise, returns `0`.

The term spatially crosses denotes a spatial relation between two
given geometries that has the following properties:

- The two geometries intersect
- Their intersection results in a geometry that has a dimension that is one
  less than the maximum dimension of the two given geometries
- Their intersection is not equal to either of the two given geometries

CROSSES() is based on the original MySQL implementation, and uses object bounding rectangles, while [ST_CROSSES()](/kb/en/st_crosses/) uses object shapes.