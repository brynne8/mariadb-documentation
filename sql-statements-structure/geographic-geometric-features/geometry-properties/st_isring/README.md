# ST_IsRing

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

The ST_IsRing function was introduced in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

## Syntax

```sql
ST_IsRing(g)
IsRing(g)
```

## Description

Returns true if a given [LINESTRING](/sql-statements-structure/geographic-geometric-features/geometry-constructors/linestring) is a ring, that is, both [ST_IsClosed](/sql-statements-structure/geographic-geometric-features/geometry-properties/st_isclosed) and [ST_IsSimple](/sql-statements-structure/geographic-geometric-features/geometry-properties/st_issimple). A simple curve does not pass through the same point more than once.  However, see [MDEV-7510](https://jira.mariadb.org/browse/MDEV-7510).

`St_IsRing()` and `IsRing()` are synonyms.