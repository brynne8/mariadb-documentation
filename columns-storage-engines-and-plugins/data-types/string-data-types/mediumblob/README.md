# MEDIUMBLOB

## Syntax

```sql
MEDIUMBLOB
```

## Description

A [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob) column with a maximum
length of 16,777,215 (2<sup>24</sup> - 1) bytes.
Each MEDIUMBLOB value is stored using a three-byte length prefix that
indicates the number of bytes in the value.

## See Also

- [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob)
- [BLOB and TEXT Data Types](/columns-storage-engines-and-plugins/data-types/string-data-types/blob-and-text-data-types)
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements)