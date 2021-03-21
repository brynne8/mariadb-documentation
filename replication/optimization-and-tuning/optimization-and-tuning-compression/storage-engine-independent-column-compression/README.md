# Storage-Engine Independent Column Compression

##### MariaDB starting with [10.3.2](/kb/en/mariadb-1032-release-notes/)

Storage-engine independent support for column compression was introduced in [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/)

Storage-engine independent column compression enables [TINYBLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/tinyblob/), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/), [MEDIUMBLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/mediumblob/), [LONGBLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/longblob/), [TINYTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/tinytext/), [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/), [MEDIUMTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/mediumtext/), [LONGTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/longtext/), [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/) and [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/) columns to be compressed.

This is performed by means of a new COMPRESSED [column attribute](/kb/en/create-table/#column-and-index-definitions):
`COMPRESSED[=&lt;compression_method&gt;]`

Currently the only supported compression method is `zlib`.

### Field Length Compatibility

When using the `COMPRESSED` attribute, note that FIELD LENGTH is reduced by 1; for example, a BLOB has a length of 65535, while BLOB COMPRESSED has 65535-1. See [MDEV-15592](https://jira.mariadb.org/browse/MDEV-15592).

### New System Variables

#### `column_compression_threshold`

- <strong>Description:</strong> Minimum column data length eligible for compression.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--column-compression-threshold=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `100`
- <strong>Range:</strong> `0` to `4294967295`

---

#### `column_compression_zlib_level`

- <strong>Description:</strong> zlib compression level (1 gives best speed, 9 gives best compression).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--column-compression-zlib-level=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `6`
- <strong>Range:</strong> `1` to `9`

---

#### `column_compression_zlib_strategy`

- <strong>Description:</strong> The strategy parameter is used to tune the compression algorithm. Use the value `DEFAULT_STRATEGY` for normal data, `FILTERED` for data produced by a filter (or predictor), `HUFFMAN_ONLY` to force Huffman encoding only (no string match), or `RLE` to limit match distances to one (run-length encoding). Filtered data consists mostly of small values with a somewhat random distribution. In this case, the compression algorithm is tuned to compress them better. The effect of `FILTERED` is to force more Huffman coding and less string matching; it is somewhat intermediate between `DEFAULT_STRATEGY` and `HUFFMAN_ONLY`. `RLE` is designed to be almost as fast as `HUFFMAN_ONLY`, but give better compression for PNG image data. The strategy parameter only affects the compression ratio but not the correctness of the compressed output even if it is not set appropriately. `FIXED` prevents the use of dynamic Huffman codes, allowing for a simpler decoder for special applications.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--column-compression-zlib-strategy=#</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `enum`
- <strong>Default Value:</strong> `DEFAULT_STRATEGY`
- <strong>Valid Values:</strong> `DEFAULT_STRATEGY`, `FILTERED`, `HUFFMAN_ONLY`, `RLE`, `FIXED`

---

#### `column_compression_zlib_wrap`

- <strong>Description:</strong> If set to `1` (`0` is default), generate zlib header and trailer and compute adler32 check value. It can be used with storage engines that don't provide data integrity verification to detect data corruption.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--column-compression-zlib-wrap{=0|1}</code>
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

### New Status Variables

#### `Column_compressions`

- <strong>Description:</strong> Incremented each time field data is compressed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Column_decompressions`

- <strong>Description:</strong> Incremented each time field data is decompressed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

### Limitations

- The only supported method currently is zlib.
- The [CSV](/columns-storage-engines-and-plugins/storage-engines/csv/) storage engine stores data uncompressed on-disk even if the COMPRESSED attribute is present.
- It is not possible to create indexes over compressed columns.

### Comparison with InnoDB Page Compression

Storage-independent column compression is different to [InnoDB Page Compression](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-compression/) in a number of ways.

- It is storage engine independent, while InnoDB page compression applies to InnoDB only.
- By being specific to a column, one can access non-compressed fields without the decompression overhead.
- Only zlib is available, while InnoDB page compression can offer alternative compression algorithms.
- It is not recommended to use multiple forms of compression over the same data.
- It is intended for compressing large blobs, while InnoDB page compression is suitable for a more general case.
- Columns cannot be indexed, while with InnoDB page compression indexes are possible as usual.

### Examples

```sql
CREATE TABLE cmp (i TEXT COMPRESSED);

CREATE TABLE cmp2 (i TEXT COMPRESSED=zlib);
```

### See Also

- [InnoDB Page Compression](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-compression/)
- [InnoDB Compressed Row Format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/)