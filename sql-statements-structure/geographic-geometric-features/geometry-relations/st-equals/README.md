# ST_EQUALS

## Syntax

```sql
ST_EQUALS(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether geometry <em>`g1`</em> is spatially equal to geometry <em>`g2`</em>.

ST_EQUALS() uses object shapes, while [EQUALS()](/sql-statements-structure/geographic-geometric-features/geometry-relations/equals), based on the original MySQL implementation, uses object bounding rectangles.

## Examples

```sql
SET @g1 = ST_GEOMFROMTEXT('LINESTRING(174 149, 176 151)');

SET @g2 = ST_GEOMFROMTEXT('LINESTRING(176 151, 174 149)');

SELECT ST_EQUALS(@g1,@g2);
+--------------------+
| ST_EQUALS(@g1,@g2) |
+--------------------+
|                  1 |
+--------------------+
```

```sql
SET @g1 = ST_GEOMFROMTEXT('POINT(0 2)');

SET @g1 = ST_GEOMFROMTEXT('POINT(2 0)');

SELECT ST_EQUALS(@g1,@g2);
+--------------------+
| ST_EQUALS(@g1,@g2) |
+--------------------+
|                  0 |
+--------------------+
```