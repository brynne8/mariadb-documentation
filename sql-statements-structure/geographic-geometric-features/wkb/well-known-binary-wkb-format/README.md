# Well-Known Binary (WKB) Format

WKB stands for Well-Known Binary, a format for representing geographical and geometrical data.

WKB uses 1-byte unsigned integers, 4-byte unsigned integers, and 8-byte double-precision numbers.

- The first byte indicates the byte order. 00 for big endian, or 01 for little endian.
- The next 4 bytes indicate the geometry type. Values from 1 to 7 indicate whether the type is Point, LineString, Polygon, MultiPoint, MultiLineString, MultiPolygon, or GeometryCollection respectively.
- The 8-byte floats represent the co-ordinates.

Take the following example, a sequence of 21 bytes each represented by two hex digits:

```sql
000000000140000000000000004010000000000000
```

- It's big endian
<ul><li>00<strong>0000000140000000000000004010000000000000</strong>
</li></ul>
- It's a POINT
<ul><li>00<strong>00000001</strong>40000000000000004010000000000000
</li></ul>
- The X co-ordinate is 2.0
<ul><li>0000000001<strong>4000000000000000</strong>4010000000000000
</li></ul>
- The Y-co-ordinate is 4.0
<ul><li>00000000014000000000000000<strong>4010000000000000</strong></li></ul>