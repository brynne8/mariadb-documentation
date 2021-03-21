# MULTIPOLYGON

## Syntax

```sql
MultiPolygon(poly1,poly2,...)
```

## Description

Constructs a [WKB](/sql-statements-structure/geographic-geometric-features/wkb) MultiPolygon value from a set of WKB [Polygon](/sql-statements-structure/geographic-geometric-features/geometry-constructors/polygon) arguments. If any argument is not a WKB Polygon, the return value is `NULL`.

## Example

```sql
CREATE TABLE gis_multi_polygon  (g MULTIPOLYGON);
INSERT INTO gis_multi_polygon VALUES
    (MultiPolygonFromText('MULTIPOLYGON(((28 26,28 0,84 0,84 42,28 26),(52 18,66 23,73 9,48 6,52 18)),((59 18,67 18,67 13,59 13,59 18)))')),
    (MPolyFromText('MULTIPOLYGON(((28 26,28 0,84 0,84 42,28 26),(52 18,66 23,73 9,48 6,52 18)),((59 18,67 18,67 13,59 13,59 18)))')),
    (MPolyFromWKB(AsWKB(MultiPolygon(Polygon(LineString(Point(0, 3), Point(3, 3), Point(3, 0), Point(0, 3)))))));
```