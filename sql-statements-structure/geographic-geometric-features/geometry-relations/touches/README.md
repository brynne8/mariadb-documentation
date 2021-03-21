# TOUCHES

## Syntax

```sql
Touches(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether `g1` spatially touches `g2`. Two
geometries spatially touch if the interiors of the geometries do not intersect,
but the boundary of one of the geometries intersects either the boundary or the
interior of the other.

TOUCHES() is based on the original MySQL implementation and uses object bounding rectangles, while [ST_TOUCHES()](/kb/en/st_touches/) uses object shapes.