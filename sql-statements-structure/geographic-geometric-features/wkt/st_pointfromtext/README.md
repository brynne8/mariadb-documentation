# ST_PointFromText

## Syntax

```sql
ST_PointFromText(wkt[,srid])
PointFromText(wkt[,srid])
```

## Description

Constructs a [POINT](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point/) value using its [WKT](/sql-statements-structure/geographic-geometric-features/wkt/wkt-definition/) representation and [SRID](/kb/en/srid/).

`ST_PointFromText()` and `PointFromText()` are synonyms.

## Examples

```sql
CREATE TABLE gis_point  (g POINT);
SHOW FIELDS FROM gis_point;
INSERT INTO gis_point VALUES
    (PointFromText('POINT(10 10)')),
    (PointFromText('POINT(20 10)')),
    (PointFromText('POINT(20 20)')),
    (PointFromWKB(AsWKB(PointFromText('POINT(10 20)'))));
```