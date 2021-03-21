# MPolyFromText

## Syntax

```sql
MPolyFromText(wkt[,srid])
MultiPolygonFromText(wkt[,srid])
```

## Description

Constructs a [MULTIPOLYGON](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multipolygon/) value using its [WKT](/sql-statements-structure/geographic-geometric-features/wkt/wkt-definition/) representation and [SRID](/kb/en/srid/).

`MPolyFromText()` and `MultiPolygonFromText()` are synonyms.

## Examples

```sql
CREATE TABLE gis_multi_polygon  (g MULTIPOLYGON);
SHOW FIELDS FROM gis_multi_polygon;
INSERT INTO gis_multi_polygon VALUES
    (MultiPolygonFromText('MULTIPOLYGON(((28 26,28 0,84 0,84 42,28 26),(52 18,66 23,73 9,48 6,52 18)),((59 18,67 18,67 13,59 13,59 18)))')),
    (MPolyFromText('MULTIPOLYGON(((28 26,28 0,84 0,84 42,28 26),(52 18,66 23,73 9,48 6,52 18)),((59 18,67 18,67 13,59 13,59 18)))')),
    (MPolyFromWKB(AsWKB(MultiPolygon(Polygon(LineString(Point(0, 3), Point(3, 3), Point(3, 0), Point(0, 3)))))));
```