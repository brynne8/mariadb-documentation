# TEXT

## Syntax

```sql
TEXT[(M)] [CHARACTER SET charset_name] [COLLATE collation_name]
```

## Description

A `TEXT` column with a maximum length of `65,535` (`2<sup>16</sup> - 1`)
characters. The effective maximum length is less if the value contains
multi-byte characters. Each `TEXT` value is stored using a two-byte length
prefix that indicates the number of bytes in the value.  If you need a bigger storage, consider using [MEDIUMTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/mediumtext) instead.

An optional length `M` can be given for this type. If this is done, MariaDB
creates the column as the smallest `TEXT` type large enough to hold values
`M` characters long.

Before [MariaDB 10.2](/kb/en/what-is-mariadb-102/), all MariaDB [collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets) were of type PADSPACE, meaning that TEXT (as well as [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar) and [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char) values) are compared without regard for trailing spaces. This does not apply to the [LIKE](/built-in-functions/string-functions/like) pattern-matching operator, which takes into account trailing spaces.

Before [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/), `BLOB` and `TEXT` columns could not be assigned a [DEFAULT](/kb/en/create-table/#default) value. This restriction was lifted in [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/).

## Examples

Trailing spaces:

```sql
CREATE TABLE strtest (d TEXT(10));
INSERT INTO strtest VALUES('Maria   ');

SELECT d='Maria',d='Maria   ' FROM strtest;
+-----------+--------------+
| d='Maria' | d='Maria   ' |
+-----------+--------------+
|         1 |            1 |
+-----------+--------------+

SELECT d LIKE 'Maria',d LIKE 'Maria   ' FROM strtest;
+----------------+-------------------+
| d LIKE 'Maria' | d LIKE 'Maria   ' |
+----------------+-------------------+
|              0 |                 1 |
+----------------+-------------------+
```

## Difference between [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar) and TEXT

- [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar) columns can be fully indexed. `TEXT` columns can only be indexed over a specified length.
- Using TEXT or [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob) in a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) query that uses temporary tables for storing intermediate results will force the temporary table to be disk based (using the [Aria storage engine](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine) instead of the [memory storage engine](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine), which is a bit slower. This is not that bad as the [Aria storage engine](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine) caches the rows in memory. To get the benefit of this, one should ensure that the [aria_pagecache_buffer_size](/kb/en/aria-system-variables/#aria_pagecache_buffer_size) variable is big enough to hold most of the row and index data for temporary tables.

### For Storage Engine Developers

- Internally the full length of the [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar) column is allocated inside each TABLE objects record[] structure. As there are three such buffers, each open table will allocate 3 times max-length-to-store-varchar bytes of memory.
- `TEXT` and `BLOB` columns are stored with a pointer (4 or 8 bytes) + a 1-4 bytes length.  The `TEXT` data is only stored once. This means that internally `TEXT` uses less memory for each open table but instead has the additional overhead that each `TEXT` object needs to be allocated and freed for each row access (with some caching in between).

## See Also

- [BLOB and TEXT Data Types](/columns-storage-engines-and-plugins/data-types/string-data-types/blob-and-text-data-types)
- [MEDIUMTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/mediumtext)
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements)