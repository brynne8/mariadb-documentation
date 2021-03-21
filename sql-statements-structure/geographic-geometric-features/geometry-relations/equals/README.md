# EQUALS

## Syntax

```sql
Equals(g1,g2)
```

From [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/):

```sql
MBREQUALS(g1,g2)
```

## Description

Returns `1` or `0` to indicate whether <em>`g1`</em> is spatially equal to <em>`g2`</em>.

EQUALS() is based on the original MySQL implementation and uses object bounding rectangles, while [ST_EQUALS()](/sql-statements-structure/geographic-geometric-features/geometry-relations/equals) uses object shapes.

From [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/), `MBREQUALS` is a synonym for `Equals`.