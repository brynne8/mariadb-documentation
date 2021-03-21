# MULTIPOINT

## Syntax

```sql
MultiPoint(pt1,pt2,...)
```

## Description

Constructs a [WKB](/sql-statements-structure/geographic-geometric-features/wkb) MultiPoint value using WKB [Point](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point) arguments. If any argument is not a WKB Point, the return value is `NULL`.

## Examples

```sql
SET @g = ST_GEOMFROMTEXT('MultiPoint( 1 1, 2 2, 5 3, 7 2, 9 3, 8 4, 6 6, 6 9, 4 9, 1 5 )');

CREATE TABLE gis_multi_point (g MULTIPOINT);
INSERT INTO gis_multi_point VALUES
    (MultiPointFromText('MULTIPOINT(0 0,10 10,10 20,20 20)')),
    (MPointFromText('MULTIPOINT(1 1,11 11,11 21,21 21)')),
    (MPointFromWKB(AsWKB(MultiPoint(Point(3, 6), Point(4, 10)))));
```