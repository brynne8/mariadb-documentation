# ST_GeomFromGeoJSON

##### MariaDB starting with [10.2.4](/kb/en/mariadb-1024-release-notes/)

ST_GeomFromGeoJSON was added in [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

## Syntax

```sql
ST_GeomFromGeoJSON(g[, option])
```

## Description

Given a GeoJSON input <em>g</em>, returns a geometry object. The <em>option</em> specifies what to do if <em>g</em> contains geometries with coordinate dimensions higher than 2.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td>1</td><td>Return an error (the default)</td></tr>
<tr><td>2 - 4</td><td>The document is accepted, but the coordinates for higher coordinate dimensions are stripped off.</td></tr>
</tbody></table>

Note that this function did not work correctly before [MariaDB 10.2.8](/kb/en/mariadb-1028-release-notes/) - see [MDEV-12180](https://jira.mariadb.org/browse/MDEV-12180).

## Examples

```sql
SET @j = '{ "type": "Point", "coordinates": [5.3, 15.0]}';

SELECT ST_AsText(ST_GeomFromGeoJSON(@j));
+-----------------------------------+
| ST_AsText(ST_GeomFromGeoJSON(@j)) |
+-----------------------------------+
| POINT(5.3 15)                     |
+-----------------------------------+
```