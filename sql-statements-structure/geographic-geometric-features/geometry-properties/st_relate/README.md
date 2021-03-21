# ST_RELATE

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

The ST_RELATE() function was introduced in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

## Syntax

```sql
ST_Relate(g1, g2, i)
```

## Description

Returns true if Geometry `g1` is spatially related to Geometry`g2` by testing for intersections between the interior, boundary and exterior of the two geometries as specified by the values in intersection matrix pattern `i`.