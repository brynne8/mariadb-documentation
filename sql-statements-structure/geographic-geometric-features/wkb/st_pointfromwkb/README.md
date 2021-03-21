# ST_PointFromWKB

## Syntax

```sql
ST_PointFromWKB(wkb[,srid])
PointFromWKB(wkb[,srid])
```

## Description

Constructs a [POINT](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point) value using its [WKB](/sql-statements-structure/geographic-geometric-features/wkb/well-known-binary-wkb-format) representation and [SRID](/kb/en/srid/).

`ST_PointFromWKB()` and `PointFromWKB()` are synonyms.

## Examples

```sql
SET @g = ST_AsBinary(ST_PointFromText('POINT(0 4)'));

SELECT ST_AsText(ST_PointFromWKB(@g)) AS p;
+------------+
| p          |
+------------+
| POINT(0 4) |
+------------+
```