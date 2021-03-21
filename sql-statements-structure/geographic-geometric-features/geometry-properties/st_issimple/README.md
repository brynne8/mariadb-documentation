# ST_IsSimple

## Syntax

```sql
ST_IsSimple(g)
IsSimple(g)
```

## Description

Returns true if the given Geometry has no anomalous geometric points, false if it does, or NULL if given a NULL value.

ST_IsSimple() and IsSimple() are synonyms.

## Examples

A POINT is always simple.

```sql
SET @g = 'Point(1 2)';

SELECT ST_ISSIMPLE(GEOMFROMTEXT(@g));
+-------------------------------+
| ST_ISSIMPLE(GEOMFROMTEXT(@g)) |
+-------------------------------+
|                             1 |
+-------------------------------+
```