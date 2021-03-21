# MyISAM Index Storage Space

Regular [MyISAM](/kb/en/myisam/) tables make use of [B-tree indexes](/kb/en/storage-engine-index-types/#b-tree-indexes).

String indexes are space-compressed, which reduces the size of [VARCHARs](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar) that don't use the full length, or a string that has trailing spaces. String indexes also make use of prefix-compression, where strings with identical prefixes are compressed.

Numeric indexes can also be prefix-compressed compressed if the [PACK_KEYS=1](/kb/en/create-table/#table-options) option is used. Regardless, the high byte is always stored first, which allows a reduced index size.

In the worst case, with no strings being space-compressed, the total index storage space will be (index_length+4)/0.67 per index.