# ST_PolyFromWKB

## Syntax

```sql
ST_PolyFromWKB(wkb[,srid])
ST_PolygonFromWKB(wkb[,srid])
PolyFromWKB(wkb[,srid])
PolygonFromWKB(wkb[,srid])
```

## Description

Constructs a [POLYGON](/sql-statements-structure/geographic-geometric-features/geometry-constructors/polygon/) value using its [WKB](/sql-statements-structure/geographic-geometric-features/wkb/well-known-binary-wkb-format/) representation and [SRID](/kb/en/srid/).

`ST_PolyFromWKB()`, `ST_PolygonFromWKB()`, `PolyFromWKB()` and `PolygonFromWKB()` are synonyms.

## Examples

```sql
SET @g = ST_AsBinary(ST_PolyFromText('POLYGON((1 1,1 5,4 9,6 9,9 3,7 2,1 1))'));

SELECT ST_AsText(ST_PolyFromWKB(@g)) AS p;
+----------------------------------------+
| p                                      |
+----------------------------------------+
| POLYGON((1 1,1 5,4 9,6 9,9 3,7 2,1 1)) |
+----------------------------------------+
```