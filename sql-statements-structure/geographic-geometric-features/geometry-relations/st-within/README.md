# ST_WITHIN

## Syntax

```sql
ST_WITHIN(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether geometry <em>`g1`</em> is spatially within geometry <em>`g2`</em>.

This tests the opposite relationship as <a undefined>ST_CONTAINS()</a>.

ST_WITHIN() uses object shapes, while [WITHIN()](/sql-statements-structure/geographic-geometric-features/geometry-relations/within/), based on the original MySQL implementation, uses object bounding rectangles.

## Examples

```sql
SET @g1 = ST_GEOMFROMTEXT('POINT(174 149)');

SET @g2 = ST_GEOMFROMTEXT('POLYGON((175 150, 20 40, 50 60, 125 100, 175 150))');

SELECT ST_WITHIN(@g1,@g2);
+--------------------+
| ST_WITHIN(@g1,@g2) |
+--------------------+
|                  1 |
+--------------------+

SET @g1 = ST_GEOMFROMTEXT('POINT(176 151)');

SELECT ST_WITHIN(@g1,@g2);
+--------------------+
| ST_WITHIN(@g1,@g2) |
+--------------------+
|                  0 |
+--------------------+
```