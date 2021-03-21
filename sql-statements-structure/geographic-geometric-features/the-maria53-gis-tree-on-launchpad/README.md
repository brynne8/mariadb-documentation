# The maria/5.3-gis tree on Launchpad.

<strong>Note:</strong> This page is obsolete. The information is old, outdated, or otherwise currently incorrect. We are keeping the page for historical reasons only. <strong>Do not</strong> rely on the information in this article.

The maria/5.3-gis tree on Launchpad.

Basic information about the existing spatial features can be found in the
[Geographic Features](/kb/en/geographic-features/) section of the Knowlegebase. The
[Spatial Extensions page of the MySQL manual](http://dev.mysql.com/doc/refman/5.6/en/spatial-extensions.html)
also applies to MariaDB.

The [maria/5.3-gis tree](https://code.launchpad.net/~maria-captains/maria/5.3-gis), contains recent code improving the spatial functionality in MariaDB.

MySQL operates on spatial data based on the OpenGIS standards, particularly the
[OpenGIS SFS](http://www.opengeospatial.org/standards/sfs) (Simple feature
access, SQL option).

Initial support was based on version 05-134 of the standard. MariaDB implements
a subset of the 'SQL with Geometry Types' environment proposed by the OGC.  And
the SQL environment was extended with a set of geometry types.

Now we have started to implement the newer 06-104r4 standard.

MariaDB supports spatial extensions to operate on spatial features.
These features are available for Aria, MyISAM, InnoDB, NDB, and ARCHIVE tables.

For spatial columns, Aria and MyISAM supports both SPATIAL and non-SPATIAL
indexes.  Other storage engines support non-SPATIAL indexes.

The most recent changes in the code are aimed at meeting the OpenGIS
requirements. One thing missing in the present version is that the functions
which check spatial relations don't consider the actual shape of an object,
instead they operate only on their bounding rectangles. These legacy functions
have been left as they are, and new, properly-working functions are named with
an '`ST_`' prefix, in accordance with the last OpenGIS requirements. Also,
operations over geometry features were added.

The list of new functions:

Spatial operators. They produce new geometries.

<table><tbody><tr><th>Name</th><th>Description</th></tr>
<tr><td><code>ST_UNION(A, B)</code></td><td>union of A and B</td></tr>
<tr><td><code>ST_INTERSECTION(A, B)</code></td><td>intersection of A and B</td></tr>
<tr><td><code>ST_SYMDIFFERENCE(A, B)</code></td><td>symdifference, notintersecting parts of A and B</td></tr>
<tr><td><code>ST_BUFFER(A, radius)</code></td><td>returns the shape of the area that lies in 'radius' distance from the shape A.</td></tr>
</tbody></table>

Predicates, return boolean result of the relationship

<table><tbody><tr><th>Name</th><th>Description</th></tr>
<tr><td><code>ST_INTERSECTS(A, B)</code></td><td>if A and B have an intersection</td></tr>
<tr><td><code>ST_CROSSES(A, B)</code></td><td>if A and B cross</td></tr>
<tr><td><code>ST_EQUAL(A, B)</code></td><td>if A nad B are equal</td></tr>
<tr><td><code>ST_WITHIN(A, B)</code></td><td>if A lies within B</td></tr>
<tr><td><code>ST_CONTAINS(A,B)</code></td><td>if B lies within A</td></tr>
<tr><td><code>ST_DISJOINT(A,B)</code></td><td>if A and B have no intersection</td></tr>
<tr><td><code>ST_TOUCHES(A,B)</code></td><td>if A touches B</td></tr>
</tbody></table>

Geometry metadata views:

<table><tbody><tr><th><code>GEOMETRY_COLUMNS</code></th><td><em>this table describes the available feature tables and their Geometry properties</em></td></tr>
<tr><th>fields:</th><td><code>F_TABLE_CATALOG VARCHAR(200) NOT NULL,</code></td></tr>
<tr><th></th><td><code>F_TABLE_SCHEMA VARCHAR(200) NOT NULL,</code></td></tr>
<tr><th></th><td><code>F_TABLE_NAME VARCHAR(200) NOT NULL,</code></td></tr>
<tr><th></th><td><code>F_GEOMETRY_COLUMN VARCHAR(200) NOT NULL,</code></td></tr>
<tr><th></th><td><code>G_TABLE_CATALOG VARCHAR(200) NOT NULL,</code></td></tr>
<tr><th></th><td><code>G_TABLE_SCHEMA VARCHAR(200) NOT NULL,</code></td></tr>
<tr><th></th><td><code>G_TABLE_NAME VARCHAR(200) NOT NULL,</code></td></tr>
<tr><th></th><td><code>STORAGE_TYPE INTEGER,</code></td></tr>
<tr><th></th><td><code>GEOMETRY_TYPE INTEGER,</code></td></tr>
<tr><th></th><td><code>COORD_DIMENSION INTEGER,</code></td></tr>
<tr><th></th><td><code>MAX_PPR INTEGER,</code></td></tr>
<tr><th></th><td><code>SRID INTEGER NOT NULL</code></td></tr>
<tr><td></td><td></td></tr>
<tr><td></td><td></td></tr>
<tr><th>SPATIAL_REF_SYS</th><td><em>this table describes the coordinate system and transformations for Geometry</em></td></tr>
<tr><th>fields:</th><td><code>SRID INTEGER NOT NULL PRIMARY KEY,</code></td></tr>
<tr><th></th><td><code>AUTH_NAME VARCHAR(200),</code></td></tr>
<tr><th></th><td><code>AUTH_SRID INTEGER,</code></td></tr>
<tr><th></th><td><code>SRTEXT TEXT</code></td></tr>
</tbody></table>