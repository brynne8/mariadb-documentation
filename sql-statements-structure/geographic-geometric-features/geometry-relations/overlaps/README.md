# OVERLAPS

## Syntax

```sql
OVERLAPS(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether `g1` spatially overlaps `g2`.
The term spatially overlaps is used if two geometries intersect and their
intersection results in a geometry of the same dimension but not equal to
either of the given geometries.

OVERLAPS() is based on the original MySQL implementation and uses object bounding rectangles, while [ST_OVERLAPS()](/kb/en/st_overlaps/) uses object shapes.