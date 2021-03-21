# ST_CONTAINS

## Syntax

```sql
ST_CONTAINS(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether a geometry `g1` completely contains geometry `g2`.

ST_CONTAINS() uses object shapes, while [CONTAINS()](/sql-statements-structure/geographic-geometric-features/geometry-relations/contains), based on the original MySQL implementation, uses object bounding rectangles.

ST_CONTAINS tests the opposite relationship to [ST_WITHIN()](/kb/en/st_within/).

## Examples

```sql
SET @g1 = ST_GEOMFROMTEXT('POLYGON((175 150, 20 40, 50 60, 125 100, 175 150))');

SET @g2 = ST_GEOMFROMTEXT('POINT(174 149)');

SELECT ST_CONTAINS(@g1,@g2);
+----------------------+
| ST_CONTAINS(@g1,@g2) |
+----------------------+
|                    1 |
+----------------------+

SET @g2 = ST_GEOMFROMTEXT('POINT(175 151)');

SELECT ST_CONTAINS(@g1,@g2);
+----------------------+
| ST_CONTAINS(@g1,@g2) |
+----------------------+
|                    0 |
+----------------------+
```