# ST_GeomFromText

## Syntax

```sql
ST_GeomFromText(wkt[,srid])
ST_GeometryFromText(wkt[,srid])
GeomFromText(wkt[,srid])
GeometryFromText(wkt[,srid])
```

## Description

Constructs a geometry value of any type using its [WKT](/sql-statements-structure/geographic-geometric-features/wkt/wkt-definition/) representation and [SRID](/kb/en/srid/).

`GeomFromText()`, `GeometryFromText()`, `ST_GeomFromText()` and `ST_GeometryFromText()` are all synonyms.

## Example

```sql
SET @g = ST_GEOMFROMTEXT('POLYGON((1 1,1 5,4 9,6 9,9 3,7 2,1 1))');
```