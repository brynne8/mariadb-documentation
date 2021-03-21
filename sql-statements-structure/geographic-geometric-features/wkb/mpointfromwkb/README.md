# MPointFromWKB

## Syntax

```sql
MPointFromWKB(wkb[,srid])
MultiPointFromWKB(wkb[,srid])
```

## Description

Constructs a [MULTIPOINT](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multipoint/) value using its [WKB](/sql-statements-structure/geographic-geometric-features/wkb/well-known-binary-wkb-format/) representation and [SRID](/kb/en/srid/).

`MPointFromWKB()` and `MultiPointFromWKB()` are synonyms.

## Examples

```sql
SET @g = ST_AsBinary(MPointFromText('MultiPoint( 1 1, 2 2, 5 3, 7 2, 9 3, 8 4, 6 6, 6 9, 4 9, 1 5 )'));

SELECT ST_AsText(MPointFromWKB(@g));
+-----------------------------------------------------+
| ST_AsText(MPointFromWKB(@g))                        |
+-----------------------------------------------------+
| MULTIPOINT(1 1,2 2,5 3,7 2,9 3,8 4,6 6,6 9,4 9,1 5) |
+-----------------------------------------------------+
```