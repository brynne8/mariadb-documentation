# MULTILINESTRING

## Syntax

```sql
MultiLineString(ls1,ls2,...)
```

## Description

Constructs a WKB MultiLineString value using [WKB](/sql-statements-structure/geographic-geometric-features/wkb/) [LineString](/sql-statements-structure/geographic-geometric-features/geometry-constructors/linestring/) arguments.  If any argument is not a WKB LineString, the return value is
`NULL`.

## Example

```sql
CREATE TABLE gis_multi_line (g MULTILINESTRING);
INSERT INTO gis_multi_line VALUES
 (MultiLineStringFromText('MULTILINESTRING((10 48,10 21,10 0),(16 0,16 23,16 48))')),
 (MLineFromText('MULTILINESTRING((10 48,10 21,10 0))')),
 (MLineFromWKB(AsWKB(MultiLineString(LineString(Point(1, 2), Point(3, 5)), LineString(Point(2, 5),Point(5, 8),Point(21, 7))))));
```