# ST_POINTN

## Syntax

```sql
ST_PointN(ls,N)
PointN(ls,N)
```

## Description

Returns the N-th [Point](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point/) in the [LineString](/sql-statements-structure/geographic-geometric-features/geometry-constructors/linestring/) value `ls`.
Points are numbered beginning with `1`.

`ST_PointN()` and `PointN()` are synonyms.

## Examples

```sql
SET @ls = 'LineString(1 1,2 2,3 3)';

SELECT AsText(PointN(GeomFromText(@ls),2));
+-------------------------------------+
| AsText(PointN(GeomFromText(@ls),2)) |
+-------------------------------------+
| POINT(2 2)                          |
+-------------------------------------+
```