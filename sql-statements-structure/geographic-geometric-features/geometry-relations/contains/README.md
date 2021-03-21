# CONTAINS

## Syntax

```sql
Contains(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether a geometry `g1` completely contains geometry `g2`. CONTAINS() is based on the original MySQL implementation and uses object bounding rectangles, while [ST_CONTAINS()](/kb/en/st_contains/) uses object shapes.

This tests the opposite relationship to [Within()](/sql-statements-structure/geographic-geometric-features/geometry-relations/within).