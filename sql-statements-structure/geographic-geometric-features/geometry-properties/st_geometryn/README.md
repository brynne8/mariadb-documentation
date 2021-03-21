# ST_GEOMETRYN

## Syntax

```sql
ST_GeometryN(gc,N)
GeometryN(gc,N)
```

## Description

Returns the N-th geometry in the GeometryCollection <em>`gc`.</em> Geometries are numbered beginning with 1.

`ST_GeometryN()` and `GeometryN()` are synonyms.

## Example

```sql
SET @gc = 'GeometryCollection(Point(1 1),LineString(12 14, 9 11))';

SELECT AsText(GeometryN(GeomFromText(@gc),1));
+----------------------------------------+
| AsText(GeometryN(GeomFromText(@gc),1)) |
+----------------------------------------+
| POINT(1 1)                             |
+----------------------------------------+
```