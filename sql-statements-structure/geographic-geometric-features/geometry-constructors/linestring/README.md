# LINESTRING

## Syntax

```sql
LineString(pt1,pt2,...)
```

## Description

Constructs a [WKB](/sql-statements-structure/geographic-geometric-features/wkb) LineString value from a number of WKB [Point](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point) arguments.  If any argument is not a WKB Point, the return value is
`NULL`. If the number of [Point](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point) arguments is less than two, the return value is `NULL`.

## Examples

```sql
SET @ls = 'LineString(1 1,2 2,3 3)';

SELECT AsText(EndPoint(GeomFromText(@ls)));
+-------------------------------------+
| AsText(EndPoint(GeomFromText(@ls))) |
+-------------------------------------+
| POINT(3 3)                          |
+-------------------------------------+

CREATE TABLE gis_line  (g LINESTRING);
INSERT INTO gis_line VALUES
    (LineFromText('LINESTRING(0 0,0 10,10 0)')),
    (LineStringFromText('LINESTRING(10 10,20 10,20 20,10 20,10 10)')),
    (LineStringFromWKB(AsWKB(LineString(Point(10, 10), Point(40, 10)))));
```