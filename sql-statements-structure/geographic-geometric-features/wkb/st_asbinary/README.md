# ST_AsBinary

## Syntax

```sql
ST_AsBinary(g)
AsBinary(g)
ST_AsWKB(g)
AsWKB(g)
```

## Description

Converts a value in internal geometry format to its [WKB](/sql-statements-structure/geographic-geometric-features/wkb/well-known-binary-wkb-format) representation and returns the binary result.

`ST_AsBinary()`, `AsBinary()`, `ST_AsWKB()` and `AsWKB()` are synonyms,

## Examples

```sql
SET @poly = ST_GeomFromText('POLYGON((0 0,0 1,1 1,1 0,0 0))');
SELECT ST_AsBinary(@poly);

SELECT ST_AsText(ST_GeomFromWKB(ST_AsWKB(@poly)));
+--------------------------------------------+
| ST_AsText(ST_GeomFromWKB(ST_AsWKB(@poly))) |
+--------------------------------------------+
| POLYGON((0 0,0 1,1 1,1 0,0 0))             |
+--------------------------------------------+
```