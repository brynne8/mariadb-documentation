# GIS features in 5.3.3

Basic information about the existing spatial features can be found in the
[Geographic Features](/kb/en/geographic-features/) section of the Knowlegebase. The
[Spatial Extensions page of the MySQL manual](http://dev.mysql.com/doc/refman/5.6/en/spatial-extensions.html)
also applies to MariaDB.

The [MariaDB 5.3.3](/kb/en/mariadb-533-release-notes/) release , contains code improving
the spatial functionality in MariaDB.

MySQL operates on spatial data based on the OpenGIS standards, particularly the
[OpenGIS SFS](http://www.opengeospatial.org/standards/sfs) (Simple feature
access, SQL option).

Initial support was based on version 05-134 of the standard. MariaDB implements
a subset of the 'SQL with Geometry Types' environment proposed by the OGC.  And
the SQL environment was extended with a set of geometry types.

MariaDB supports spatial extensions to operate on spatial features.
These features are available for [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/), [MyISAM](/kb/en/myisam/), [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/), NDB, and [ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive/) tables.

For spatial columns, Aria and MyISAM supports both [SPATIAL](/kb/en/spatial-indexes/) and non-SPATIAL
indexes.  Other storage engines support non-SPATIAL indexes.

The most recent changes in the code are aimed at meeting the OpenGIS
requirements. One thing missed in previous versions is that the functions
which check spatial relations didn't consider the actual shape of an object,
instead they operate only on their bounding rectangles. These legacy functions
have been left as they are and new, properly-working functions are named with
an '`ST_`' prefix, in accordance with the latest OpenGIS requirements. Also,
operations over geometry features were added.

The list of new functions:

Spatial operators. They produce new geometries.

<table><tbody><tr><th>Name</th><th>Description</th></tr>
<tr><td><a href="/kb/en/st_union/">ST_UNION(A, B)</a></td><td>union of A and B</td></tr>
<tr><td><a href="/kb/en/st_intersection/">ST_INTERSECTION(A, B)</a></td><td>intersection of A and B</td></tr>
<tr><td><a href="/kb/en/st_symdifference/">ST_SYMDIFFERENCE(A, B)</a></td><td>symdifference, notintersecting parts of A and B</td></tr>
<tr><td><a href="/kb/en/st_buffer/">ST_BUFFER(A, radius)</a></td><td>returns the shape of the area that lies in 'radius' distance from the shape A.</td></tr>
</tbody></table>

Predicates, return boolean result of the relationship

<table><tbody><tr><th>Name</th><th>Description</th></tr>
<tr><td><a href="/kb/en/st_intersects/">ST_INTERSECTS(A, B)</a></td><td>if A and B have an intersection</td></tr>
<tr><td><a href="/kb/en/st_crosses/">ST_CROSSES(A, B)</a></td><td>if A and B cross</td></tr>
<tr><td><a href="/kb/en/st_equals/">ST_EQUALS(A, B)</a></td><td>if A and B are equal</td></tr>
<tr><td><a href="/kb/en/st_within/">ST_WITHIN(A, B)</a></td><td>if A lies within B</td></tr>
<tr><td><a href="/kb/en/st_contains/">ST_CONTAINS(A,B)</a></td><td>if B lies within A</td></tr>
<tr><td><a href="/kb/en/st_disjoint/">ST_DISJOINT(A,B)</a></td><td>if A and B have no intersection</td></tr>
<tr><td><a href="/kb/en/st_touches/">ST_TOUCHES(A,B)</a></td><td>if A touches B</td></tr>
</tbody></table>