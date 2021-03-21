# COMPRESS

## Syntax

```sql
COMPRESS(string_to_compress)
```

## Description

Compresses a string and returns the result as a binary string. This
function requires MariaDB to have been compiled with a compression
library such as zlib. Otherwise, the return value is always `NULL`. The
compressed string can be uncompressed with [UNCOMPRESS()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/uncompress).

The [have_compress](/kb/en/server-system-variables/#have_compress) server system variable indicates whether a compression library is present.

## Examples

```sql
SELECT LENGTH(COMPRESS(REPEAT('a',1000)));
+------------------------------------+
| LENGTH(COMPRESS(REPEAT('a',1000))) |
+------------------------------------+
|                                 21 |
+------------------------------------+

SELECT LENGTH(COMPRESS(''));
+----------------------+
| LENGTH(COMPRESS('')) |
+----------------------+
|                    0 |
+----------------------+

SELECT LENGTH(COMPRESS('a'));
+-----------------------+
| LENGTH(COMPRESS('a')) |
+-----------------------+
|                    13 |
+-----------------------+

SELECT LENGTH(COMPRESS(REPEAT('a',16)));
+----------------------------------+
| LENGTH(COMPRESS(REPEAT('a',16))) |
+----------------------------------+
|                               15 |
+----------------------------------+
```