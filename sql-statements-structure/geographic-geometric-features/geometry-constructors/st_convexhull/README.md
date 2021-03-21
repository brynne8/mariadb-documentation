# ST_CONVEXHULL

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

ST_ConvexHull() was introduced in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

## Syntax

```sql
ST_ConvexHull(g)
ConvexHull(g)
```

## Description

Given a geometry, returns a geometry that is the minimum convex geometry enclosing all geometries within the set. Returns NULL if the geometry value is NULL or an empty value.

ST_ConvexHull() and ConvexHull() are synonyms.

## Examples

The ConvexHull of a single point is simply the single point:

```sql
SET @g = ST_GEOMFROMTEXT('Point(0 0)');

SELECT ST_ASTEXT(ST_CONVEXHULL(@g));
+------------------------------+
| ST_ASTEXT(ST_CONVEXHULL(@g)) |
+------------------------------+
| POINT(0 0)                   |
+------------------------------+
```

```sql
SET @g = ST_GEOMFROMTEXT('MultiPoint(0 0, 1 2, 2 3)');

SELECT ST_ASTEXT(ST_CONVEXHULL(@g));
+------------------------------+
| ST_ASTEXT(ST_CONVEXHULL(@g)) |
+------------------------------+
| POLYGON((0 0,1 2,2 3,0 0))   |
+------------------------------+
```

```sql
SET @g = ST_GEOMFROMTEXT('MultiPoint( 1 1, 2 2, 5 3, 7 2, 9 3, 8 4, 6 6, 6 9, 4 9, 1 5 )');

SELECT ST_ASTEXT(ST_CONVEXHULL(@g));
+----------------------------------------+
| ST_ASTEXT(ST_CONVEXHULL(@g))           |
+----------------------------------------+
| POLYGON((1 1,1 5,4 9,6 9,9 3,7 2,1 1)) |
+----------------------------------------+
```