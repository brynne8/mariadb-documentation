# MPolyFromWKB

## Syntax

```sql
MPolyFromWKB(wkb[,srid])
MultiPolygonFromWKB(wkb[,srid])
```

## Description

Constructs a [MULTIPOLYGON](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multipolygon) value using its [WKB](/sql-statements-structure/geographic-geometric-features/wkb/well-known-binary-wkb-format)  representation and [SRID](/kb/en/srid/).

`MPolyFromWKB()` and `MultiPolygonFromWKB()` are synonyms.

## Examples

```sql
SET @g = ST_AsBinary(MPointFromText('MULTIPOLYGON(((28 26,28 0,84 0,84 42,28 26),(52 18,66 23,73 9,48 6,52 18)),((59 18,67 18,67 13,59 13,59 18)))'));

SELECT ST_AsText(MPolyFromWKB(@g));
+---------------------------------------------------------------------------------------------------------------+
| ST_AsText(MPolyFromWKB(@g))                                                                                   |
+---------------------------------------------------------------------------------------------------------------+
| MULTIPOLYGON(((28 26,28 0,84 0,84 42,28 26),(52 18,66 23,73 9,48 6,52 18)),((59 18,67 18,67 13,59 13,59 18))) |
+---------------------------------------------------------------------------------------------------------------+
```