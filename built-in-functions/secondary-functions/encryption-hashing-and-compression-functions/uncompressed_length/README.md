# UNCOMPRESSED_LENGTH

## Syntax

```sql
UNCOMPRESSED_LENGTH(compressed_string)
```

## Description

Returns the length that the compressed string had before being
compressed with [COMPRESS()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/compress).

`UNCOMPRESSED_LENGTH()` returns `NULL` or an incorrect result if the string is not compressed.

Until [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), returns `MYSQL_TYPE_LONGLONG`, or [bigint(10)](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint), in all cases. From [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), returns `MYSQL_TYPE_LONG`, or [int(10)](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int), when the result would fit within 32-bits.

## Examples

```sql
SELECT UNCOMPRESSED_LENGTH(COMPRESS(REPEAT('a',30)));
+-----------------------------------------------+
| UNCOMPRESSED_LENGTH(COMPRESS(REPEAT('a',30))) |
+-----------------------------------------------+
|                                            30 |
+-----------------------------------------------+
```