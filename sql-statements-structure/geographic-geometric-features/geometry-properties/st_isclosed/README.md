# ST_ISCLOSED

## Syntax

```sql
ST_IsClosed(g)
IsClosed(g)
```

## Description

Returns 1 if a given [LINESTRING's](/sql-statements-structure/geographic-geometric-features/geometry-constructors/linestring/) start and end points are the same, or 0 if they are not the same. Before [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/), returns NULL if not given a LINESTRING. After [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/), returns -1.

`ST_IsClosed()` and `IsClosed()` are synonyms.

## Examples

```sql
SET @ls = 'LineString(0 0, 0 4, 4 4, 0 0)';
SELECT ST_ISCLOSED(GEOMFROMTEXT(@ls));
+--------------------------------+
| ST_ISCLOSED(GEOMFROMTEXT(@ls)) |
+--------------------------------+
|                              1 |
+--------------------------------+

SET @ls = 'LineString(0 0, 0 4, 4 4, 0 1)';
SELECT ST_ISCLOSED(GEOMFROMTEXT(@ls));
+--------------------------------+
| ST_ISCLOSED(GEOMFROMTEXT(@ls)) |
+--------------------------------+
|                              0 |
+--------------------------------+
```