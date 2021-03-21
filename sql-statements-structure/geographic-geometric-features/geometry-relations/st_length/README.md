# ST_LENGTH

## Syntax

```sql
ST_LENGTH(ls)
```

## Description

Returns as a double-precision number the length of the
[LineString](/sql-statements-structure/geographic-geometric-features/geometry-constructors/linestring) value <em>`ls`</em> in its associated spatial reference.

## Examples

```sql
SET @ls = 'LineString(1 1,2 2,3 3)';

SELECT ST_LENGTH(ST_GeomFromText(@ls));
+---------------------------------+
| ST_LENGTH(ST_GeomFromText(@ls)) |
+---------------------------------+
|                2.82842712474619 |
+---------------------------------+
```