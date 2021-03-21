# MBRWithin

## Syntax

```sql
MBRWithin(g1,g2)
```

## Description

Returns 1 or 0 to indicate whether the Minimum Bounding Rectangle of
g1 is within the Minimum Bounding Rectangle of g2. This tests the
opposite relationship as [MBRContains()](/sql-statements-structure/geographic-geometric-features/mbr-minimum-bounding-rectangle/mbrcontains/).

## Examples

```sql
SET @g1 = GeomFromText('Polygon((0 0,0 3,3 3,3 0,0 0))');
SET @g2 = GeomFromText('Polygon((0 0,0 5,5 5,5 0,0 0))');
SELECT MBRWithin(@g1,@g2), MBRWithin(@g2,@g1);
+--------------------+--------------------+
| MBRWithin(@g1,@g2) | MBRWithin(@g2,@g1) |
+--------------------+--------------------+
|                  1 |                  0 |
+--------------------+--------------------+
```