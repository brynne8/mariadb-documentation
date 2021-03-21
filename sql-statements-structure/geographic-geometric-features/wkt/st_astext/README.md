# ST_AsText

## Syntax

```sql
ST_AsText(g)
AsText(g)
ST_AsWKT(g)
AsWKT(g)
```

## Description

Converts a value in internal geometry format to its [WKT](/sql-statements-structure/geographic-geometric-features/wkt/wkt-definition/) representation and returns the string result.

`ST_AsText()`, `AsText()`, `ST_AsWKT()` and `AsWKT()` are all synonyms.

## Examples

```sql
SET @g = 'LineString(1 1,4 4,6 6)';

SELECT ST_AsText(ST_GeomFromText(@g));
+--------------------------------+
| ST_AsText(ST_GeomFromText(@g)) |
+--------------------------------+
| LINESTRING(1 1,4 4,6 6)        |
+--------------------------------+
```