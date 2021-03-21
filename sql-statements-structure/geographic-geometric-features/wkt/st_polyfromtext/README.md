# ST_PolyFromText

## Syntax

```sql
ST_PolyFromText(wkt[,srid])
ST_PolygonFromText(wkt[,srid])
PolyFromText(wkt[,srid])
PolygonFromText(wkt[,srid])
```

## Description

Constructs a [POLYGON](/sql-statements-structure/geographic-geometric-features/geometry-constructors/polygon) value using its [WKT](/sql-statements-structure/geographic-geometric-features/wkt/wkt-definition) representation and [SRID](/kb/en/srid/).

`ST_PolyFromText()`, `ST_PolygonFromText()`, `PolyFromText()` and `ST_PolygonFromText()` are all synonyms.

## Examples

```sql
CREATE TABLE gis_polygon   (g POLYGON);
INSERT INTO gis_polygon VALUES
    (PolygonFromText('POLYGON((10 10,20 10,20 20,10 20,10 10))')),
    (PolyFromText('POLYGON((0 0,50 0,50 50,0 50,0 0), (10 10,20 10,20 20,10 20,10 10))'));
```