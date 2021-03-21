# DISJOINT

## Syntax

```sql
Disjoint(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether `g1` is spatially disjoint from
(does not intersect) `g2`.

DISJOINT() tests the opposite relationship to [INTERSECTS()](/sql-statements-structure/geographic-geometric-features/geometry-relations/intersects/).

DISJOINT() is based on the original MySQL implementation and uses object bounding rectangles, while [ST_DISJOINT()](/sql-statements-structure/geographic-geometric-features/geometry-relations/st_disjoint/) uses object shapes.