# UNCOMPRESS

## Syntax

```sql
UNCOMPRESS(string_to_uncompress)
```

## Description

Uncompresses a string compressed by the [COMPRESS()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/compress/) function. If the
argument is not a compressed value, the result is `NULL`. This function
requires MariaDB to have been compiled with a compression library such
as zlib. Otherwise, the return value is always `NULL`. The [have_compress](/kb/en/server-system-variables/#have_compress) server system variable indicates whether a compression library is present.

## Examples

```sql
SELECT UNCOMPRESS(COMPRESS('a string'));
+----------------------------------+
| UNCOMPRESS(COMPRESS('a string')) |
+----------------------------------+
| a string                         |
+----------------------------------+

SELECT UNCOMPRESS('a string');
+------------------------+
| UNCOMPRESS('a string') |
+------------------------+
| NULL                   |
+------------------------+
```