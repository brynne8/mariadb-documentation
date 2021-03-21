# INTERSECTS

## Syntax

```sql
INTERSECTS(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether geometry <em>`g1`</em> spatially intersects geometry <em>`g2`</em>.

INTERSECTS() is based on the original MySQL implementation and uses object bounding rectangles, while [ST_INTERSECTS()](/kb/en/st_intersects/) uses object shapes.

INTERSECTS() tests the opposite relationship to [DISJOINT()](/sql-statements-structure/geographic-geometric-features/geometry-relations/disjoint/).