# ST_OVERLAPS

## Syntax

```sql
ST_OVERLAPS(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether geometry <em>`g1`</em> spatially overlaps geometry <em>`g2`</em>.

The term spatially overlaps is used if two geometries intersect and their
intersection results in a geometry of the same dimension but not equal to
either of the given geometries.

ST_OVERLAPS() uses object shapes, while [OVERLAPS()](/sql-statements-structure/geographic-geometric-features/geometry-relations/overlaps/), based on the original MySQL implementation, uses object bounding rectangles.