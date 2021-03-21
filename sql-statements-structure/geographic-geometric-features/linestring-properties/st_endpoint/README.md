# ST_ENDPOINT

## Syntax

```sql
ST_EndPoint(ls)
EndPoint(ls)
```

## Description

Returns the [Point](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point/) that is the endpoint of the
[LineString](/sql-statements-structure/geographic-geometric-features/geometry-constructors/linestring/) value `ls`.

`ST_EndPoint()` and `EndPoint()` are synonyms.

## Examples

```sql
SET @ls = 'LineString(1 1,2 2,3 3)';

SELECT AsText(EndPoint(GeomFromText(@ls)));
+-------------------------------------+
| AsText(EndPoint(GeomFromText(@ls))) |
+-------------------------------------+
| POINT(3 3)                          |
+-------------------------------------+
```