# ST_ENVELOPE

## Syntax

```sql
ST_ENVELOPE(g)
ENVELOPE(g)
```

## Description

Returns the Minimum Bounding Rectangle (MBR) for the geometry value `g`.  The result is returned as a Polygon value.

The polygon is defined by the corner points of the bounding box:

```sql
POLYGON((MINX MINY, MAXX MINY, MAXX MAXY, MINX MAXY, MINX MINY))
```

`ST_ENVELOPE()` and `ENVELOPE()` are synonyms.

## Examples

```sql
SELECT AsText(ST_ENVELOPE(GeomFromText('LineString(1 1,4 4)')));
+----------------------------------------------------------+
| AsText(ST_ENVELOPE(GeomFromText('LineString(1 1,4 4)'))) |
+----------------------------------------------------------+
| POLYGON((1 1,4 1,4 4,1 4,1 1))                           |
+----------------------------------------------------------+
```