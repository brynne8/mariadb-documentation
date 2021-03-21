# ST_CENTROID

## Syntax

```sql
ST_Centroid(mpoly)
Centroid(mpoly)
```

## Description

Returns a point reflecting the mathematical centroid (geometric center) for the [MultiPolygon](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multipolygon/) <em>mpoly</em>. The resulting point will not necessarily be on the MultiPolygon.

`ST_Centroid()` and `Centroid()` are synonyms.

## Examples

```sql
SET @poly = ST_GeomFromText('POLYGON((0 0,20 0,20 20,0 20,0 0))');
SELECT ST_AsText(ST_Centroid(@poly)) AS center;
+--------------+
| center       |
+--------------+
| POINT(10 10) |
+--------------+
```