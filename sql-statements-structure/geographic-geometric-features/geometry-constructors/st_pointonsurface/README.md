# ST_POINTONSURFACE

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

ST_POINTONSURFACE() was introduced in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

## Syntax

```sql
ST_PointOnSurface(g)
PointOnSurface(g)
```

## Description

Given a geometry, returns a [POINT](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point/) guaranteed to intersect a surface. However, see [MDEV-7514](https://jira.mariadb.org/browse/MDEV-7514).

ST_PointOnSurface() and PointOnSurface() are synonyms.