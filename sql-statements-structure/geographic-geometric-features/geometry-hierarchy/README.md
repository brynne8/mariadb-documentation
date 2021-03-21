# Geometry Hierarchy

## Description

Geometry is the base class. It is an abstract class. The instantiable
subclasses of Geometry are restricted to zero-, one-, and
two-dimensional geometric objects that exist in two-dimensional
coordinate space. All instantiable geometry classes are defined so
that valid instances of a geometry class are topologically closed
(that is, all defined geometries include their boundary).

The base Geometry class has subclasses for Point, Curve, Surface, and
GeometryCollection:

- [Point](/sql-statements-structure/geographic-geometric-features/geometry-constructors/point) represents zero-dimensional objects.
- Curve represents one-dimensional objects, and has subclass [LineString](/sql-statements-structure/geographic-geometric-features/geometry-constructors/linestring), with sub-subclasses Line and LinearRing.
- Surface is designed for two-dimensional objects and has subclass [Polygon](/sql-statements-structure/geographic-geometric-features/geometry-constructors/polygon).
- [GeometryCollection](/sql-statements-structure/geographic-geometric-features/geometry-constructors/geometrycollection) has specialized zero-, one-, and two-dimensional collection classes named [MultiPoint](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multipoint), [MultiLineString](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multilinestring), and [MultiPolygon](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multipolygon) for modeling geometries corresponding to collections of Points, LineStrings, and Polygons, respectively. MultiCurve and MultiSurface are introduced as abstract superclasses that generalize the collection interfaces to handle Curves and Surfaces.

Geometry, Curve, Surface, MultiCurve, and MultiSurface are defined as
non-instantiable classes. They define a common set of methods for
their subclasses and are included for extensibility.

[Point](/sql-statements-structure/geographic-geometric-features/point-properties), [LineString](/sql-statements-structure/geographic-geometric-features/linestring-properties), [Polygon](/sql-statements-structure/geographic-geometric-features/polygon-properties), [GeometryCollection](/sql-statements-structure/geographic-geometric-features/geometry-constructors/geometrycollection), [MultiPoint](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multipoint),
[MultiLineString](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multilinestring), and [MultiPolygon](/sql-statements-structure/geographic-geometric-features/geometry-constructors/multipolygon) are instantiable classes.