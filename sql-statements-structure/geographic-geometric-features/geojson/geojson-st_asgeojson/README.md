# ST_AsGeoJSON

##### MariaDB starting with [10.2.4](/kb/en/mariadb-1024-release-notes/)

ST_AsGeoJSON was added in [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

## Syntax

```sql
ST_AsGeoJSON(g[, max_decimals[, options]])
```

## Description

Returns the given geometry <em>g</em> as a GeoJSON element. The optional <em>max_decimals</em> limits the maximum number of decimals displayed.

The optional <em>options</em> flag can be set to `1` to add a bounding box to the output.

Note that this function did not work correctly before [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/) - see [MDEV-12181](https://jira.mariadb.org/browse/MDEV-12181).

## Examples

```sql
SELECT ST_AsGeoJSON(ST_GeomFromText('POINT(5.3 7.2)'));
+-------------------------------------------------+
| ST_AsGeoJSON(ST_GeomFromText('POINT(5.3 7.2)')) |
+-------------------------------------------------+
| {"type": "Point", "coordinates": [5.3, 7.2]}    |
+-------------------------------------------------+
```

## See also

- [ST_GeomFromGeoJSON](/sql-statements-structure/geographic-geometric-features/geojson/st_geomfromgeojson/)