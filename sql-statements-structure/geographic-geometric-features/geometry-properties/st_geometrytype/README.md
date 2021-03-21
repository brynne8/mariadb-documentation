# ST_GEOMETRYTYPE

## Syntax

```sql
ST_GeometryType(g)
GeometryType(g)
```

## Description

Returns as a string the name of the geometry type of which the
geometry instance `g` is a member. The name corresponds to one of the
instantiable Geometry subclasses.

`ST_GeometryType()` and `GeometryType()` are synonyms.

## Examples

```sql
SELECT GeometryType(GeomFromText('POINT(1 1)'));
+------------------------------------------+
| GeometryType(GeomFromText('POINT(1 1)')) |
+------------------------------------------+
| POINT                                    |
+------------------------------------------+
```