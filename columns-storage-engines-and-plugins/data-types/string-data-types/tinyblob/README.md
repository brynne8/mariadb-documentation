# TINYBLOB

## Syntax

```sql
TINYBLOB
```

## Description

A [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) column with a maximum length of 
255 (2<sup>8</sup> - 1) bytes. Each
TINYBLOB value is stored using a one-byte length prefix that indicates
the number of bytes in the value.

## See Also

- [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/)
- [BLOB and TEXT Data Types](/columns-storage-engines-and-plugins/data-types/string-data-types/blob-and-text-data-types/)
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements/)