# MLineFromWKB

## Syntax

```sql
MLineFromWKB(wkb[,srid])
MultiLineStringFromWKB(wkb[,srid])
```

## Description

Constructs a [MULTILINESTRING](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multilinestring/) value using its [WKB](/sql-statements-structure/geographic-geometric-features/wkb/well-known-binary-wkb-format/)  representation and [SRID](/kb/en/srid/).

`MLineFromWKB()` and `MultiLineStringFromWKB()` are synonyms.

## Examples

```sql
SET @g = ST_AsBinary(MLineFromText('MULTILINESTRING((10 48,10 21,10 0),(16 0,16 23,16 48))'));

SELECT ST_AsText(MLineFromWKB(@g));
+--------------------------------------------------------+
| ST_AsText(MLineFromWKB(@g))                            |
+--------------------------------------------------------+
| MULTILINESTRING((10 48,10 21,10 0),(16 0,16 23,16 48)) |
+--------------------------------------------------------+
```