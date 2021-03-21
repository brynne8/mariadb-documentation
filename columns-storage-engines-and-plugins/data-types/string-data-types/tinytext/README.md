# TINYTEXT

## Syntax

```sql
TINYTEXT [CHARACTER SET charset_name] [COLLATE collation_name]
```

## Description

A [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) column with a maximum length of 255 (`2<sup>8</sup> - 1`) characters. The effective maximum length is less if the value contains multi-byte characters. Each TINYTEXT value is stored using a one-byte length prefix that indicates the number of bytes in the value.

## See Also

- [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/)
- [BLOB and TEXT Data Types](/columns-storage-engines-and-plugins/data-types/string-data-types/blob-and-text-data-types/)
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements/)