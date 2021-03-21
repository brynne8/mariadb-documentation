# ST_STARTPOINT

## Syntax

```sql
ST_StartPoint(ls)
StartPoint(ls)
```

## Description

Returns the [Point](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point/) that is the start point of the
[LineString](/sql-statements-structure/geographic-geometric-features/geometry-constructors/linestring/) value `ls`.

`ST_StartPoint()` and `StartPoint()` are synonyms.

## Examples

```sql
SET @ls = 'LineString(1 1,2 2,3 3)';

SELECT AsText(StartPoint(GeomFromText(@ls)));
+---------------------------------------+
| AsText(StartPoint(GeomFromText(@ls))) |
+---------------------------------------+
| POINT(1 1)                            |
+---------------------------------------+
```