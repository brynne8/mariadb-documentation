# ST_NUMPOINTS

## Syntax

```sql
ST_NumPoints(ls)
NumPoints(ls)
```

## Description

Returns the number of [Point](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point/) objects in the [LineString](/sql-statements-structure/geographic-geometric-features/geometry-constructors/linestring/)
value `ls`.

`ST_NumPoints()` and `NumPoints()` are synonyms.

## Examples

```sql
SET @ls = 'LineString(1 1,2 2,3 3)';

SELECT NumPoints(GeomFromText(@ls));
+------------------------------+
| NumPoints(GeomFromText(@ls)) |
+------------------------------+
|                            3 |
+------------------------------+
```