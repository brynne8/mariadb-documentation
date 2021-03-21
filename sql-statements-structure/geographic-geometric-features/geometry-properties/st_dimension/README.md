# ST_DIMENSION

## Syntax

```sql
ST_Dimension(g)
Dimension(g)
```

## Description

Returns the inherent dimension of the geometry value <em>`g`</em>. The result can
be

<table><tbody><tr><th>Dimension</th><th>Definition</th></tr>
<tr><td><code>   -1</code></td><td>empty geometry</td></tr>
<tr><td><code>    0</code></td><td>geometry with no length or area</td></tr>
<tr><td><code>    1</code></td><td>geometry with no area but nonzero length</td></tr>
<tr><td><code>    2</code></td><td>geometry with nonzero area</td></tr>
</tbody></table>

`ST_Dimension()` and `Dimension()` are synonyms.

## Examples

```sql
SELECT Dimension(GeomFromText('LineString(1 1,2 2)'));
+------------------------------------------------+
| Dimension(GeomFromText('LineString(1 1,2 2)')) |
+------------------------------------------------+
|                                              1 |
+------------------------------------------------+
```