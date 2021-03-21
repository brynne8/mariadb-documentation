# MariaDB Plans - GIS

<strong>Note:</strong> This page is obsolete. The information is old, outdated, or otherwise currently incorrect. We are keeping the page for historical reasons only. <strong>Do not</strong> rely on the information in this article.

OpenGIS compliance:

- create required tables: GeometryTables, GeometryColumns, related views.
- stored procedure AddGeometryColumn
- prefill the spatial_ref_sys table.

Optimizer:

- optimize simple queries with Intersects(), Within, Distance()&lt;X
- add Distance_sphere() and the related optimization.

Add possible III-rd coordinate (Attitude).

- Distance3D, related optimization.

Precise math coordinates instead of DOUBLE-s.